
# $derived
```ts
<script>
    let count = $state(1);

    // ❌ 错误做法（普通变量）：
    // 页面刚打开时 double 是 2。但以后不管 count 怎么加，double 永远死死固定在 2，不会动了。
    let double = count * 2; 

    //  正确做法（派生状态）：
    // 只要 count 变了，Svelte 5 会自动重新计算，让 double 永远保持是 count 的 2 倍。
    let doubleDerived = $derived(count * 2);
</script>
```

## 填函数
```ts
<script>
    let score = $state(85);

    // 括号里是一个复杂的匿名函数，根据分数判断等级
    let grade = $derived(() => {
        if (score >= 90) return '优秀';
        if (score >= 80) return '良好';
        return '及格';
    }()); // <-- 注意这里的小括号，代表立即执行
</script>

<input type="number" bind:value={score} />
<p>等级：{grade}</p>


<script>
    let firstName = $state("张");
    let lastName = $state("三");

    // 1. 先在外层定义一个普通的拼接函数
    function formatName(first, last) {
        return `${first} · ${last}`;
    }

    // 2. 在 $derived 中调用它
    // 只要 firstName 或 lastName 变了，这个函数就会被重新执行
    let fullName = $derived(formatName(firstName, lastName));
</script>

```