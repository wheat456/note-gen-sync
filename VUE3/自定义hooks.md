**用途：** 封装一段数据和函数

在src下建一个`composables`文件夹
些**hooks**文件通常命名为usexxx.ts
1. 创建导出函数，命名为`usexxx`
```ts

export function xxx(){
	
	return {x1,x2}

}
```
在其他文件中
```ts
export function useCounter() {
  // 1. 内部状态 (变量)
  const count = ref(0);

  // 2. 内部逻辑 (函数)
  const increment = () => count.value++;
  const reset = () => count.value = 0;

  // 3. 返回大礼包
  return {
    count,      // 变量
    increment,  // 函数
    reset       // 函数
  };
}
```


直接
```ts
let {y1,y2}=usexxx()
```