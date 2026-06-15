``` ts 
const {
  headerRows,
  rows,
  tableAttrs,
  tableBodyAttrs,
} = table.createViewModel(columns);
```

```html
<table>
    <thead>
        <tr>
            <th></th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td></td>
        </tr>
    </tbody>
</table>
```

**`headerRows` (表头行集合)**
- 代表表格的头部行数据（即 `<thead>` 内部的行）。
- 它是一个数组，包含每一行表头的信息。如果你的表格有表头合并（列分组），它会包含多行。
- 你需要遍历它来渲染 `<tr>` 和 `<th>` 标签。

**`rows` (数据行集合)**

- 代表表格的主体数据行（即 `<tbody>` 内部的行）。
- 它是一个数组，包含了根据你的原始数据（Data）和列定义（Columns）过滤、排序、分页后最终要显示的每一行对象。
- 你需要遍历它来渲染 `<tr>` 和 `<td>` 标签。


**`tableAttrs` (表格基础属性)**

- 一个包含表格标签所需 HTML 属性的对象（例如 `role="table"` 等可访问性属性）。
- 使用时需要通过 Svelte 的 `{...$tableAttrs}` 展开语法直接传给最外层的 `<table>` 标签。
**`tableBodyAttrs` (表体属性)**

- 一个包含表体标签所需 HTML 属性的对象（例如 `role="rowgroup"`）。
- 使用时需要通过 `{...$tableBodyAttrs}` 展开语法直接传给 `<tbody>` 标签。



# 属性展开语法
1. 首先，定义一个对象
```ts
	 const myAttrs = {
    id: "main-table",
    class: "striped-table",
    "data-version": "1.0"
};

```

2.  如果还在标签上手写了属性那么就会覆盖
```html
<table {...myAttrs}>
</table>
```