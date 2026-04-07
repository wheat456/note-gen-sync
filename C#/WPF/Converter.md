实现接口`IValueConverter`
Convert 从数据到ui
ConvertBack 从ui到数据
Binding.DoNothing 遇到错误不会更新到新的值
直接在resources里面用local引用，然后定义key
主要用于简单的值转换比如0，1转换为bool



可以在要转换的类上加上属性
```cs
[TypeConverterAttribute(typeof(xxxx))]
```

```xml
<x:Code>
   <![CDATA[
	    这里可以写c# 代码
    ]]>
</x:Code>
```
