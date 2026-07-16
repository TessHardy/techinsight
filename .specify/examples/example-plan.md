# 技术方案：achievement-system

> 基于 spec.md 的验收标准，描述 HOW

## 方案概述

使用 localStorage 持久化 + 全局 ACHIEVEMENTS 数组 + 各交互点埋入 unlockAchievement() 调用实现成就系统。

## 技术选型

| 需求 | 方案 | 理由 | 替代方案 |
|------|------|------|----------|
| 成就数据存储 | localStorage JSON | 无需后端，constitution 允许 | IndexedDB（过重） |
| 解锁通知 | CSS 动画 toast | 零依赖，constitution 要求内联 | 第三方 toast 库（违规） |
| 进度追踪 | 全局计数器对象 | 简单直接，与已有代码风格一致 | Proxy 代理（不必要） |

## 实现步骤

### 步骤 1: 定义成就数据和状态

- 文件：`index.html` JS 区域
- 操作：新增 ACHIEVEMENTS 数组和 achievementUnlocked 对象
- 关键代码：
  ```javascript
  var ACHIEVEMENTS = [
    {id:'first_visit', icon:'🎯', title:'初探者', desc:'打开 TechInsight'},
    {id:'data_lover', icon:'📊', title:'数据控', desc:'查看所有 7 种图表'},
    // ...
  ];
  var achievementUnlocked = {};
  try { achievementUnlocked = JSON.parse(localStorage.getItem('ti_achievements') || '{}'); } catch(e) {}
  ```

### 步骤 2: 实现 unlockAchievement 函数

- 文件：`index.html` JS 区域
- 操作：新增解锁逻辑 + toast 通知
- 关键代码：
  ```javascript
  function unlockAchievement(id) {
    if (achievementUnlocked[id]) return;
    achievementUnlocked[id] = Date.now();
    localStorage.setItem('ti_achievements', JSON.stringify(achievementUnlocked));
    showAchievementToast(id);
    checkMasterExplorer();
  }
  ```

### 步骤 3: 在各交互点埋入调用

- 文件：`index.html` JS 区域
- 操作：在 showChartDetail()、pkRandom()、tellFortune() 等函数中插入 unlockAchievement 调用
- 关键代码：
  ```javascript
  // 在 showChartDetail 中
  achievementStats.chartsViewed++;
  if (achievementStats.chartsViewed >= achievementStats.totalCharts) {
    unlockAchievement('data_lover');
  }
  ```

### 步骤 4: 成就面板 HTML + CSS

- 文件：`index.html` HTML/CSS 区域
- 操作：新增面板 DOM 结构和样式
- 关键代码：
  ```html
  <div id="achievementPanel" class="achievement-panel" role="dialog" aria-label="成就">
    <!-- 动态生成 -->
  </div>
  ```

## 数据流

```
用户操作（查看图表/PK/运势...）
   ↓                    ↓
交互函数          achievementStats 计数器更新
   ↓                    ↓
unlockAchievement(id)   达到阈值?
   ↓                    ↓
localStorage 持久化     是 → 解锁成就
   ↓
showAchievementToast()  →  3s 后自动消失
```

## 数据模型

```javascript
// 成就定义（只读）
var ACHIEVEMENTS = [
  {id: string, icon: string, title: string, desc: string}
];

// 解锁状态（localStorage: ti_achievements）
// {"first_visit": 1700000000000, "data_lover": 1700000001000}

// 进度统计（内存，不持久化）
var achievementStats = {
  chartsViewed: 0,
  totalCharts: 7,
  pkCount: 0,
  fortuneCount: 0,
  citiesHovered: new Set()
};
```

存储位置：localStorage key `ti_achievements`

## 状态管理

| 状态 | 类型 | 初始值 | 更新时机 | 持久化 |
|------|------|--------|----------|--------|
| achievementUnlocked | object | {} | 解锁成就时 | 是（localStorage） |
| achievementStats.chartsViewed | number | 0 | 查看图表时 | 否 |
| achievementStats.pkCount | number | 0 | 完成PK时 | 否 |

## 风险与缓解

| 风险 | 概率 | 影响 | 缓解策略 |
|------|------|------|----------|
| localStorage 损坏 | 低 | 中 | try-catch 包裹 JSON.parse |
| 成就计数不准 | 中 | 低 | 用 Set 去重，避免重复计数 |
| toast 与其他弹窗冲突 | 中 | 低 | toast 层级 z-index 9999 |

## 性能影响

- 预估新增代码行数：180 行
- 预估渲染影响：< 5ms（toast 动画用 CSS transform）
- 优化措施：toast 用 requestAnimationFrame
- 性能预算校验：当前 85KB + ~3KB = ~88KB，未超标

## Constitution 合规检查

- [x] 架构约束：合规，全部内联在 index.html
- [x] 编码规范：合规，用 var + function + 单引号
- [x] 依赖策略：合规，零新增依赖
- [x] 性能预算：合规，88KB < 100KB
- [x] 安全约束：合规，localStorage 不存敏感信息
