**用途：** 封装一段数据和函数

在src下建一个hooks文件夹
这些hooks文件通常命名为usexxx.ts
```ts

export function xxx(){
	
	return {x1,x2}

}
```
在其他文件中
直接
```ts
let {y1,y2}=usexxx()
```