---
description: 将 Agent 上下文和 Artifacts 同步到 git 仓库以进行跨设备备份。
---

1.  **定位 Artifacts**: 确认当前的 Brain 目录。
    - 路径: `C:\Users\Admin\.gemini\antigravity\brain\`

2.  **复制到归档目录**:
    - **源**: `C:\Users\Admin\.gemini\antigravity\brain\<current-session-id>` (或根据需要复制所有)
    - **目标**: `d:\Documents\Workspace\.agent\context\`
    - _注意_: 你可能希望将其压缩或按日期组织。

3.  **Git 推送**:
    ```powershell
    cd d:\Documents\Workspace
    git add .agent/context
    git commit -m "Backup agent context"
    git push
    ```

> **注意**: 这是一个手动触发的工作流。未来我们可以将其脚本化。
