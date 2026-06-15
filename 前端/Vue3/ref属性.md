**为了解决多个组件id相同的问题**
```js
<div ref="name">
</div>

<script>

let name= ref();
<script/>
```

**ref** 在**script**中要使用`value`,在`template`中不用
