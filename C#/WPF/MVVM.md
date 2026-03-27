#### 1. Model：数据与业务规则的 “仓库”

- **职责**：存放数据、定义数据结构，以及最核心的业务逻辑（如数据验证、计算、持久化等）。
- **特点**：完全独立于 UI，不依赖 View 或 ViewModel，甚至可以在非 UI 场景（如控制台、后端）复用。

#### 2. View：用户界面的 “展示层”

- **职责**：只负责 UI 渲染（布局、控件、样式），通过数据绑定关联 ViewModel 的数据和命令。
- **特点**：几乎不写业务逻辑，代码（如 XAML 的后台代码 .xaml.cs）仅处理纯 UI 相关逻辑（如动画、控件事件转发给命令）。

#### 3. ViewModel：View 与 Model 的 “中间人”

- **职责**：暴露供 View 绑定的**数据属性**和**命令**，协调 View 与 Model 的交互（如从 Model 取数据给 View，接收 View 的操作后调用 Model 处理）。
- **特点**：不依赖 View（不知道具体用什么控件展示），但依赖 Model；需要实现属性通知（如 `INotifyPropertyChanged`）让 View 感知数据变化。

先写**model->viewModel->view**
## 项目结构
```plaintext
├─ Models/              // 存放 Model 相关代码
├─ ViewModels/          // 存放 ViewModel 相关代码
├─ Views/               // 存放 View 相关代码
└─ Services/            // 存放业务服务（可选，复杂项目用）
```

# 数据绑定
### 语法

```xaml
<TextBox Text="{Binding UserName ， Mode= ，UpdateSourceTrigger=}" />
```

### 步骤
1. 将数据从model中拿到
```c#
public partial class PersonViewModel : ObservableObject
{
    private readonly Person _model; // Model 对象

    public PersonViewModel(Person model)
    {
        _model = model;
    }

    public string Name
    {
        get => _model.Name;
        set
        {
            if (_model.Name != value)
            {
                _model.Name = value;
                OnPropertyChanged();
            }
        }
    }

    // 或者直接暴露整个 Model（不推荐，破坏封装）
    // public Person Model => _model;
}
```

  2. 设置 **DataContext**
**在 XAML 中直接声明**
```xaml
<!-- Views/StudentView.xaml -->
<Window 
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:viewModels="clr-namespace:MyApp.ViewModels">
    
    <!-- 设置 DataContext 为 StudentViewModel -->
    <Window.DataContext>
        <viewModels:StudentViewModel />
    </Window.DataContext>
</Window>
```

**在后台代码中设置**
```csharp
// Views/StudentView.xaml.cs
public partial class StudentView : Window
{
    public StudentView()
    {
        InitializeComponent();
        DataContext = new StudentViewModel(); // 关联 ViewModel
    }
}
```

3. 绑定 `<TextBlock Text="{Binding Name}" />

定义依赖属性
在.cs文件中用propdp tab两次就创建了架构
  