---
trigger: always_on
tags: [core, general]
---

# Core Rules

**核心开发规则（Core Development Rules）**

> 本文档包含从项目规则中提取的通用核心规范，适用于所有项目。  
> 特定项目的业务规则请查阅 `project.md`。

---

# 🧭 规则优先级（从高到低）

1. **IDE Global Rules（例如:GEMINI.md/Cursor User Rules）**
2. **.agent/rules/core.md（全局基础规则）**
3. **.agent/rules/project.md（项目特定规则）**
4. **.agent/rules/guides/\*（任务类型最佳实践）**

### ⚖️ 冲突处理规则

- 必须遵循上述顺序执行。
- 禁止忽略任何更高优先级规则。

---

# 1. 国际化（i18n）

> ⚠️ 以下规则仅在项目启用 i18n 时生效。

### ✅ 必须使用 i18n 的场景

- 模板文本（template text）
- 按钮文案、标签文字
- 提示文字、用户提示消息
- 弹窗标题

### ❌ 不使用 i18n 的场景

- 代码注释
- `console.log`
- 变量名、类型名
- API 字段名

### 📁 翻译文件维护

- 更新功能时需同步维护所有语言包。

---

# 2. 前端先行开发（Frontend First）

> 当前端实现早于后端 API / 字段时，必须标记并追踪临时代码。

### 📌 示例

```ts
// TODO(feat/branch-name): 等待后端 - 功能简述
// Backend: fieldName/apiName
const tempData = ref([]);
```

---

## 📐 标准流程（三步走）

1. **添加 TODO 注释**  
   使用 `TODO(分支名)` 格式，以便 IDE 自动收集。
2. **登记到追踪文档**  
   写入：`.agent/doc/<project-branch>/pending-backend.md`
3. **后端就绪后清理**  
   移除 TODO、删除临时代码、更新文档。

---

## 🔑 实施关键点

- Mock 数据必须集中管理，不得散落在代码中。
- 类型扩展使用本地类型，不修改全局类型定义。
- 定期清理 pending 文档，避免临时代码长期遗留。

---

## 🗂️ 相关文件

- **临时追踪文档**：  
  `.agent/doc/<project-branch>/pending-backend.md`

- **模板文件**：  
  `.agent/pending-backend-template.md`

- **前后端协作规范**：  
  `.agent/rules/guides/frontend-backend.md`

---
