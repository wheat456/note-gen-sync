用于将“行为（Behaviors）”或“触发器（Triggers）”附加到普通的 XAML 控件上。
它是连接 UI 元素和命令（Command）的桥梁。
通常你会在 XAML 中看到 `i:Interaction.Triggers` 或 `i:Interaction.Behaviors`。

# 安装
`Microsoft.Xaml.Behaviors.Wpf`
建立一个行为的文件夹专门存放行为类
# 行为 (Behaviors)
1. 写一个行为类，以`Behavior`结尾，实现`Behavior<T>` 其中T为实际要作用的类型
```cs
public class ShowMessageBehavior : Behavior<FrameworkElement>
```
2. 定义重写附加函数和取消函数
```cs
    protected override void OnAttached()
    {
        base.OnAttached();
        // 关联事件：鼠标左键点击
        AssociatedObject.MouseLeftButtonDown += OnClick;
    }

    // 行为从控件移除时执行（必须清理事件，防止内存泄漏）
    protected override void OnDetaching()
    {
        base.OnDetaching();
        AssociatedObject.MouseLeftButtonDown -= OnClick;
    }
```
3. 关联对象就是行为作用的对象，可以给它加上回调

# 触发器与操作 (Triggers & Actions)
## 定义触发器
1. 以`Trigger`结尾，继承`TriggerBase<T>`，其中T是作用的类型
```cs
public class DoubleClickTrigger : TriggerBase<FrameworkElement>
```
2. 同样重写附加函数和取消函数
3. 在回调中调用`动作`
```cs
    private void OnDoubleClick(object sender, MouseButtonEventArgs e)
    {
        InvokeActions(e); // 核心！触发动作
    }
```
4. 使用
```xml
<Button Content="双击测试">
    <i:Interaction.Triggers>

        <!-- 你定义的 Trigger -->
        <local:DoubleClickTrigger>

            <!-- 这里放任意 Action -->
            <i:InvokeCommandAction Command="{Binding MyCommand}" />

        </local:DoubleClickTrigger>

    </i:Interaction.Triggers>
</Button>
```

# 定义Action 
1. 以`Action`结尾，继承`TriggerAction<T>`，其中T是作用的类型
```cs
public class MyMessageBoxAction : TriggerAction<FrameworkElement>
```
2. 重写`Invoke` 函数
```cs
    protected override void Invoke(object parameter)
    {
        // 这里就是动作真正执行的地方
        MessageBox.Show("我是 Action，被 Trigger 触发了！");
    }
```
3. （可选）依赖属性
```cs
    public string Message
    {
        get => (string)GetValue(MessageProperty);
        set => SetValue(MessageProperty, value);
    }

    public static readonly DependencyProperty MessageProperty =
        DependencyProperty.Register(
            nameof(Message),
            typeof(string),
            typeof(ShowMessageAction));

    protected override void Invoke(object parameter)
    {
        // parameter 来自 Trigger.InvokeActions(xxx)
        // 比如可以是 MouseButtonEventArgs

        MessageBox.Show(Message);
    }
```


# 官方触发器
- 其中事件名就算WPF中的事件
```cs
<i:EventTrigger EventName="Click">
```
- 还有`<i:DataTrigger>`、`<i:PropertyChangedTrigger>`、`<i:KeyTrigger>`、`<i:TimerTrigger>`



behavior
将一段复杂的逻辑封装成一个可复用的组件，然后像“贴纸”一样贴在 XAML 控件上。
- **`OnAttached()`**: 当行为被贴到控件上时触发。在这里你可以访问 `AssociatedObject`（即被贴的那个控件）并挂载事件。
- **`OnDetaching()`**: 当行为从控件上移除时触发。在这里必须解绑事件，防止内存泄漏。

| `MouseDragElementBehavior`    | 鼠标拖拽                      |
| ----------------------------- | ------------------------- |
| `FluidMoveBehavior`           | 流体移动，比如list重新编排           |
| `DataStateBehavior`           | 数据状态行为，当数据为某一个值时，就执行响应的动作 |
| `GoToStateAction`             | 条件行为                      |
| `TranslateZoomRotateBehavior` | 平移/缩放/旋转，类似地图             |

trigger

### 1. 事件类：监听 UI 的风吹草动

- **`EventTrigger` (最常用)**
    - **用途**：监听任何普通的路由事件（如 `Click`, `MouseEnter`, `Loaded`）。
	    - 比如：`EventName`= Click等，当有事件触发才会执行action
    - **场景**：页面加载时自动播放动画，或点击按钮时触发 ViewModel 的命令。    

- **`KeyTrigger`**
    - **用途**：专门监听键盘按键。    
    - **场景**：按下 `Enter` 键登录，或按下 `Esc` 键退出。
        
- **`StoryboardCompletedTrigger`**
    - **用途**：当某段 XAML 动画（Storyboard）播放结束时触发。
    - **场景**：进场动画播完后，自动显示登录框。
### 2. 数据类：监听数值的变化
- **`DataTrigger`**
    - **用途**：监听绑定的数据属性。当数据等于某个特定值时触发。
    - **场景**：当 `Score > 100` 时，触发屏幕撒花的动画动作。

- **`PropertyChangedTrigger`**
    - **用途**：只要属性值**一变**就触发，不管变成了什么。 
    - **场景**：文本框内容一变，立刻执行一次实时语法检查动作。
        
- **`DataStoreChangedTrigger`**
    - **用途**：这是为 Blend 数据存储设计的高级触发器，用于监听本地数据字典的变化。

### 3. 时间与状态类
- **`TimerTrigger`**
    - **用途**：定时器。每隔一段时间就“闹铃”一次。
    - **场景**：每隔 10 秒切换一次广告位图，或做一个数字时钟。

- **`TriggerCollection`**
    - **用途**：这其实是一个容器。当你需要把好几个触发器打包在一起时会用到，通常在编写复杂的自定义 Behavior 时在后台代码中使用。

