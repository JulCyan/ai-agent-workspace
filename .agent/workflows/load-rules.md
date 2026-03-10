---
description: 加载项目规则 - 读取 workspace 配置并注入匹配的 .agent/rules
---

# /load-rules [workspace-name]

加载与当前 workspace 匹配的项目规则。

## 用法

```
/load-rules amb-go-seller
/load-rules meta
/load-rules project-example
```

## 步骤

// turbo-all

1. 读取 `rule-loader` Skill：`.agent/skills/rule-loader/SKILL.md`
2. 按 Skill 中定义的 5 步流程执行规则加载
   - 参数 `workspace-name` 对应 `.workspace/<workspace-name>.code-workspace`
   - 若未传参数，检查用户 Active Document 是否为 `.code-workspace` 文件
