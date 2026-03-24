```ts
import {createPinia} from 'pinia'
const pinia = createPinia()
app.use(pinia)
```
在src下，创建store文件夹，创建需要存储的组件脚本比如`Sum.ts Count.ts`
在文件中写
```ts
// 组合式示例 (Setup Store)
export const useCountStore = defineStore('count', () => {
  const count = ref(0); // 相当于 state
  
  function increment(val: number) { // 相当于 action
    count.value += val; 
  }

  return { count, increment };
})
```

在其他文件中引入
```ts
import {useCountStore} from '..store/count'
const countStore=useCountStore()
```


# 解构
使用`storeRef()`来解构保证结构的数据也是响应式的,就算再`store`中是`ref`的导出也没有响应的功能了

# 组合式写法
```ts
export const useCountStore=defineStore('count',()=>{
	const talklist = reactive(0)
	function getTalk(){
	}
	return {talklist,getTalk}
})
```


