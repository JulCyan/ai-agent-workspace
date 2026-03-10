# AI Agent Workspace

> **A production-ready meta-workspace template for AI-native development. Unify your Agent rules, memory context, and multi-repo workflows.**
>
> 专为 AI 原生开发打造的元工作区模板。统一管理 Agent 规则、记忆上下文和多仓库工作流。

这是专为 **Cursor**、**Google Antigravity** 和现代 AI 编程助手设计的**元工作区 (Meta-Workspace)** 配置。它允许您在一个统一的视窗中管理多个微服务或项目，同时共享同一套 AI 规则 (`.agent`)、编辑器配置 (`.vscode`) 和上下文记忆。

## ✨ 核心特性

- **🧠 集中式 AI 规则**: 在 `.agent/rules` 中定义全局提示词和行为准则，所有子项目自动继承。
- **💾 上下文持久化**: 通过 `.agent/context` 归档和同步 AI 的思维链与 Artifacts (需手动从 gitignore 放行私有内容)。
- **🎯 干扰隔离**: 智能的 `.gitignore` 和 `files.exclude` 配置，让您只关注配置本身，而忽略繁重的项目代码。
- **🔄 跨设备同步**: 像同步代码一样同步您的开发环境和“外脑”。
- **📦 Workspace-First**: 所有 IDE 行为以 `.workspace/base.code-workspace` 为唯一规范源。

## 🚨 核心约定 (Critical Rules)

> [!CAUTION]
> **禁止使用 `code .` 打开此文件夹！** 这会导致配置无法正确加载。
>
> ✅ **正确方式**: `code .workspace/full.code-workspace` 或其他具体工作区文件。

## 🚀 快速开始 (Getting Started)

### 1. 使用模板

克隆此仓库作为您的工作区根目录：

```bash
git clone https://github.com/your-username/cursor-agent-workspace.git Workspace
cd Workspace
```

### 2. 放入您的项目

将您现有的项目文件夹（如 `frontend-app`, `backend-service`）移动或克隆到此 `Workspace` 目录下。
_注：本仓库的 `.gitignore` 默认忽略所有未加白名单的子文件夹，因此您的项目代码**不会**被提交到这个配置仓库中。_

### 3. 配置工作区

复制示例文件创建您的工作区定义：

```bash
cp go-study.code-workspace my-project.code-workspace
```

打开 `my-project.code-workspace`，在 `folders` 中添加您的项目路径。

## 📂 目录结构

```text
Workspace/
├── .agent/                     # 🧠 AI 大脑核心
│   ├── docs/                   # 通用文档/模板 (✅ Git Tracked)
│   ├── projects/               # ⚡ 项目工作区 (❌ Ignored)
│   │   ├── shared/             #   🔄 跨设备同步 (✅ Git Tracked)
│   │   ├── <project-branch>/    #   💻 活跃项目工作区 (❌ Ignored)
│   │   └── archive/            #   🏁 已完成归档 (❌ Ignored)
│   ├── rules/                  # 📏 全局 Prompt 规则 (✅ Git Tracked)
│   ├── skills/                 # 🛠️ Agent 技能模块 (✅ Git Tracked)
│   ├── workflows/              # 🔄 常用 AI 工作流脚本 (✅ Git Tracked)
│   └── context/                # 💾 私有记忆 (❌ Ignored)
├── .workspace/                 # 🟢 [核心] 工作区定义文件
│   ├── base.code-workspace     # 基础模版 (作为继承源)
│   └── full.code-workspace     # 全局管理入口
├── .vscode/                    # 🔴 [已废弃] 仅留存根
├── my-app/                     # 📦 您的实际项目 (❌ Ignored - Managed separately)
├── LICENSE                     # MIT License
└── README.md
```

## 🧠 智能规则配置 (Smart Rules)

本工作区支持**基于上下文的规则加载**，以避免 Prompt 污染。

### 1. 配置标签 (Workspace Tags)

在您的 `.code-workspace` 文件中定义该工作区需激活的规则标签：

```json
"settings": {
  "agent.active_rule_tags": ["go", "backend", "general"]
}
```

### 2. 定义规则 (Rule Tags)

在 `.agent/rules/` 下的规则文件头部添加 YAML Frontmatter：

```yaml
---
tags: ["go", "backend"]
trigger: model_decision
---
```

### 3. 加载规则 (Load Rules)

在对话开始时，通过 `/load-rules` 工作流主动加载当前工作区匹配的规则：

```bash
/load-rules <workspace-name>
# 例如：/load-rules amb-go-seller
```

> **执行原理**: 触发 `.agent/workflows/load-rules.md` 后，Agent 会根据 `rule-loader` Skill 读取对应工作区配置中的 Tags，与规则文件的 Tags 取交集，并智能注入所有匹配的业务规则。

> **默认策略**: 若工作区未配置 Tag，系统默认提取 `["core", "general"]` 标签的规则。

## 🤝 Contributing

欢迎提交 Issue 或 PR 分享您觉得好用的 `.agent/rules` 或工作流！

## License

[MIT](LICENSE)
