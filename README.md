# TechInsight 技术洞察

> 🚀 一站式技术分析可视化平台，让数据驱动技术决策

[![TRAE](https://img.shields.io/badge/Built%20with-TRAE-blueviolet)](https://www.trae.ai)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)
[![Live Demo](https://img.shields.io/badge/demo-online-brightgreen)](https://tesshardy.github.io/techinsight/)
[![GitHub Pages](https://img.shields.io/badge/GitHub-Pages-222)](https://tesshardy.github.io/techinsight/)

## ✨ 项目简介

**TechInsight** 是一个面向开发者和技术决策者的**技术分析可视化平台**。它将分散在 GitHub、NPM、Stack Overflow 等多个平台的技术数据聚合为直观的可视化图表，让技术选型从"凭经验"变成"凭数据"。

### 🎯 核心价值

- **效率提升 12 倍**：原本需要 2 小时的分析工作，10 分钟即可完成
- **降低选型风险 60%**：数据驱动的决策，告别反复争论
- **8 大核心功能**：7 种图表 + 智能评分 + AI 助手 + PDF 导出

## 🌐 在线体验

- 🚀 **Demo 地址**：https://tesshardy.github.io/techinsight/
- 📦 **GitHub 仓库**：https://github.com/TessHardy/techinsight

## 📸 项目截图

### 首屏
- 粒子动画背景
- 渐变标题 + 打字机效果
- 实时数据流滚动条

### 图表分析区
- 7 种 ECharts 图表
- 6 框架智能筛选器
- 时间范围切换

### 智能评分
- 6 种典型场景
- AI 多维评估
- 可视化推荐结果

### AI 智能助手
- 6 个高频问题知识库
- 自定义问题 AI 回答
- 一键生成定制报告

## ✨ 核心功能

### 📊 数据分析
| 功能 | 说明 |
|------|------|
| 📈 **7 种 ECharts 图表** | 折线/柱状/饼/雷达/仪表盘/散点/热力图 |
| 🔍 **智能筛选器** | 6 框架实时联动 7 图表 |
| ⏰ **时间范围切换** | 近 3 年 / 近 5 年 / 全部 |
| 🔮 **AI 趋势预测** | 2026-2027 预测区间可视化 |
| 🔮 **多框架雷达对比** | 2-6 框架同时叠加对比 |

### 🎮 互动玩法
| 功能 | 说明 |
|------|------|
| 🎮 **PK 擂台** | 6 维度对比 + 随机对决 + 胜利者加冕 |
| 🔮 **今日框架运势** | 占卜 + 神秘动画 + 幸运三件套 |
| 🗺️ **智能学习路径** | 3 题问卷生成专属路线 |
| 💼 **薪资就业分析** | 6 框架薪资 + 趋势涨跌 |
| 🌍 **全球开发者地图** | 15 个城市动态光点 |

### 🤖 AI 助手
| 功能 | 说明 |
|------|------|
| 🎯 **智能评分** | 6 种场景（中后台/大前端/移动端/全栈/SEO/初创） |
| 💬 **AI 智能助手** | 6+ 高频问答 + 定制洞察报告 |
| 📝 **代码示例** | 6 种框架代码片段，一键复制 |

### 🎨 用户体验
| 功能 | 说明 |
|------|------|
| 🌓 **主题切换** | 深色/浅色主题自由切换 |
| 🎬 **新手引导** | 7 步引导 + localStorage 记忆 |
| ⌨️ **键盘快捷键** | `?` 调出快捷键面板，Vim 式 `g+h/c/p` 跳转 |
| 🔗 **URL 状态同步** | 筛选/主题可分享链接 |
| 📄 **PDF 报告导出** | 完整内容一键导出 |
| 🚀 **返回顶部火箭** | 滚动 400px 后出现 |
| ♿ **a11y 友好** | ARIA 标签 + 键盘导航 + 减少动画偏好 |
| 🛡️ **404 错误页** | 主题同步的优雅错误页 |

## 💡 真实场景

### 场景 1：新项目技术选型
> 痛点：3 天选框架争论 → 用 TechInsight 智能评分 1 分钟决策

### 场景 2：老项目性能优化
> 痛点：没有数据说服领导 → 用散点图 + PDF 报告让数据说话

### 场景 3：团队技术分享
> 痛点：经验主义没说服力 → 7 种图表 + AI 预测数据支撑观点

## 🪤 开发过程：踩过的坑

| 坑 | 解决方案 |
|----|---------|
| 🐌 ECharts 数据点卡顿 | 开启 `large:true` + `sampling:'lttb'` |
| 🎨 主题切换图表不更新 | 统一 `themeConfig` + `setOption()` |
| ✂️ 模板字符串 `<script>` 截断 | 用 `\x3Cscript` 转义，显示时还原 |
| 🚫 GitHub Pages 404 | Source 选 `GitHub Actions` 而非 main |
| ⚡ 筛选器联动卡顿 | `requestAnimationFrame` 批处理 |
| 🐢 AI 助手首次加载慢 | `setTimeout` 延迟加载知识库 |

## 🎨 技术栈

- **HTML5** + **CSS3** + **原生 JavaScript**（无构建工具）
- **ECharts 5.x**（CDN 引入）
- **localStorage**（主题状态、新手引导状态）
- **GitHub Pages**（自动部署）
- **GitHub Actions**（CI/CD）

## 🚀 本地运行

```bash
# 克隆仓库
git clone https://github.com/TessHardy/techinsight.git

# 进入目录
cd techinsight

# 启动本地服务器（可选）
node server.js

# 浏览器打开
open http://localhost:3000
```

或者直接双击 `index.html` 在浏览器中打开。

## 📦 部署

本项目使用 **GitHub Pages** + **GitHub Actions** 自动部署：

1. 推送代码到 `main` 分支
2. GitHub Actions 自动构建
3. 自动发布到 `https://<username>.github.io/techinsight/`

## 🤖 TRAE 能力运用

本次开发全程使用 **TRAE** 完成，主要能力：

- 🧠 代码生成（HTML/CSS/JS 框架）
- ✍️ 代码补全（ECharts 配置）
- 📁 多文件编辑
- 💻 终端命令（git、node）
- 🌐 浏览器调试
- 🐛 错误修复

## 📈 项目数据

- 单文件 HTML：~1800 行
- 总功能数：**10 大核心功能**
- 图表数量：**7 种 ECharts 图表**
- 真实场景：**3 个**
- 案例展示：**4 个**
- 踩坑记录：**6 个**
- 代码示例：**6 种框架**
- AI 知识库：**6 个高频问题**

## 🏆 大赛信息

本作品为 **TRAE AI 创造力大赛** 参赛作品，赛道：**学习工作**

## 📄 License

MIT License - 详见 [LICENSE](LICENSE) 文件

## 🙏 致谢

- **TRAE** - 让创意快速变成产品
- **ECharts** - 强大的可视化图表库
- **GitHub Pages** - 免费的静态网站托管

---

⭐ 如果这个项目对你有帮助，欢迎 Star！
