**作用**：父组件向子组件单向传递数据


## 子组件
```vue
<!-- Child.vue (子组件 - 基础声明) -->
<script setup>
// 1. 接收 props 并赋值给变量（在 script 中使用）
const props = defineProps(['title', 'likes'])

console.log(props.title)
</script>

<template>
  <!-- 2. 在 template 中可以直接使用字段名，无需加 props. -->
  <div>
    <h3>{{ title }}</h3>
    <p>点赞数：{{ likes }}</p>
  </div>
</template
```


## 父组件传递数据
- 静态字符串直接写，非字符串类型或响应式变量必须加 `v-bind`（简写为 `:`）
- **注意区分**：`likes="100"` 传过去的是**字符串 `"100"`**；`:likes="100"` 传过去的才是**数字 `100`**。
- title 等必须和子组件定义的一样
```vue 
<!-- Parent.vue (父组件) -->
<script setup>
import { ref } from 'vue'
import Child from './Child.vue'

const currentLikes = ref(100)
</script>

<template>
  <!-- 静态字符串：传递纯文本 "Vue 3 核心指南" -->
  <!-- 动态绑定 (:)：传递数字、布尔值、对象或响应式变量 -->
  
  <Child 
    title="Vue 3 核心指南" 
    :likes="currentLikes" 
  />
</template>
```


# 在TS中的规范

```vue 
<script setup lang="ts">
interface Props {
  title: string
  likes?: number
  tags?: string[]
}

// 结合 withDefaults 为可选属性设置默认值
const props = withDefaults(defineProps<Props>(), {
  likes: 0,
  tags: () => ['Vue', 'Frontend']
})
</script>
```

vue3.5结构后还有响应性
```ts
const { msg } = defineProps(['msg'])
```
解构是一次性值拷贝，切断了和响应式 props 对象的关联

