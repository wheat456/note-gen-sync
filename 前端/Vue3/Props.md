- 父传子参数
- 父状态变化可以更新到子变化
```ts
const { msg } = defineProps(['msg'])
```
解构是一次性值拷贝，切断了和响应式 props 对象的关联
