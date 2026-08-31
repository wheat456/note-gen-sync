- 作用：将 组件中的的内容放到其他地方去渲染，比如body
- `to` 属性接收一个 CSS 选择器字符串（如 `"body"`、`"#modal-target"`）或直接传入一个 DOM 元素对象。
- 当 `:disabled="true"` 时，插槽内容不会被传送到目标节点，而是**直接就地渲染**在当前组件的原位置。
```vue 
<Teleport to="body" :disabled="isMobile">
    <div v-if="isOpen" class="modal-overlay">
      <div class="modal-content">
        <h3>系统提示</h3>
        <p>这是一个脱离了父级 DOM 限制的弹窗。</p>
        <button @click="isOpen = false">关闭</button>
      </div>
    </div>
  </Teleport>
```

# 多个teleport挂载

```vue
<Teleport to="#notification-root">
  <div>通知 A：第一条消息</div>
</Teleport>

<!-- 组件 B -->
<Teleport to="#notification-root">
  <div>通知 B：第二条消息（排在 A 下方）</div>
</Teleport>
```
