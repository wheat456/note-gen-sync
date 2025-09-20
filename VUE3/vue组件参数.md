```js
 //给一个组件传参数
 <person a='test'/>
 
 //在peson.vue中接受参数
 import {defineProps} from 'vue'
 let x = defineProps(['a'])
 
 //如果想传变量，则
  <person :a='test'/> //就会找名为test的变量
  
  // 限制接受条件
   let x = defineProps<{list？:Ptype}>(['a']) //?可传可不传
//如果要默认值
withDefaults(defineProps<{list？:Ptype}>(['a']),{
	list:()=>{[xxxx]} 
})
```