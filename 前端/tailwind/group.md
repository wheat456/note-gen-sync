`group` 类用于**根据父元素的状态改变子元素的样式**（对应原生 CSS 中的 `.parent:hover .child` 选择器联动），无需编写自定义 CSS。

- **标记父级**：在父元素上添加 `group` 类。
- **修饰子级**：在子元素上添加 `group-{状态}:{样式}`。

```html
<!-- 父元素标记为 group -->
<div class="group p-6 bg-white hover:bg-blue-600 rounded-xl transition duration-200">
  <!-- 当父元素 hover 时，标题变为白色 -->
  <h3 class="text-gray-900 group-hover:text-white font-bold">卡片标题</h3>
  
  <!-- 当父元素 hover 时，图标发生位移并变色 -->
  <svg class="text-gray-400 group-hover:text-white group-hover:translate-x-1 transition">
    ...
  </svg>
</div>
```

# 命名分组
```html
<!-- 外层卡片：标记为 group/card -->
<div class="group/card p-6 bg-white border">
  <h2 class="group-hover/card:text-blue-600">卡片主标题</h2>

  <!-- 内层列表项：标记为 group/item -->
  <div class="group/item flex items-center justify-between p-2 hover:bg-gray-100">
    <span class="text-gray-600 group-hover/item:text-black">单条项目</span>
    
    <!-- 仅在 hover 内层 item 时显示，不受外层 card hover 影响 -->
    <button class="opacity-0 group-hover/item:opacity-100">删除</button>
  </div>
</div>
```