`<component>` 是一个**内置的动态组件容器**。它可以根据数据变化，在同一个位置动态挂载、切换不同的组件，避免编写冗长且难以维护的 `v-if` / `v-else-if` 分支。

```vue
<script setup>
import { shallowRef } from 'vue'
import TabA from './TabA.vue'
import TabB from './TabB.vue'

// 1. 定义当前激活的组件
const currentTab = shallowRef(TabA)
</script>

<template>
  <div class="tabs">
    <button @click="currentTab = TabA">切换到 A</button>
    <button @click="currentTab = TabB">切换到 B</button>
  </div>

  <!-- 2. 动态渲染组件 -->
  <component :is="currentTab" />
</template>
```

`<component>` 可以像普通组件一样传递属性和绑定事件：
```vue
<template>
  <!-- 通用绑定方式 -->
  <component 
    :is="currentTab" 
    :user-id="123"  这里就是传给子组件的Props
    @update="handleUpdate" 
  />
</template>
```

不同组件需要不同Props
```js
const currentProps = computed(() => {
  if (currentView.value === UserCard) {
    return { name: 'Alice', role: 'Admin' }
  }
  return { postId: 42, title: 'Vue 3 指南' }
})
<template> 
<component :is="currentView" v-bind="currentProps" /> 
</template>
```