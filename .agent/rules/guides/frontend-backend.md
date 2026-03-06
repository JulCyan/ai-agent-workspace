---
trigger: manual
description: 前端先行开发
---

# 前端后端协作详细指南

本指南提供前端先行开发时的**临时代码管理**最佳实践。

---

## 核心原则

**问题**：前端实现早于后端时，临时代码容易被遗忘。

**解决方案**：注释标记 + 文档追踪 + 定期检查。

---

## 1. 代码标记规范

### 标准格式

```typescript
// TODO(feat/branch-name): 等待后端 API - 功能简述
// Backend: fieldName/apiName
// Track: docs/pending-backend.md#行号
```

### 字段说明

- **TODO(分支名)**：IDE 可自动收集
- **Backend**: 后端需要提供的字段或 API
- **Track**: 追踪文档位置（可选，但推荐）

---

## 2. 常见场景示例

### 场景 1：等待 API 接口

```typescript
// TODO(feat/v1.1.0): 等待后端 API - 订单地区筛选
// Backend: regionIds (number[]), API: salesOrderList
// Track: docs/pending-backend.md#L5
const regionIds = ref<number[]>([]);

const getList = async (params) => {
  const data = await salesOrderList({
    ...params,
    ...(regionIds.value.length > 0 ? { regionIds: regionIds.value } : {}),
  });
  return data;
};
```

### 场景 2：等待后端字段

```typescript
// TODO(feat/v1.1.0): 等待后端字段 - 售后订单数量
// Backend: aftersalesCount (number)
// Track: docs/pending-backend.md#L8
const ORDER_STATUS_COUNT_MAP = {
  unPayCount: 0,
  unSendCount: 1,
  aftersalesCount: 7, // 售后（临时）
};
```

### 场景 3：临时 Mock 数据

```typescript
// TODO(feat/v1.1.0): 临时 Mock - 物流公司数据
// Backend: API /api/logistics/companies
// Track: docs/pending-backend.md#L12
export const MOCK_LOGISTICS_COMPANIES = [
  { id: 1, name: "SF Express" },
  { id: 2, name: "YTO Express" },
];
```

### 场景 4：类型扩展

```typescript
import { SalesOrder } from "@/types/order";

// TODO(feat/v1.1.0): 临时类型扩展 - 售后状态
// Backend: aftersalesStatus (number)
// Track: docs/pending-backend.md#L15
type SalesOrderWithAftersales = SalesOrder & {
  aftersalesStatus?: number;
};
```

---

## 3. 文档追踪

### 更新 `docs/pending-backend.md`

每次添加临时代码时，同步更新追踪文档：

```markdown
| 订单地区筛选 | feat/v1.1.0 | [index.vue#L212](../src/.../index.vue#L212) | regionIds | @张三 | 2025-12-15 | API: salesOrderList |
```

---

## 4. 后端就绪后的清理

### 步骤 1：更新代码

```typescript
// ❌ 移除临时标记
- // TODO(feat/v1.1.0): 等待后端 API - 订单地区筛选
- // Backend: regionIds (number[]), API: salesOrderList
- // Track: docs/pending-backend.md#L5

// ✅ 更新为正式实现
const regionIds = ref<number[]>([]);
```

### 步骤 2：更新文档

从"进行中"移到"已完成"：

```markdown
## ✅ 已完成

| 订单地区筛选 | 2025-12-11 | PR#123 | 后端已提供 regionIds 筛选 |
```

### 步骤 3：代码审查

- 检查是否遗漏其他相关临时代码
- 移除 Mock 数据
- 更新类型定义

---

## 5. 查找临时代码

### IDE TODO 视图

- **VSCode**: `Ctrl+Shift+P` → "TODO Tree: Show"
- **Cursor**: 侧边栏 → TODO 图标
- **WebStorm**: View → Tool Windows → TODO

### 命令行搜索

```bash
# 查找所有待后端的代码
grep -r "TODO(feat/" src/

# 查看追踪文档
cat docs/pending-backend.md
```

---

## 6. 定期检查

### 每日站会

开发人员自查：

```bash
# 我的 TODO 列表
grep -r "TODO(feat/" src/ | grep "我的功能"
```

### Sprint 回顾

- 检查 `docs/pending-backend.md`
- 跟进超过 2 周未完成的项
- 清理已完成但未更新的记录

### 代码审查

审查者检查：

- 是否添加了 TODO 注释？
- 是否更新了追踪文档？
- 是否有遗漏的临时代码？

---

## 7. Mock 数据管理

### 优先使用统一 Mock 服务

**推荐**：`vite-plugin-mock`

```typescript
// src/mock/order.ts
import { MockMethod } from "vite-plugin-mock";

export default [
  {
    url: "/api/order/list",
    method: "get",
    response: () => ({
      code: 200,
      data: {
        records: [
          /* mock data */
        ],
        total: 10,
      },
    }),
  },
] as MockMethod[];
```

### 业务代码 Mock

**仅在必要时**：创建专用 Mock 文件

```typescript
// src/api/order/orderMock.ts
// TODO(feat/v1.1.0): 临时 Mock 数据
export const getMockOrderList = () => {
  return Promise.resolve({
    records: [
      /* mock data */
    ],
    total: 10,
  });
};
```

---

## 检查清单

### 添加临时代码时

- [ ] 添加 `TODO(分支名)` 注释
- [ ] 说明等待的后端字段/API
- [ ] 更新 `docs/pending-backend.md`
- [ ] Mock 数据集中管理
- [ ] 使用本地类型扩展

### 后端就绪后

- [ ] 移除 TODO 注释
- [ ] 替换 Mock 为真实 API
- [ ] 更新类型定义
- [ ] 更新 `pending-backend.md` 为"已完成"
- [ ] 代码审查确认无遗漏

### 定期检查

- [ ] 每日自查 TODO 列表
- [ ] Sprint 回顾检查追踪文档
- [ ] 及时跟进长期未完成项
