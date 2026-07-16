# 项目章程模板

> 复制此文件到 `.specify/memory/constitution.md`，填充你的项目约束

## 技术栈

- 前端：{框架/库，如 React 18 / Vue 3 / 原生 HTML}
- 后端：{语言+框架，如 Node.js + Express / Python + FastAPI / 无}
- 数据库：{类型+版本，如 PostgreSQL 16 / SQLite / 无}
- 部署：{平台，如 Vercel / GitHub Pages / Docker}

## 架构约束

- {约束 1，如：前后端分离，API 优先}
- {约束 2，如：SSR 首屏，CSR 交互}
- {约束 3，如：无外部状态管理库，用原生 Context}

## 编码规范

- 语言：{TypeScript strict / JavaScript ES2022 / Python 3.12+}
- 格式化：{Prettier 配置 / Black / gofmt}
- Lint：{ESLint 规则集 / Ruff / golangci-lint}
- 命名：{camelCase / snake_case / kebab-case}
- 注释：{JSDoc 必须 / docstring 可选 / 注释仅在复杂逻辑处}

## 依赖策略

- {策略 1，如：优先使用标准库，最后才引入依赖}
- {策略 2，如：新增依赖需在 spec 中说明理由}
- {策略 3，如：锁定版本，用 lockfile}

## 性能预算

- 首屏加载 < {N}s（{网络条件}）
- Lighthouse 性能分 > {N}
- 包体积 < {N}KB（gzip）
- API 响应 p95 < {N}ms

## 安全约束

- {约束 1，如：不硬编码密钥，用环境变量}
- {约束 2，如：所有用户输入必须校验和转义}
- {约束 3，如：不使用 eval / innerHTML}
