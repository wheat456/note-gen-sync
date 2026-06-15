## 项目结构
```plaintext
├─ Models/              // 存放 Model 相关代码
├─ ViewModels/          // 存放 ViewModel 相关代码
├─ Views/               // 存放 View 相关代码
└─ Services/            // 存放业务服务（可选，复杂项目用）
```
#### 1. Model
- 简单说就是数据原型，比如定义一个人的基本信息：年龄、身高、姓名

#### 2. View

- 写UI 相关的Xaml

#### 定义依赖属性
在 **.cs** 文件中用`propdp` **tab**两次就创建了架构，然后用tab依次调整。
其中 **Myproperty** 就是在**xaml**中的属性
**ownerclass** 写当前所在的类
**PropertyMetadata** 默认值
```cs
        public int MyProperty
        {
            get { return (int)GetValue(MyPropertyProperty); }
            set { SetValue(MyPropertyProperty, value); }
        }

        // Using a DependencyProperty as the backing store for MyProperty.  This enables animation, styling, binding, etc...
        public static readonly DependencyProperty MyPropertyProperty =
            DependencyProperty.Register("MyProperty", typeof(int), typeof(ownerclass), new PropertyMetadata(0));
```

#### 3. ViewModel

- **职责**：暴露供 View 绑定的**数据属性**和**命令**，协调 View 与 Model 的交互（如从 Model 取数据给 View，接收 View 的操作后调用 Model 处理）。
- **特点**：不依赖 View（不知道具体用什么控件展示），但依赖 Model；需要实现属性通知（如 `INotifyPropertyChanged`）让 View 感知数据变化。

#### 绑定
**手动实现属性变化通知**

```cs
public class ViewModel : INotifyPropertyChanged  
{  
private int _count;  
  
public int Count  
{  
get => _count;  
set  
{  
if (_count != value)  
{  
_count = value;  
OnPropertyChanged(nameof(Count));  
}  
}  
}  
  
public event PropertyChangedEventHandler PropertyChanged;  
  
protected virtual void OnPropertyChanged(string propertyName)  
{  
PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));  
}  
}
```

	在使用 **mvvm** 后属性变化的写法, 这个属性是双向的，但是在前端通过mode=twoway来改变
```cs
        private int _count;

        public int Count
        {
            get { return _count; }
            set
            {
                SetProperty(ref _count, value); //需要mvvm框架才可以
            }
        }
```







先写**model->viewModel->view**


# MVVM Toolkit
# 命令
这里需要首字母大写
```cs
        [RelayCommand] 
        public void AddCount()
        {
            Count++;
        }
```

# 数据绑定
### 语法
- UpdateSourceTrigger
	- LostFocus 失去焦点更新 
	- PropertyChanged **实时更新**
	- Explicit **手动更新**
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


  