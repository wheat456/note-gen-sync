```ts
import {createPinia} from 'pinia'
const pinia = createPinia()
app.use(pinia)
```
在src下，创建store文件夹，创建需要存储的组件脚本比如`Sum.ts Count.ts`
在文件中写
```ts
import {defineStore} from ‘pinia'
export const useCountStore=defineStore('count'{
	state(){
		retturn {
		
		}
	}
	
	action：{
		函数名(参数){
			this.xxx //this 指代useCountStore 在其他文件中使用countStore.函数名调用
		}
	}

})
```

在其他文件中引入
```ts
import {useCountStore} from '..store/count'
const countStore=useCountStore()
```

# 批量修改
```ts
countStore.$patch({
	sum:88,
	name:'niaho'
})
```


# 解构
使用storeRef()来解构保证结构的数据也是响应式的

# getters
- 可以用来操作state
```ts
gettters：{
	函数名(state){
		return xxx
	}
}
//也可以使用this
```

# 组合式写法
```ts
export const useCountStore=defineStore('count',()=>{
	const talklist = reactive(0)
	function getTalk(){
	}
	return {talklist,getTalk}
})
```


