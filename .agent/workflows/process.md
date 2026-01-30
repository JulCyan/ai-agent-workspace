---
description: 项目开发工作流 - 目录结构与交互规则
---

# 开发工作流 (Development Protocol)

> 每次开发需求时，在 `.agent/projects/<project-branch>/` 下组织工作文档。

## 📁 目录结构

```text
.agent/
├── docs/                      # 通用文档/模板 (跟踪)
│   └── templates/
├── projects/                  # 活跃工作区 (跟踪)
│   ├── <project-branch>/
│   │   ├── docs/              # 需求、方案、输出
│   │   ├── prototypes/        # 原型设计
│   │   └── apis/              # 接口文档 + 数据
│   └── archive/               # 归档 (不跟踪)
│       └── <project-branch>/
├── rules/
└── workflows/
```

---

## 📘 交互规则

### 🟦 Input（任务前）

- 用户将资料放入 `.agent/projects/<project-branch>/` 对应子目录：
  - API 文档/数据 → `apis/`
  - 原型设计 → `prototypes/`
  - 参考文档 → `docs/`

> **注意**：用户可能先将文档临时放入projects根目录，Agent 在规划阶段需将其**移动**至对应子目录。

### 🟩 Output（任务中/后）

- Agent 输出需求分析、实施方案等文档至 `.agent/projects/<project-branch>/docs/`

### 🟨 Archive（任务完成）

- 将整个 `<project-branch>/` 从 `projects/` 移动到 `projects/archive/`
- `archive/` 不上传，仅本地保留

> ⚠️ **MOVE 操作**：移动时原文件不应保留，确保使用 **Move（剪切）** 而非 Copy。

---

## 📌 命名约定

- `<project-branch>`：项目名 + 分支名，使用连字符连接
  - 示例：`fe-main-feat-login`、`fe-upload-tb-fix-sku`
- 跨项目通用文档可使用 `shared-xxx` 或 `common-xxx`
