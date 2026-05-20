# @const
- 必须使用 `{@const ...}` 标签形式（注意是以 `@` 开头，而 `{#key}` 或 `{#each}` 是以 `#` 开头）
- 声明的变量是**只读（Constant）**的，无法在模版中对其进行重新赋值。
- 
# Key
**`{#key expression}`（Key 块）**：用于单个区域或组件，目的是为了在值变化时**强制破坏并重建**。


# bind
- 实现**双向绑定**
## 函数绑定
```ts
bind:property={{ get: () => value, set: (newValue) => { value = newValue } }}

```
比如，其中`value`就是`input`当前的输入值，`age`是一个`$state`
```ts
<input 
	type="text"
	bind:value={{
		get: () => age,
		set: (v) => age = Number(v) || 0
	}} 
/>
```

# use:
`use:actionName` 的意思是：“**当这个 DOM 元素被创建出来时，请立刻帮我执行 `actionName(node)` 这个函数。**”

Action 函数还可以接收参数，并且可以返回一个带有 `destroy` 和 `update` 的对象：

- **`destroy`**: 当元素从页面上消失（销毁）时自动触发，适合用来清除定时器、解绑原生事件。
- 在函数中，`return` 一个`destory(){}`
```ts
		return {
			// 2. 元素销毁时：自动触发销毁逻辑
			destroy() {
				node.removeEventListener('scroll', handleScroll);
				console.log('元素已销毁，事件监听已安全移除');
			}
		};
```


- **`update`**: 当你传给 `use:` 的参数发生改变时自动触发。
- `<div use:tooltip={message}>鼠标放上来</div>`,其中`message`是参数
- 在函数中，`return` 一个`update(这里是新的值){}`
```ts
		return {
			// 2. 参数更新时：当外部的 message 改变，此函数自动触发
			update(newText) {
				tip.textContent = newText; // 实时更新提示框内容
			},
			// 3. 销毁时
			destroy() {
				tip.remove();
			}
		};
```


