# SDD 交付管线模板

Spec-Driven Development (SDD) 交付管线 — 从需求到上线的结构化开发流程。

```
/specify → /plan → /tasks → /implement
  WHAT      HOW     WHEN      CODE
```

## 一键初始化

```powershell
# 进入你的新项目目录
cd your-project

# 运行初始化（从已有管线的项目复制）
powershell -ExecutionPolicy Bypass -File "f:\code\trae-active\.specify\scripts\powershell\init-pipeline.ps1"

# 编辑项目章程（必做！）
notepad .specify\memory\constitution.md
```

初始化完成后生成：

```
.specify/           规范模板 + 章程 + 脚本
.trae/skills/       4 个 slash command
.trae/hooks/        3 个生命周期 hook
.trae/settings.json Hook 注册
.trae/rules/        Agent 自动读取的项目规则
specs/              规范文档存放目录
scripts/worktree.ps1 Git Worktree 管理工具
```

## 使用流程

### 新功能开发

1. `/specify` — 生成 `specs/NNN-name/spec.md`，定义做什么
2. `/plan` — 生成 `specs/NNN-name/plan.md`，设计怎么做
3. `/tasks` — 生成 `specs/NNN-name/tasks.md`，拆分任务
4. `/implement` — 按 tasks 逐项写代码

### Bug 修复 / 小改动

直接告诉 Agent，跳过规范流程。

### 并行开发

```powershell
.\scripts\worktree.ps1 create feature-x   # 创建 worktree
.\scripts\worktree.ps1 list                # 查看所有
.\scripts\worktree.ps1 remove feature-x    # 删除
```

### 手动创建规范目录

```powershell
.\.specify\scripts\powershell\new-spec.ps1 001 feature-name
```

## 模板清单

| 文件 | 用途 | 何时使用 |
|------|------|----------|
| `constitution-template.md` | 项目章程 | 每个项目初始化时 |
| `feature-template.md` | 功能规范 (WHAT) | `/specify` 时 |
| `plan-template.md` | 技术方案 (HOW) | `/plan` 时 |
| `tasks-template.md` | 任务拆分 (WHEN) | `/tasks` 时 |
| `checklist-template.md` | 质量检查 | spec 完成后自检 |
| `research-template.md` | 技术调研 | 方案选型时（可选） |

## 自动 Hooks

| Hook | 触发时机 | 作用 |
|------|----------|------|
| SessionStart | 会话启动 | 自动加载 constitution + specs 状态 |
| UserPromptSubmit | 用户发消息 | 检测功能需求，建议走 SDD 流程 |
| Stop | Agent 完成 | 质量门禁（语法/体积/残留检查） |

## 示例

`examples/` 目录包含成就系统的完整示例：

- `example-spec.md` — 规范怎么写
- `example-plan.md` — 方案怎么写
- `example-tasks.md` — 任务怎么拆

## 必做事项

初始化后务必：

1. **编辑 `constitution.md`** — 替换为你的项目技术栈、约束、规范
2. **重启 TRAE** — 让 hooks 和 skills 生效
