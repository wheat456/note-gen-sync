# 安装
`Microsoft.Web.WebView2`

# 使用
- 在xaml里面定义`wv2`
- 在`grid`中使用`wv2`
```xaml
<wv2:WebView2 Name="webView"/>
```

- 在main.cs中定义window_load函数来加载webview
`private async void Window_Loaded(object sender, RoutedEventArgs e)`函数中的参数是必须的，这是这个委托的格式
- 拿到dist路径
`string distPath = Path.Combine(AppDomain.CurrentDomain.BaseDirectory, "ui", "dist");`
- 设置虚拟域名
```
            webView.CoreWebView2.SetVirtualHostNameToFolderMapping(
                    "myvueapp.local",
                    distPath,
                    CoreWebView2HostResourceAccessKind.Allow);
webView.Source = new Uri("https://myvueapp.local/index.html");
```



# 桥接
1. 创建一个NativeBridge类
```cs
    [ComVisible(true)]
    public class NativeBridge
    {
		//这里都是写前端调用函数
    }
```

2. 在main.cs中注入对象
```cs
webView.CoreWebView2.AddHostObjectToScript("bridge", new NativeBridge());
```


