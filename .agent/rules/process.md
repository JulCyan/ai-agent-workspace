---
trigger: always_on
tags: [core, general, workflow]
---

# 开发工作流 (Development Protocol)

> 每次开发需求时，在 `.agent/` 下按 **类型（doc/prototype/api）** 和 **项目/分支** 维度组织工作文档。

### 📁 目录结构示例

```text
.agent/
  ├── doc/                           # 方案设计与文档产出
  │   ├── <project-branch>/          # 项目-分支（如 fe-main-feat-login）
  │   │   ├── requirements.md        # 需求分析
  │   │   ├── implementation.md      # 实施方案
  │   │   ├── pending-backend.md     # 待后端支持项
  │   │   └── output/                # [Output] 文档产出区
  │   ├── archive/                   # 历史文档归档(任务完成)
  │   └── ...
  │
  ├── prototype/                     # 原型设计文件（用户提供）
  │   ├── <project-branch>/
  │   └── ...
  │
  └── api/                           # API 接口文档（用户提供）
      ├── <project-branch>/
      └── ...
```

---

## 📘 交互规则

### 🟦 Input（任务前）

- 用户将 API 文档放入 `.agent/api/<project-branch>/`
- 用户将原型设计放入 `.agent/prototype/<project-branch>/`
- 用户将参考文档放入 `.agent/doc/<project-branch>/`

> **注意**：用户可能先将文档临时放入 `doc/`、`prototype/`、`api/` 根目录下，Agent 在规划阶段需将其**移动**（Move，非复制）至对应的 `<project-branch>/` 子目录中，原文件不应保留。

### 🟩 Output（任务中/后）

- Agent 输出需求分析、实施方案等文档至 `.agent/doc/<project-branch>/`
- 所有产出文档写入 `output/` 子目录

### 🟨 Archive（任务完成）

- 将历史文档移动至 `.agent/doc/archive/`

### 📌 命名约定

- `<project-branch>`：项目名 + 分支名，使用连字符连接（如 `fe-main-feat-login`、`fe-upload-tb-fix-sku`）
- 跨项目通用文档可使用 `shared-xxx` 或 `common-xxx`
