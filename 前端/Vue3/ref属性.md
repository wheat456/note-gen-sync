**为了解决多个组件id相同的问题**
- 通常需要在`script`中操作元素时采用`ref`标记元素
```js
<div ref="name">
</div>

<script>

let name= ref();
<script/>
```

