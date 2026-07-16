# 技术方案：auto-dark-mode

> 基于 spec.md 的验收标准，描述 HOW

## 方案概述

在现有主题初始化逻辑前插入 `matchMedia` 检测，优先使用 localStorage 记录，无记录时回退到系统偏好；添加 `change` 事件监听实现实时响应。

## 技术选型

| 需求 | 方案 | 理由 | 替代方案 |
|------|------|------|----------|
| 系统主题检测 | `window.matchMedia('(prefers-color-scheme: dark)')` | 原生 API，零依赖，浏览器兼容性 95%+ | CSS `@media` alone（无法改变 JS 状态） |
| 实时响应系统变化 | `matchMedia.addEventListener('change', fn)` | 原生事件，无需轮询 | `setInterval` 检测（浪费性能） |
| 偏好优先级 | localStorage > matchMedia > 默认亮色 | 符合用户预期：手动选择 > 自动检测 | 始终跟随系统（忽略用户手动选择） |

## 实现步骤

### 步骤 1: 修改主题初始化逻辑

- 文件：`index.html#L1778-L1783`
- 操作：修改现有的 `if(localStorage...)` 初始化块，增加 matchMedia 回退
- 关键代码：
  ```javascript
  // 初始化主题（优先级：localStorage > 系统偏好 > 默认亮色）
  var savedTheme = localStorage.getItem('techinsight_theme');
  if (savedTheme) {
    currentTheme = savedTheme;
  } else if (window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches) {
    currentTheme = 'dark';
  } else {
    currentTheme = 'light';
  }
  document.documentElement.setAttribute('data-theme', currentTheme);
  var themeBtn = document.getElementById('themeToggle');
  if (themeBtn) themeBtn.textContent = currentTheme === 'dark' ? '🌙' : '☀️';
  ```

### 步骤 2: 添加系统主题变化监听（P1）

- 文件：`index.html`（初始化逻辑之后）
- 操作：新增 `matchMedia` change 事件监听器，仅在无 localStorage 偏好时响应
- 关键代码：
  ```javascript
  // 系统主题变化时实时响应（仅当用户未手动选择时）
  if (window.matchMedia) {
    var darkQuery = window.matchMedia('(prefers-color-scheme: dark)');
    darkQuery.addEventListener('change', function(e) {
      if (localStorage.getItem('techinsight_theme')) return; // 用户已手动选择，忽略
      currentTheme = e.matches ? 'dark' : 'light';
      document.documentElement.setAttribute('data-theme', currentTheme);
      var btn = document.getElementById('themeToggle');
      if (btn) btn.textContent = currentTheme === 'dark' ? '🌙' : '☀️';
      // 更新图表背景
      var bgColor = currentTheme === 'dark' ? 'transparent' : '#ffffff';
      Object.keys(chartInstances).forEach(function(key) {
        if (chartInstances[key]) chartInstances[key].setOption({backgroundColor: bgColor});
      });
    });
  }
  ```

### 步骤 3: 修改 toggleTheme 保持一致性

- 文件：`index.html#L1762-L1776`
- 操作：无需修改。现有 `toggleTheme()` 已正确保存到 localStorage，手动切换后 change 监听器会因 `localStorage.getItem()` 有值而跳过，符合"手动选择优先"逻辑。

## 数据流

```
页面加载
   ↓
localStorage.getItem('techinsight_theme')
   ↓                    ↓
  有值                无值
   ↓                    ↓
使用存储值         matchMedia 检测
   ↓                    ↓
   └─────→ 应用主题 ─────┘
               ↓
        setAttribute + 更新按钮图标
               ↓
        (P1) matchMedia change 事件
               ↓
        localStorage 无值? → 实时切换
        localStorage 有值? → 忽略
```

## 数据模型

无新增数据结构。复用已有 `currentTheme` 变量和 `techinsight_theme` localStorage key。

存储位置：localStorage key `techinsight_theme`（已有，值域 `'dark'` / `'light'`）

## 状态管理

| 状态 | 类型 | 初始值 | 更新时机 | 持久化 |
|------|------|--------|----------|--------|
| currentTheme | string | 由 localStorage/matchMedia 决定 | 手动切换 / 系统变化 | 是（techinsight_theme） |
| matchMedia 监听 | function | 页面加载时注册 | 系统主题变化时触发 | 否 |

## 风险与缓解

| 风险 | 概率 | 影响 | 缓解策略 |
|------|------|------|----------|
| `matchMedia` 不兼容（IE11） | 低 | 低 | `if (window.matchMedia)` 守卫，回退到亮色 |
| change 监听与手动切换冲突 | 低 | 中 | 监听器内检查 localStorage 有值则 return |
| 图表背景未更新 | 低 | 低 | change 回调中复用 toggleTheme 的图表更新逻辑 |

## 性能影响

- 预估新增代码行数：20 行
- 预估渲染影响：< 1ms（matchMedia 是同步 API）
- 优化措施：无（已是最低开销）
- 性能预算校验：当前 ~88KB + ~0.5KB = ~88.5KB，未超标（<100KB）

## Constitution 合规检查

- [x] 架构约束：合规，全部内联在 index.html
- [x] 编码规范：合规，用 `var` + `function` + 单引号 + `addEventListener`
- [x] 依赖策略：合规，使用浏览器原生 API `matchMedia`
- [x] 性能预算：合规，新增 20 行 + 0.5KB < 预算
- [x] 安全约束：合规，无 eval / 无外部调用 / 无敏感数据
