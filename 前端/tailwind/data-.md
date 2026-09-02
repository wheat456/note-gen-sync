- 先在vue3上使用`:data-自定义名字="被绑定的变量"`
```js 
<template>
  <!-- 方式 A：绑定枚举值 -->
  <div :data-status="status" class="status-card">
    <p>当前状态：{{ status }}</p>
  </div>

  <!-- 方式 B：绑定布尔值（假值必须设为 undefined 以自动移除属性） -->
  <button 
    :data-selected="isSelected || undefined" 
    @click="isSelected = !isSelected"
    class="select-btn"
  >
    切换选中 
  </button>
</template>
```

- 在css中使用`data-[status=loading]:` 后面跟上css