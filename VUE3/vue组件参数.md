```js
 //给一个组件传参数
 <Person a='test'/>
 
 //在peson.vue中接受参数
 import {defineProps} from 'vue'
 let x = defineProps(['a'])
```