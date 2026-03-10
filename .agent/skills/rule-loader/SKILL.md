---
name: rule-loader
description: 在对话开始时加载项目规则。当用户执行 /load-rules 或需要加载项目特定规则时使用。读取 .code-workspace 中的 agent.active_rule_tags，与 .agent/rules/ 下规则文件的 tags 做交集匹配，加载符合条件的规则。
---

# Rule Loader

基于 `.code-workspace` 配置动态加载 `.agent/rules/` 下的规则文件。

## Workflow

### Step 1: 确定 Workspace 文件

用户通过 `/load-rules <workspace-name>` 传入 workspace 名称（不含 `.code-workspace` 后缀）。

读取对应文件：`.workspace/<workspace-name>.code-workspace`

如未传参数：

- 检查用户 Active Document 是否为 `.code-workspace` 文件，若是则使用
- 否则提示用户指定 workspace 名称

### Step 2: 提取 active_rule_tags

从 workspace 文件中提取 `settings["agent.active_rule_tags"]` 数组。

> 注意：`.code-workspace` 是 JSONC 格式（含注释），读取时忽略 `//` 注释行，手动解析 JSON 值。

**Fallback**：若字段不存在或解析失败，使用默认值 `["general", "core"]`。

### Step 3: 扫描规则文件

使用 `find_by_name` 扫描以下目录中所有 `.md` 文件：

- `.agent/rules/*.md`
- `.agent/rules/guides/*.md`

### Step 4: 匹配与加载

对每个规则文件，读取其 YAML frontmatter 中的 `tags` 和 `trigger` 字段：

| 条件                                      | 操作           |
| ----------------------------------------- | -------------- |
| `trigger: always` 或 `trigger: always_on` | **无条件加载** |
| `tags` 与 `active_rule_tags` 有交集       | **加载**       |
| `tags` 无交集，或无 `tags` 字段           | **跳过**       |

对匹配的文件执行 `view_file` 读取完整内容并记住。

### Step 5: 输出摘要

输出规则加载结果表格：

```
✅ 已加载：core.md (trigger: always)
✅ 已加载：project-fe-go-seller.md (tag match: fe-go-seller)
⏭️ 已跳过：project-example.md (无匹配 tag)
⏭️ 已跳过：guides/i18n.md (无匹配 tag)
```
