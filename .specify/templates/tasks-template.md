# 任务列表：{feature_name}

> 基于 plan.md 的实现步骤，拆分为可执行、可验证的原子任务

## 元信息

- 关联规范：`specs/NNN-{feature_name}/spec.md`
- 关联方案：`specs/NNN-{feature_name}/plan.md`
- 总任务数：{N}
- 预估总工时：{N} 分钟

---

## 任务

### T1: {任务标题}

- [ ] 未完成

**描述：** {具体可执行的操作，如：在 index.html 的 CSS 区域新增 .modal-overlay 样式}

**文件：** `index.html#L{start}-L{end}`

**验证：** {如何确认完成，如：浏览器中打开页面，触发弹窗，遮罩层半透明黑色}

---

### T2: {任务标题}

- [ ] 未完成

**描述：** {具体操作}

**文件：** `index.html#L{start}-L{end}`

**验证：** {验证方式}

---

### T3: {任务标题}

- [ ] 未完成

**描述：** {具体操作}

**文件：** `index.html#L{start}-L{end}`

**验证：** {验证方式}

---

### T4: {任务标题}

- [ ] 未完成

**描述：** {具体操作}

**文件：** `index.html#L{start}-L{end}`

**验证：** {验证方式}

---

## 执行顺序

```
T1 → T2 → T3 → T4   (串行，T2 依赖 T1 的 CSS 类名)
T1 → T3              (并行，CSS 和 HTML 结构可同时写)
```

## 完成定义 (Definition of Done)

- [ ] 所有任务 checkbox 标记为 [x]
- [ ] spec.md 中所有 P0 验收标准通过
- [ ] 无 console 错误或警告
- [ ] 暗色/亮色主题切换正常
- [ ] 移动端视口下布局不崩
- [ ] 新增代码无 eval / innerHTML
- [ ] 性能预算未超标
