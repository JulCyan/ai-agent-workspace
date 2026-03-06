---
description: project-example 示例项目约束
tags: [project-example]
trigger: model_decision
---

# Project Rules

**项目特定规则（Project-Specific Rules）**

> 全局通用规则请查阅：`core.md` 本文档仅包含项目特有的业务规则

---

## 1. 组件库选择 (Component Library)

**优先级**：复用现有 → Element Plus → Ant Design

**规则**：

- ✅ **第一优先**：复用项目中已有的成熟组件（无论 Element 或 Ant Design）
- ✅ **新组件**：优先使用 Element Plus（项目长期方向）
- ⚠️ **例外**：Element 功能缺失 或 已有成熟 Ant Design 实现广泛使用时，可用 Ant Design（需注释说明原因）

**决策流程**：

```
现有实现存在？
  ├─ 是 → 复用（Element/Ant Design 均可）
  └─ 否 → Element 满足需求？
           ├─ 是 → 用 Element Plus
           └─ 否 → 评估是否必须用 Ant Design（需注释原因）
```

---

## 2. 表格配置 (Table Configuration)

**使用 BaseTable 内置功能，避免手动实现**

### 2.1 文本溢出

- ✅ **正确**：依赖 vxe-table 的 `showOverflow`（自动省略号 + hover 完整内容）
- ❌ **错误**：手动 `formatter` + `substring()` + `'...'`

### 2.2 空值处理

- ✅ **正确**：依赖 BaseTable 的 `formatZeroNullData`（自动将 `null/undefined/''` 转为 `-`）
- ❌ **错误**：手动 `formatter` 返回 `'--'` 或 `'-'`
- 📌 **例外**：需要自定义格式时才用 `formatter`（如货币、日期）

### 2.3 排序

- ✅ **正确**：后端处理排序，前端仅传 `sortField` 和 `sortOrder`
- ❌ **错误**：前端实现排序逻辑（时间范围筛选、字母排序等）
- 📌 配置：`is_sort: true` + `sortType: 'timeRange'|'alphabet'|'amount'`

**示例**：

```typescript
// ✅ 正确：简洁配置，依赖内置功能
{
  key: 'buyerNote',
  name: t('salesOrder.detailDrawer.tables.list.buyerNote'),
  width: 200,
  is_show: true,
  // vxe-table 自动处理溢出和空值
}

// ✅ 正确：仅在需要自定义格式时用 formatter
{
  key: 'totalAmount',
  name: t('salesOrder.detailDrawer.tables.list.totalAmount'),
  width: 120,
  is_show: true,
  formatter: ({ cellValue }) => {
    if (!cellValue) return '--';
    return `¥${parseFloat(cellValue).toFixed(2)}`;
  },
}
```

---

## 快速检查清单

### 组件库

- [ ] 检查了是否有可复用的现有组件？
- [ ] 新组件优先考虑了 Element Plus？
- [ ] 使用 Ant Design 时添加了原因注释？

### 表格配置

- [ ] 避免了手动截断文本？
- [ ] 避免了手动处理空值（除非自定义格式）？
- [ ] 排序逻辑由后端处理？

---

**详细指南**：参考 `.agent/rules/guides/` 目录
