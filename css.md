# 盒模型
`outline`：不占空间
### 标准盒模型
- `语法` box-sizing: content-box;
- Content
- Padding
- Border
- Margin
- 实际宽度=width +padding +border



### 怪异盒模型
- 语法`box-sizing: border-box;`
- width 就是最终的宽度


# 布局

### display
- content 会让该组件隐形

- **`inline-`（对外形象）**：告诉它的父元素，把我当成一个**普通的行内文本**（就像一个字或一个词）。我可以和其他文字或图片并排站在同一行里，不会霸道地自己换行。
- **`-xxx`（对内统治）**：告诉它内部的子元素，我们在箱子里面依然维持 **`xxx` 的布局规则**（比如 Flexbox 的弹性排列，或者 Grid 的网格划分）。

p,h3{} 将同时应用到两个元素上

颜色
rgba(,,,)最后是透明度
浏览器最小字体大小12px
font-weight 文本粗细 bold bolder ligther 100-900
font-style normal italic
font-family : xxx, xxx
background-image: url("")指定图片
background-repeat 如何平铺
background-position 水平和垂直