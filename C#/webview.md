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
## vue
1. 定义双方发送的数据类型
```ts
interface WpfMessage {

  type: string;

  payload: any;

}
```

2. 定义导出函数
```ts
 export function useWpfBridge(){
 }
```
	1. 定义接受到数据的处理函数 
```ts
  const handleMessage = (event: any) => {

    const data = event.data as WpfMessage;

    lastMessage.value = data;

  

    if (data.type === "") {

    }

  };
```
	2. 定义挂载和卸载执行的注册监听 
```ts
  

  onMounted(() => {

    if (window.chrome?.webview) {

      // 注册监听

      window.chrome.webview.addEventListener("message", handleMessage);

      // 通知 WPF：Vue 这边已经准备好了

      sendToWpf({ type: "VUE_READY", payload: {} });

    } else {

      console.warn("当前环境不是 WebView2，WPF Bridge 无法启动");

    }

  });

  

  onUnmounted(() => {

    window.chrome?.webview?.removeEventListener("message", handleMessage);

  });
```
	3. 定义发送函数，方便外边调用

