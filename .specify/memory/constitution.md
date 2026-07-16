# TechInsight 项目章程

## 技术栈

- 前端：纯 HTML/CSS/JS 单文件架构（index.html），零构建依赖
- 图表：ECharts 5.x（CDN 引入）
- 动画：Canvas API + CSS @keyframes + IntersectionObserver
- 持久化：localStorage（主题、成就、引导状态）
- 部署：GitHub Pages + GitHub Actions CI/CD

## 架构约束

- **单文件原则**：所有 CSS/JS 内联于 index.html，不拆分外部文件
- **零构建**：不使用 Webpack/Vite/任何 bundler，直接浏览器运行
- **CDN 优先**：外部依赖仅通过 CDN script 标签引入
- **渐进增强**：核心功能不依赖 JS，JS 增强体验

## 编码规范

- 使用 `var` 声明变量（兼容性优先）
- 函数声明使用 `function` 关键字，不用箭头函数
- 字符串使用单引号
- CSS 变量定义在 `:root`，暗色主题用 `[data-theme="dark"]`
- 事件监听用 `addEventListener`，不用 `onclick`
- 所有用户交互元素添加 ARIA 标签

## 依赖策略（Ponytail 哲学）

1. YAGNI — 不需要的别加
2. 复用 — 先看已有代码能不能用
3. 标准库 — 用浏览器原生 API
4. 原生平台 — HTML/CSS/JS 能做的不上框架
5. 依赖 — 只有 CDN 引入，不 npm install
6. 一行 — 能一行解决的不写函数
7. 最小可行 — 满足需求的最小代码量

## 性能预算

- index.html < 100KB（gzip 前）
- 首屏加载 < 3s（3G 网络）
- 动画帧率 >= 30fps
- 无外部 API 调用

## 安全约束

- 不存储敏感信息到 localStorage
- 不使用 eval()
- 不引入第三方追踪脚本
- Content-Security-Policy 友好
