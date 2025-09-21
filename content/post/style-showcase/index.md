---
title: 样式测试
description: 用于测试各种元素的显示效果
slug: style-showcase
date: 2025-09-21 7:32:00+0000
categories:
    - Blog
tags:
    - Hugo
    - Trial
    - Frontend
toc: true
draft: false
---

这是一篇专门用于验证文章内部各种元素样式的测试文章。

## 标题层级

### H3 标题

#### H4 标题

##### H5 标题

###### H6 标题

---

## 引用

> 一段带有多行的引用文字，展示圆角毛玻璃容器的效果。支持链接，比如 [Hugo 官网](https://gohugo.io) 以及加粗和斜体。 
>
> 第二段引用，用于测试多段落的间距与排版。
>
> <span class="cite">—— 佚名</span>

> 嵌套引用测试：
>
>> 这是一个第二层级的引用内容。

---

## 列表

- 无序列表项 A
- 无序列表项 B
  - 子项 B-1
  - 子项 B-2

1. 有序列表项 1
2. 有序列表项 2
3. 有序列表项 3

- [ ] 任务列表（未完成）
- [x] 任务列表（已完成）

---

## 表格

| 模块 | 说明 | 状态 |
| --- | --- | --- |
| 标题样式 | 移除左侧强调，优化锚点 | 完成 |
| 引用样式 | 圆角毛玻璃容器 | 完成 |
| 表格样式 | 表头渐变、边框增强、无斑马纹 | 完成 |

> 下面是一个更宽的表格，测试横向滚动与表头背景：

| 字段 | 类型 | 必填 | 说明 | 示例 |
| --- | --- | --- | --- | --- |
| id | string | 是 | 唯一标识 | `abc-123` |
| title | string | 是 | 标题文本 | “GraphRAG 指南” |
| created_at | datetime | 是 | ISO8601 时间 | `2025-09-21T10:00:00Z` |
| tags | array[string] | 否 | 标签列表 | `["Themes","CSS"]` |

---

## 代码与高亮

行内代码：`console.log("glass")`。

```ts
// TypeScript 示例
type Post = {
  id: string;
  title: string;
  createdAt: string;
  tags?: string[];
};

const posts: Post[] = [
  { id: "1", title: "Hello", createdAt: "2025-09-21T10:00:00Z" },
];

console.table(posts);
```

---

## 图片

![示例图片](/bg.jpg)

---

## 数学公式与脚注

行内公式 \(E=mc^2\)。

块级公式：

\[ a^2 + b^2 = c^2 \]

这里有一个脚注引用[^note-1]，再来一个脚注[^note-2]。

[^note-1]: 这是第一个脚注的内容。
[^note-2]: 第二个脚注，用于测试脚注列表的样式。


