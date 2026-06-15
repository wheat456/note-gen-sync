直接创建要给类库，然后把类给封装进去

点击引入的 DLL，在属性窗口确认 **复制本地 (Copy Local)** 为 **True**。这样打包时 DLL 才会出现在 `.exe` 旁边

# 调用外部的dll
```cs

[DllImport("MyNativeLib.dll",, CallingConvention = CallingConvention.Cdecl))]
public static extern void GetGreeting(StringBuilder buffer);
```