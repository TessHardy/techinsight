# 任务列表：auto-dark-mode

> 基于 plan.md 的实现步骤，拆分为可执行、可验证的原子任务

## 元信息

- 关联规范：`specs/001-auto-dark-mode/spec.md`
- 关联方案：`specs/001-auto-dark-mode/plan.md`
- 总任务数：3
- 预估总工时：15 分钟

---

### T1: 替换主题初始化逻辑为三级优先级

- [x] 已完成

**描述：** 将 `index.html#L1778-L1783` 的简单 `if(localStorage)` 初始化替换为三级优先级：localStorage > matchMedia > 默认亮色。现有代码只判断 `===light`，不处理无 localStorage 且系统为暗色的情况。

**文件：** `index.html#L1778-L1783`

**替换前：**
```javascript
// 初始化主题
if(localStorage.getItem('techinsight_theme')==='light'){
  currentTheme='light';
  document.documentElement.setAttribute('data-theme','light');
  document.getElementById('themeToggle').textContent='☀️';
}
```

**替换后：**
```javascript
// 初始化主题（优先级：localStorage > 系统偏好 > 默认亮色）
var savedTheme=localStorage.getItem('techinsight_theme');
if(savedTheme){
  currentTheme=savedTheme;
}else if(window.matchMedia&&window.matchMedia('(prefers-color-scheme:dark)').matches){
  currentTheme='dark';
}else{
  currentTheme='light';
}
document.documentElement.setAttribute('data-theme',currentTheme);
var themeBtn=document.getElementById('themeToggle');
if(themeBtn)themeBtn.textContent=currentTheme==='dark'?'🌙':'☀️';
```

**验证：** 
1. 清除 localStorage，设置系统为暗色 → 打开页面应显示暗色主题
2. 清除 localStorage，设置系统为亮色 → 打开页面应显示亮色主题
3. localStorage 有 `techinsight_theme=light` → 无论系统偏好，应显示亮色

---

### T2: 添加 matchMedia change 事件监听

- [x] 已完成

**描述：** 在主题初始化代码之后（原 L1783 后），新增 `matchMedia` 的 `change` 事件监听器。当系统主题切换时，若 localStorage 无手动偏好记录，则实时跟随系统切换。需同步更新图表背景色。

**文件：** `index.html#L1783` 之后插入

**插入代码：**
```javascript
// 系统主题变化时实时响应（仅当用户未手动选择时）
if(window.matchMedia){
  var darkQuery=window.matchMedia('(prefers-color-scheme:dark)');
  darkQuery.addEventListener('change',function(e){
    if(localStorage.getItem('techinsight_theme'))return;
    currentTheme=e.matches?'dark':'light';
    document.documentElement.setAttribute('data-theme',currentTheme);
    var btn=document.getElementById('themeToggle');
    if(btn)btn.textContent=currentTheme==='dark'?'🌙':'☀️';
    var bgColor=currentTheme==='dark'?'transparent':'#ffffff';
    Object.keys(chartInstances).forEach(function(key){
      if(chartInstances[key])chartInstances[key].setOption({backgroundColor:bgColor});
    });
  });
}
```

**验证：**
1. 清除 localStorage → 页面加载后切换系统暗色/亮色 → 页面应实时跟随切换
2. 手动点击主题切换按钮 → 再切换系统主题 → 页面不应跟随（保持手动选择）
3. 手动切换后清除 localStorage → 再切换系统主题 → 应恢复跟随

---

### T3: 确保 toggleTheme 手动选择优先级正确

- [x] 已完成（审查通过，无需修改）

**描述：** 审查现有 `toggleTheme()` 函数（`index.html#L1762-L1776`），确认：
1. 函数末尾 `localStorage.setItem('techinsight_theme',currentTheme)` 存在 → 手动选择会被记录
2. T2 的 change 监听器通过 `localStorage.getItem('techinsight_theme')` 有值则 return → 手动选择不会被系统覆盖
3. 两者逻辑一致，无需修改 `toggleTheme()` 本身

**文件：** `index.html#L1762-L1776`（审查，预期无需修改）

**验证：**
1. 页面加载（无 localStorage）→ 手动切换主题 → 刷新页面 → 保持手动选择
2. 手动切换后 → 切换系统主题 → 页面不跟随 → 符合预期
3. DevTools 清除 localStorage → 刷新 → 页面回到跟随系统

---

## 执行顺序

```
T1 → T2 → T3    (串行，T2 依赖 T1 的初始化变量，T3 依赖 T2 的监听器逻辑)
```

## 完成定义 (Definition of Done)

- [ ] 所有任务 checkbox 标记为 [x]
- [ ] spec.md 中 P0 验收标准全部通过：
  - [ ] matchMedia 检测自动应用主题
  - [ ] 系统暗色 → 页面暗色；系统亮色 → 页面亮色
  - [ ] 手动切换后优先级高于系统检测
  - [ ] 有 localStorage 时使用存储值
- [ ] spec.md 中 P1 验收标准通过：
  - [ ] 系统主题变化时实时更新
  - [ ] 切换按钮状态与主题一致
- [ ] 无 console 错误或警告
- [ ] 暗色/亮色主题切换正常，图表背景跟随
- [ ] 移动端视口下布局不崩
- [ ] 新增代码无 eval / innerHTML
- [ ] 性能预算：新增 ~20 行，< 0.5KB，未超标
