# 安装
`Microsoft.Web.WebView2`

# 使用
- 在xaml里面定义`wv2`
- 在`grid`中使用`wv2`
```xaml
<wv2:WebView2 Name="webView"/>
```

- 在main.cs中定义 **window_load** 函数来加载 **webview**
	- `private async void Window_Loaded(object sender, RoutedEventArgs e)`
	- 函数中的参数是必须的，这是这个委托的格式

```cs
private async void InitializeWebView()
    {
        // 1. 等待初始化完成
        await myWebView.EnsureCoreWebView2Async();

        // 2. 此时 CoreWebView2 属性才可用，可以进行配置
        myWebView.CoreWebView2.Settings.IsPasswordAutofillEnabled = false;

        // 3. 导航到目标页面
        myWebView.Source = new Uri("https://www.google.com");
    }
```
 `await myWebView.EnsureCoreWebView2Async();` 异步初始化webview
 `myWebView.Source = new Uri("https://www.google.com");` 导航到一个网站

### 连接本地网页
如果想加载自己本地的网页，首先就应该找到本地的路径
```cs
`string distPath = System.IO.Path.Combine(AppDomain.CurrentDomain.BaseDirectory, "web_dist");`
```
然后就将路径与自定义域名绑定
```cs
webView.CoreWebView2.SetVirtualHostNameToFolderMapping( "myapp.local", distPath, CoreWebView2HostResourceAccessKind.Allow );
```
然后访问自定义域名
```cs
webView.Source = new Uri("https://myapp.local/index.html");
```
`
# 前端调用后端
1.  首先，在c# 创建一个类用来包含那些可以被前端调用的函数
2. `[ComVisible(true)]` 用这个在类上，打上标记
3. `webView.CoreWebView2.AddHostObjectToScript("myBridge", new Bridge());`
	- 注入到前端，mybrige 是前端拿到的对象，`const bridge = window.chrome.webview.hostObjects.myBridge;`
	- 前端调用后端函数 
`await bridge.ShowMessage("Hello WPF!");`
4. 如果想要c# 在非ui上运行，那么就使用 `async Task`



# 桥接
## vue
1. 首先在 Vue Ts 没有代码提示，需要自己在env.d.ts中定义全局接口
	1. **`window.chrome`**: 检查当前是否在类 Chrome/Chromium 内核的环境中运行。
	2. **`window.chrome.webview`**: 检查是否处于 **Microsoft WebView2** 容器内。如果是普通的 Edge 浏览器访问网页，这个对象是不存在的。
	3. **`window.chrome.webview.hostObjects`**: 检查是否有 C# 后端“注入”的对象。只有当 C# 执行了 `AddHostObjectToScript` 后，这个属性才可用。
```ts
declare global {
  interface Window {
    chrome: {
      webview: {
             
        hostObjects: {
	        //如果想要更好的提示
	        bridge:{
		        // 这里是后端的函数类型
		        add(a:number, b:number):promise<string>
	        }
        };

      };
    };
  }
}
```
2.  拿到 `bridge`
	1. `const bridge= window.chrome.webview.hostObjects.bridge`
	2. 通过 `bridge` 来 调用函数
3. 可以封装一个 `bridge` 来用
```ts
// src/composables/useNative.ts
export function useNative() {
  const isWebView = !!window.chrome?.webview;
  
  // 代理 C# 对象
  const nativeBridge = window.chrome?.webview?.hostObjects?.bridge;

  const callNative = async (msg: string) => {
    if (!isWebView) {
      console.warn("当前不在 WebView2 环境中");
      return;
    }
    return await nativeBridge.CallFromVue(msg);
  };

  return { isWebView, callNative };
}
```



4. 定义双方发送的数据类型
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

