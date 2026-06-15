在子组件中
```ts
<svelte:options runes />
<script>
  // 把 value 声明为可双向绑定的 prop
  let { value = $bindable('') } = $props();
</script>

<!-- 内部绑定到原生 input -->
<input bind:value={value} />

<script lang="ts">
  let { value = $bindable('') }: { value?: string } = $props();
</script>

```
- `$bindable(默认值)`：标记该 prop 支持 `bind:`，并给默认值Svelte。
- 没写 `$bindable` 的 prop 是**单向**的，子组件改了会报警告。