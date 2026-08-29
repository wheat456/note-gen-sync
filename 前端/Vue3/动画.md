`<Transition>` 组件的核心逻辑其实非常纯粹：**它本身不包含动画样式，只是在元素挂载/卸载的关键帧，帮你自动增删特定的 CSS 类名。**
只能`v-if ` `v-else` 才执行


-  **`v-enter-from`（起点）**：元素刚插入 DOM 的第一瞬间。比如：`opacity: 0`。
- **`v-enter-active`（过程）**：整个进入过程持续生效。用于写过渡属性，比如：`transition: opacity 0.5s ease`。
- **`v-enter-to`（终点）**：动画播放完毕时的最终状态。比如：`opacity: 1`。

> [!NOTE] Title
>  enterfrom 就是定义起始状态是怎么样的
>  active 就是定义过渡样式
>  to就是最终停留的状态

|类名|触发时机|
|---|---|
|`v-enter-from`|进入动画**开始前**，元素刚插入，马上加上；enter 动画开始瞬间移除|
|`v-enter-active`|进入**整个过程都存在**；动画结束移除（放 transition/animation 属性）|
|`v-enter-to`|进入动画第一帧；动画结束移除|
|`v-leave-from`|离开动画触发瞬间加上；离开开始立刻移除|
|`v-leave-active`|离开**整个过程都存在**；动画结束移除|
|`v-leave-to`|离开动画第一帧；动画结束移除|
```css
/*简写*/
.v-enter-from,
.v-leave-to{

  opacity: 0;
  transform: translateY(-20px);

}

```
- `appear` 在首次进入时也执行transition
- 如果不想用`enter` `leave`动画可以自定义
```js
<Transition
  appear
  name="fade"
  appear-active-class="custom-appear-active"
  appear-from-class="custom-appear-from"
>
```
# 自定义样式名
```js
 <Transition name="fade">

<style scoped>
.fade-enter-from,
.fade-leave-to{
  opacity: 0;
}
</style>
```

# 解决动画冲突
- 先让出去动画先执行，让进入动画后执行
```js
 <Transition name="slide" mode="out-in"">
  <button v-if="show">save</button>
  <button v-else>edit</button>
 </Transition>
```


# TransitionGroup
- 对于容器中的各个`item `都必须要有`key`
- 在`TransitionGroup` 上用`tag` 定义容器
- 其中每一项的动画和单元素动画一样都是`*-enter-active`等
- 在移除的元素动画上需要配置`position: absolute;` 否则最后会跳变
- `list-move` 如果使用这个那么其他元素的移动都是瞬间的，没有动画
```js
  <TransitionGroup name="list" tag="ul">
    <li v-for="item in items" :key="item">
      {{ item }}
    </li>
  </TransitionGroup>
  
  <style>
/* 1. 进入和离开的动画过程（和单元素一样） */
.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}

.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}

/* 2. 🌟 核心：为移动中的元素添加平滑过渡 */
.list-move {
  transition: transform 0.5s ease;
}

/* 3. ⚠️ 必加小技巧：让正在离开的元素脱离文档流，其他元素才能平滑上移补位 */
.list-leave-active {
  position: absolute;
}
  
```

# 路由动画
- transition 无法在router-view的外部包裹，且router-view 必须将渲染的组件解构出来
```vue 
v-slot="{ Component, route }" //实际使用时Component 应该是组件名
```
- 在transition中放置`component` 组件
```vue
   <component :is="Component" :key="route.path" /> //其中:is 让该组件知道渲染什么组件
   route.path 让key随着path变动好触发动画
```

```vue 
<!-- 1. 使用 v-slot 解构出当前要渲染的路由组件 Component 和路由信息 route -->
  <router-view v-slot="{ Component, route }">
    <!-- 2. 包裹 Transition，必须加上 mode="out-in" 避免两页同时存在 -->
    <transition name="page-fade" mode="out-in">
      <!-- 3. 用动态组件渲染目标页面 -->
      <!-- 加上 :key="route.path" 确保路由参数变化时也能触发动画 -->
      <component :is="Component" :key="route.path" />
    </transition>
  </router-view>
```
