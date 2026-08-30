核心作用：是**在组件切换过程中将状态保留在内存中，防止重复渲染和重新初始化**。
- **普通切换流程**：
    挂载组件 A $\rightarrow$ 切换到 B $\rightarrow$ **销毁组件 A** $\rightarrow$ 挂载组件 B
- **KeepAlive 流程**：
    挂载组件 A $\rightarrow$ 切换到 B $\rightarrow$ **休眠/缓存组件 A（保留状态）** $\rightarrow$ 挂载组件 B $\rightarrow$ 切回 A $\rightarrow$ **激活组件 A**


component 用法

```vue 
<KeepAlive>
 <component :is="currentTab" />
</KeepAlive>
```

# 条件缓存

**1. `include`（白名单）与 `exclude`（黑名单）**

- **`include`**：只有匹配名称的组件才会被缓存。
    
- **`exclude`**：任何匹配名称的组件都不会被缓存。
    

它们支持三种写法：**逗号分隔的字符串**、**正则表达式**、**数组**。

```vue 
<KeepAlive include="CompA,CompC">
  <component :is="currentTab" />
</KeepAlive>

<!-- 2. 数组写法（推荐，最清晰） -->
<KeepAlive :include="['CompA', 'CompC']">
  <component :is="currentTab" />
</KeepAlive>

<!-- 3. exclude 排除不需要缓存的组件 -->
<KeepAlive :exclude="['CompB']">
  <component :is="currentTab" />
</KeepAlive>

<KeepAlive :max="5"> 最大缓存5个，防止内存占用过高
 <component :is="currentTab" />
  </KeepAlive>
```

# 解决OnMount
当组件被 `<KeepAlive>` 缓存后，常规的生命周期钩子（`onMounted` / `onUnmounted`）在后续切换中**不会重复触发**。

**执行时机对比**

- **首次进入组件**：`onMounted` $\rightarrow$ `onActivated`
    
- **切换离开组件**：`onDeactivated`（**不触发** `onUnmounted`，组件进入休眠并保存在内存中）
    
- **再次切回组件**：`onActivated`（**不触发** `onMounted`，从内存唤醒）
    
- **组件彻底销毁**（如离开父页面或被 `max` 顶掉）：`onDeactivated` $\rightarrow$ `onUnmounted`


在子组件中
```vue 
<!-- CompA.vue -->
<script setup>
import { onMounted, onUnmounted, onActivated, onDeactivated, ref } from 'vue'

const count = ref(0)
let timer = null

// 1. 首次加载执行（只执行 1 次）
onMounted(() => {
  console.log('CompA 首次挂载')
})

// 2. 每次进入/切回该组件时触发
onActivated(() => {
  console.log('CompA 被激活：可以拉取最新数据、启动定时器')
  timer = setInterval(() => {
    count.value++
  }, 1000)
})

// 3. 每次切走/离开该组件时触发
onDeactivated(() => {
  console.log('CompA 进入休眠：暂停定时器、保存临时状态')
  clearInterval(timer)
})

// 4. 彻底销毁时才执行
onUnmounted(() => {
  console.log('CompA 彻底销毁')
})
</script>
```

