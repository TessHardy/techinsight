# SDD 交付管线 — 完整项目结构参考

> 基于验证成功的 trae-active 项目，展示完整的 `.specify` + `.trae` 目录结构
> 新项目运行 `init-pipeline` 即可自动生成以下全部内容

## 目录总览

```
project-root/
├── .specify/                          # Spec-Kit 配置层
│   ├── README.md                      # 使用说明文档
│   ├── memory/
│   │   └── constitution.md            # 项目章程（每个项目必改）
│   ├── templates/                     # 7 个通用模板
│   │   ├── project-template.md        # 模板包总览
│   │   ├── constitution-template.md   # 章程模板（空白版）
│   │   ├── feature-template.md        # 功能规范 (WHAT)
│   │   ├── plan-template.md           # 技术方案 (HOW)
│   │   ├── tasks-template.md          # 任务拆分 (WHEN)
│   │   ├── checklist-template.md      # 质量检查清单
│   │   └── research-template.md       # 技术调研（可选）
│   ├── examples/                      # 3 个真实示例
│   │   ├── example-spec.md            # 成就系统规范
│   │   ├── example-plan.md            # 成就系统方案
│   │   └── example-tasks.md           # 成就系统任务
│   └── scripts/powershell/
│       ├── init-pipeline.ps1          # 一键初始化（会随复制传播）
│       └── new-spec.ps1               # 创建新规范目录
│
├── .trae/                             # TRAE 集成层
│   ├── settings.json                  # Hook 注册配置
│   ├── hooks/                         # 3 个生命周期脚本
│   │   ├── session-start.ps1          # 会话启动 → 加载上下文
│   │   ├── user-prompt-submit.ps1     # 提交前 → 检测功能需求
│   │   └── stop.ps1                   # 完成后 → 质量门禁
│   ├── skills/                        # 4 个 slash command
│   │   ├── specify/SKILL.md           # /specify  定义 WHAT
│   │   ├── plan/SKILL.md              # /plan     设计 HOW
│   │   ├── tasks/SKILL.md             # /tasks    拆分 WHEN
│   │   └── implement/SKILL.md         # /implement 执行 CODE
│   └── rules/
│       └── project_rules.md           # Agent 自动读取的项目规则
│
├── specs/                             # 规范文档存放处
│   └── .gitkeep
│
└── scripts/
    └── worktree.ps1                   # Git Worktree 管理工具
```

---

## 各文件详解

### .specify/memory/constitution.md — 项目章程

每个项目必须编辑此文件，定义技术约束、编码规范和性能预算。

```markdown
# {项目名} 项目章程

## 技术栈
- 前端：{框架/库}
- 后端：{语言+框架 / 无}
- 数据库：{类型 / 无}
- 部署：{平台}

## 架构约束
- {约束1}
- {约束2}

## 编码规范
- 语言：{TypeScript / JavaScript / Python}
- 命名：{camelCase / snake_case}
- 注释：{JSDoc / docstring / 仅复杂逻辑处}

## 依赖策略
- 优先使用标准库，最后才引入依赖
- 新增依赖需在 spec 中说明理由

## 性能预算
- 首屏加载 < {N}s
- 包体积 < {N}KB（gzip）

## 安全约束
- 不硬编码密钥
- 不使用 eval / innerHTML
```

---

### .trae/settings.json — Hook 注册

```json
{
  "hooks": {
    "SessionStart": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "powershell -ExecutionPolicy Bypass -File \"$TRAE_PROJECT_DIR\\.trae\\hooks\\session-start.ps1\""
          }
        ]
      }
    ],
    "UserPromptSubmit": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "powershell -ExecutionPolicy Bypass -File \"$TRAE_PROJECT_DIR\\.trae\\hooks\\user-prompt-submit.ps1\""
          }
        ]
      }
    ],
    "Stop": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "powershell -ExecutionPolicy Bypass -File \"$TRAE_PROJECT_DIR\\.trae\\hooks\\stop.ps1\""
          }
        ]
      }
    ]
  }
}
```

---

### .trae/skills/specify/SKILL.md

```yaml
---
name: specify
description: 创建功能规范 (spec.md) — 定义 WHAT，不定义 HOW
---
```

工作流程：
1. 读取 constitution.md → 了解约束
2. 读取 feature-template.md → 获取模板
3. 确认编号（查看 specs/ 目录）
4. 基于用户描述填充模板
5. 输出到 `specs/NNN-name/spec.md`

---

### .trae/skills/plan/SKILL.md

```yaml
---
name: plan
description: 基于规范生成技术方案 (plan.md) — 定义 HOW
---
```

工作流程：
1. 读取 spec.md → 验收标准
2. 读取 constitution.md → 约束
3. 读取 plan-template.md → 模板
4. 设计技术方案
5. 输出到 `specs/NNN-name/plan.md`

---

### .trae/skills/tasks/SKILL.md

```yaml
---
name: tasks
description: 基于方案拆分可执行任务 (tasks.md)
---
```

工作流程：
1. 读取 plan.md → 实现步骤
2. 读取 tasks-template.md → 模板
3. 拆分为原子任务（< 30min/个）
4. 标注依赖关系（串行/并行）
5. 输出到 `specs/NNN-name/tasks.md`

---

### .trae/skills/implement/SKILL.md

```yaml
---
name: implement
description: 基于 tasks.md 执行代码实现
---
```

工作流程：
1. 读取 tasks.md → 任务清单
2. 读取 plan.md + spec.md + constitution.md
3. 逐项实现，每完成一项标记 checkbox
4. 全部完成后验证验收标准

原则：YAGNI → 复用 → 标准库 → 原生 → 一行 → 最小可行

---

### .trae/hooks/session-start.ps1 — 会话启动

自动加载 constitution + 活跃 specs 状态 + worktree 信息 + 管线提醒。源发现：`$env:TRAE_PROJECT_DIR` 或向上查找 `.specify/`。

### .trae/hooks/user-prompt-submit.ps1 — 提交前

检测中英文功能关键词（添加/新增/create/implement...），匹配到时建议走 SDD 流程，否则静默。

### .trae/hooks/stop.ps1 — 质量门禁

检查：script 标签匹配、console.log 残留、文件体积预算、git 工作区状态。严重问题（标签不匹配）exit 2 强制修正。

---

### .trae/rules/project_rules.md — Agent 自动规则

```
SDD 流程：/specify → /plan → /tasks → /implement
何时使用：新功能→全流程，Bug→跳到implement，小改动→直接改
Worktree：.\worktree.ps1 create/list/remove/hotfix
Constitution：所有修改必须遵守
```

---

### scripts/worktree.ps1 — 并行开发

| 命令 | 作用 |
|------|------|
| `.\worktree.ps1 create <name>` | 创建 feature/<name> 分支的 worktree |
| `.\worktree.ps1 list` | 列出所有 worktree |
| `.\worktree.ps1 remove <name>` | 删除 worktree（可选删分支） |
| `.\worktree.ps1 hotfix` | 创建 hotfix-日期时间 分支 |

自动检测：向上查找 `.git/` 定位项目根目录，worktree 放在同级目录 `{项目名}-{功能名}`。

---

## 全局工具

```
C:\Users\32947\.trae-cn\tools\
├── init-pipeline.ps1    ← 全局版初始化脚本
└── install.ps1          ← PATH + PS profile 别名注册
```

用法：
```powershell
init-pipeline                              # 自动发现源项目
init-pipeline -Source "f:\code\trae-active" # 指定源
init-pipeline -ProjectName "my-app"         # 指定项目名
```

源发现优先级：
1. `-Source` 参数
2. 当前目录有 `.specify/`
3. 扫描 `f:\code` 等目录，优先选有 `.trae/skills/` 的完整管线项目
