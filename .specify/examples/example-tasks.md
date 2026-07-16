# 任务列表：achievement-system

> 基于 plan.md 的实现步骤，拆分为可执行、可验证的原子任务

## 元信息

- 关联规范：`specs/005-achievement-system/spec.md`
- 关联方案：`specs/005-achievement-system/plan.md`
- 总任务数：6
- 预估总工时：45 分钟

---

## 任务

### T1: 定义 ACHIEVEMENTS 数组和状态变量

- [x] 已完成

**描述：** 在 index.html 的 JS 区域（其他全局变量附近）新增 ACHIEVEMENTS 数组定义、achievementUnlocked 对象、achievementStats 计数器

**文件：** `index.html#L1386-L1400`

**验证：** 浏览器控制台输入 `ACHIEVEMENTS.length` 返回 8

---

### T2: 实现 unlockAchievement 和 showAchievementToast 函数

- [x] 已完成

**描述：** 新增 unlockAchievement(id) 函数（去重+持久化+toast）、showAchievementToast(id) 函数（创建 DOM + 动画 + 3s 自动移除）

**文件：** `index.html#L1402-L1450`

**验证：** 控制台执行 `unlockAchievement('first_visit')`，页面底部弹出成就 toast，3s 后消失

---

### T3: 成就面板 HTML 和 CSS

- [x] 已完成

**描述：** 在导航栏添加徽章图标按钮，在 body 末尾添加成就面板 DOM 结构，新增 .achievement-toast、.achievement-panel、.achievement-item 等 CSS 样式

**文件：** `index.html#L80-L123`（CSS）、`index.html#L611-L637`（HTML）

**验证：** 点击导航栏徽章图标，弹出成就面板，展示 8 个成就项

---

### T4: 在交互点埋入成就触发调用

- [x] 已完成

**描述：** 在以下函数中插入 unlockAchievement 调用：
- showChartDetail() → chartsViewed++
- showStatDetail() → chartsViewed++
- pkRandom() → pkCount++
- tellFortune() → fortuneCount++
- selectQuiz() → learner
- renderJobs() → job_seeker
- 城市 dot onmouseenter → citiesHovered.add()

**文件：** `index.html` 多处分散

**验证：** 依次操作查看图表×7、PK×1、运势×3，检查对应成就是否解锁

---

### T5: 初始化触发 first_visit 成就

- [x] 已完成

**描述：** 在 DOMContentLoaded 事件中调用 unlockAchievement('first_visit')

**文件：** `index.html#L1460`

**验证：** 清除 localStorage 后刷新页面，自动弹出"初探者"成就

---

### T6: 实现 master_explorer 终极成就

- [x] 已完成

**描述：** 在 unlockAchievement 中检查已解锁数量 >= 7 时自动解锁 master_explorer

**文件：** `index.html#L1420-L1425`

**验证：** 手动解锁 7 个成就后，"完整体验"成就自动解锁

---

## 执行顺序

```
T1 → T2 → T3 → T4 → T5 → T6   (串行，每步依赖前一步的变量/函数)
```

## 完成定义 (Definition of Done)

- [x] 所有任务 checkbox 标记为 [x]
- [x] spec.md 中所有 P0 验收标准通过
- [x] 无 console 错误或警告
- [x] 暗色/亮色主题切换正常
- [x] 移动端视口下布局不崩
- [x] 新增代码无 eval / innerHTML
- [x] 性能预算未超标
