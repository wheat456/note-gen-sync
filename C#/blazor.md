一个razor文件叫一个组件
两种情况
- 父传子
	- 参数传递，使用`[parameter]`标记一个变量，然后父组件就可以向这个参数赋值
```xml
<InfoCard Title="My Counter" Content="@currentCount.ToString()" OnResetRequested="ResetCount"/>
```

- 子传父
	- 定义下面这种类型，然后在子组件调用`OnResetRequested.InvokeAsync();`就可以通知父组件监控的值变化，触发相应的回调函数，当invoke中有参数时，那么就要用有参数回调函数来接受
	```cs
	    [Parameter]
    public EventCallback OnResetRequested { get; set; }
    
    <InfoCard Title="My Counter" Content="@currentCount.ToString()" OnResetRequested="ResetCount"/> resetcount是回调函数
	```

- 级联参数 跨组件通信`CascadingValue`