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


