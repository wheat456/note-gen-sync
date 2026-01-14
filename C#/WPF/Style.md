style是写在resources中的
在style中写setter



隐式样式
自动的应用到所属的类型上面
```xml
<Style TargetType="Button">
    <Setter Property="FontSize" Value="20" />
    <Setter Property="Margin" Value="10" />
</Style>

<Button Content="我是自动变大的" />
<Button Content="我也是" />
```



触发器
触发器与setter并列
先写.trigger,再写trigger，trigger和setter一样都可以设置property
```xml
<Style TargetType="Button">
    <Setter Property="Background" Value="LightGray" />
    
    <Style.Triggers>
        <Trigger Property="IsMouseOver" Value="True">
            <Setter Property="Background" Value="Orange" />
            <Setter Property="Foreground" Value="White" />
        </Trigger>
    </Style.Triggers>
</Style>
```
多重触发器
在style.trigger中设置，还要设置MultiTrigger.Conditions条件，条件中有Condition
```xml
<Style TargetType="Button">
    <Setter Property="Background" Value="LightGray" />
    <Style.Triggers>
        <MultiTrigger>
            <MultiTrigger.Conditions>
                <Condition Property="IsMouseOver" Value="True" />
                <Condition Property="IsEnabled" Value="True" />
            </MultiTrigger.Conditions>
            
            <Setter Property="Background" Value="Green" />
            <Setter Property="Foreground" Value="White" />
        </MultiTrigger>
    </Style.Triggers>
</Style>
```
如果要实现或操作，那么就用两个trigger

datatrigger
不会使用property，而是绑定到后面的数据，当满足条件时，就触发
```xml
<DataTrigger Binding="{Binding 属性名}" Value="期望的值">
    <Setter Property="属性" Value="新值" />
</DataTrigger>
```

转换器
可以通过mvvm 的计算属性来解决，通过监控属性来通知计算属性更新
因为value不能进行运算，比如比较大小所以要使用转换器
```xml
<DataTrigger Binding="{Binding Price, Converter={StaticResource MoreThan100Converter}}" Value="True">
    <Setter Property="Foreground" Value="Red" />
</DataTrigger>
```
可以加转换参数
```xml
<DataTrigger Binding="{Binding Price, Converter={StaticResource MyLimitConverter}, ConverterParameter=200}" Value="True">
```

多重数据trigger
```xml
<MultiDataTrigger>
    <MultiDataTrigger.Conditions>
        <Condition Binding="{Binding IsAdmin}" Value="True" />
        <Condition Binding="{Binding IsLocked}" Value="True" />
    </MultiDataTrigger.Conditions>
    <Setter Property="IsEnabled" Value="False" />
</MultiDataTrigger>
```

样式中可以定义一个私有的resources只有当前的style能用
```xml
<Style.Resources> 
<SolidColorBrush x:Key="HoverBrush" Color="#1976D2"/> 
</Style.Resources>
```

tag
就像text一样，只不过不显示

事件触发器
用于触发动画
```xml
<Style.Triggers>
    <EventTrigger RoutedEvent="MouseEnter">
        <BeginStoryboard>
            <Storyboard>
                <DoubleAnimation Storyboard.TargetProperty="Opacity" 
                                 To="0.5" Duration="0:0:0.2" />
            </Storyboard>
        </BeginStoryboard>
    </EventTrigger>
</Style.Triggers>
```

多重绑定
将多个数据源重新格式化一下再给value
```xml
<Setter Property="ToolTip">
    <Setter.Value>
        <MultiBinding StringFormat="用户：{0}，等级：{1}">
            <Binding Path="UserName" />
            <Binding Path="UserLevel" />
        </MultiBinding>
    </Setter.Value>
</Setter>
```

输入绑定
用于捕获键盘和鼠标的输入
```xml
<Style TargetType="TextBox">
    <Setter Property="InputBindings">
        <Setter.Value>
            <InputBindingCollection>
                <KeyBinding Key="S" Modifiers="Control" Command="{Binding SaveCommand}" />
                <MouseBinding MouseAction="LeftDoubleClick" Command="{Binding OpenCommand}" />
            </InputBindingCollection>
        </Setter.Value>
    </Setter>
</Style>
```

CommandBindings
用于捕获那些广播命令


# 步骤
1. 写style 作用的类型
	- 在这里还可以使用baseon继承样式语法`{StaticResource {x:Type 控件名}}`
	- style上还可以设置key
```xml


<Style x:Key="BlueBtn" TargetType="Button" BasedOn="{StaticResource {x:Type Button}}">                                              这里
    <Setter Property="Background" Value="Blue" />
</Style>

<Style TargetType="Button">
    </Style>
    
```
2. 使用setter设置属性
```xml
<Style TargetType="Button">
    <Setter Property="Background" Value="Blue" />
    <Setter Property="FontSize" Value="14" />
</Style>
```
3. 如果要处理交互的逻辑就用style.trigger
```Xml
<Style TargetType="Button">
    <Setter Property="Background" Value="Blue" />
    
    <Style.Triggers>
        <Trigger Property="IsMouseOver" Value="True">
            <Setter Property="Background" Value="Red" />
        </Trigger>
    </Style.Triggers>
</Style>
```
4. 与上面是或的关系，重写控件模板就要setter设置template属性，然后放置一个controltemplate
```xml
<Setter Property="Template">
    <Setter.Value>
        <ControlTemplate TargetType="Button">
            <Border CornerRadius="10" Background="{TemplateBinding Background}">
                <ContentPresenter HorizontalAlignment="Center" />
            </Border>
        </ControlTemplate>
    </Setter.Value>
</Setter>
```