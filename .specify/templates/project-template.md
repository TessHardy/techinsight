# 项目模板包说明

> 本目录是 Spec-Driven Development (SDD) 交付管线的完整模板包
> 复制到任何新项目即可使用

## 目录结构

```
.specify/
├── memory/
│   └── constitution.md          ← 项目章程（必改！）
├── templates/
│   ├── project-template.md      ← 本文件：模板包总览
│   ├── constitution-template.md ← 章程模板（通用版）
│   ├── feature-template.md      ← 功能规范模板（WHAT）
│   ├── plan-template.md         ← 技术方案模板（HOW）
│   ├── tasks-template.md        ← 任务拆分模板（WHEN）
│   ├── research-template.md     ← 技术调研模板（可选）
│   └── checklist-template.md    ← 质量检查清单
├── scripts/
│   └── powershell/
│       ├── init-pipeline.ps1    ← 一键初始化脚本
│       └── new-spec.ps1         ← 创建新规范目录
└── examples/                    ← 填充示例（参考用）
    ├── example-spec.md
    ├── example-plan.md
    └── example-tasks.md
```

## 快速开始

### 1. 初始化到新项目

```powershell
cd your-new-project
powershell -ExecutionPolicy Bypass -File "f:\code\trae-active\.specify\scripts\powershell\init-pipeline.ps1"
```

### 2. 编辑 constitution.md

将模板中的占位符替换为你的项目实际情况。

### 3. 使用 SDD 流程

```
/specify → /plan → /tasks → /implement
  WHAT      HOW     WHEN      CODE
```

## 模板对应关系

| SDD 阶段 | 模板文件 | 产出文件 | 回答的问题 |
|-----------|----------|----------|-----------|
| specify | feature-template.md | specs/NNN-name/spec.md | 做什么？ |
| plan | plan-template.md | specs/NNN-name/plan.md | 怎么做？ |
| tasks | tasks-template.md | specs/NNN-name/tasks.md | 什么时候做？ |
| implement | (无模板，直接写代码) | 源代码文件 | 代码实现 |
| review | checklist-template.md | specs/NNN-name/checklist.md | 质量如何？ |
| research | research-template.md | specs/NNN-name/research.md | 方案选哪个？ |

## 占位符约定

所有模板中使用 `{花括号}` 标记占位符：

| 占位符 | 含义 | 示例值 |
|--------|------|--------|
| `{feature_name}` | 功能名（kebab-case） | `dark-mode-toggle` |
| `{NNN}` | 规范编号（三位数） | `001`, `002` |
| `{N}` | 数值 | `100`, `30` |
| `{用户角色}` | 用户角色 | `访客`, `管理员` |
| `{目标}` | 用户目标 | `切换暗色主题` |
| `{价值}` | 用户价值 | `保护眼睛` |

## 必改 vs 可选

| 文件 | 必须 | 说明 |
|------|------|------|
| constitution.md | **必改** | 每个项目的技术栈和约束不同 |
| feature-template.md | 可选 | 已通用化，可直接用 |
| plan-template.md | 可选 | 已通用化，可直接用 |
| tasks-template.md | 可选 | 已通用化，可直接用 |
| checklist-template.md | 可选 | 已通用化，可直接用 |
| research-template.md | 可选 | 按需使用 |
