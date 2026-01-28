---
description: 传淘宝项目(fe-upload-tb/fe-upload-tb-mobile)自动化验证时的测试数据设置
---

# 传淘宝测试验证规则

## 适用范围

- 项目：`fe-upload-tb`、`fe-upload-tb-mobile`
- 阶段：自动化验证阶段（使用browser_subagent测试功能时）

## 页面使用规则

> [!IMPORTANT]
> **不要自行打开测试页面**，使用用户已打开的传淘宝页面进行测试。
> 通过 `list_browser_pages` 获取已打开的页面列表。

## 测试前置条件

在点击"一键上传"按钮验证功能前，需要按顺序完成以下设置：

### 1. 图片目录选择

- 选择目录：**测试20230706**
- 位置：图片搬家区域的目录选择框

### 2. 图片搬家设置

- 图片搬家区域包含三种子类型：**宝贝**、**属性**、**描述**
- 将每种子类型的图片数量减少至 **3张以内**
- 操作方式：在对应图片区域删除多余图片，每种子类型保留不超过3张

### 3. 商品尺寸表填充

- 如果页面存在尺寸表，需要填充固定测试值：
  - **身高**：从 150 起递增（150, 155, 160, 165...）
  - **体重**：从 45 起递增（45, 50, 55, 60...）

### 4. 上架时间设置

- 选择选项：**放入仓库**
- 位置：页面底部"上架时间"区域

---

## Browser Subagent 可复用脚本

当需要测试"一键上传"功能时，使用以下任务描述：

```
Your task is to prepare the page for upload testing (use already-open page, do NOT navigate):

1. First, find the folder/directory dropdown in the image section and select "测试20230706"
2. In the "图片搬家" section, for EACH subcategory (宝贝/属性/描述):
   - If more than 3 images, click delete buttons to remove extras until only 3 remain
3. If there is a size chart table (尺寸表), fill in test values:
   - Height (身高): start from 150, increment by 5 for each row
   - Weight (体重): start from 45, increment by 5 for each row
4. Scroll to bottom and select "放入仓库" for 上架时间
5. Take a screenshot to confirm settings
6. Click the "继上传" button
7. Wait for validation results and take a screenshot of any error messages

Return: Description of settings applied and any validation messages shown.
```

---

## 注意事项

> [!IMPORTANT]
> 这些设置仅用于测试目的，避免产生真实的上传操作和图片空间占用。
