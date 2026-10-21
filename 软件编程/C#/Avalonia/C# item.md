# window
# usercontrol 
# templatedControl

# resourceDictionary
资源字典（Resource Dictionary）是专门用来存放 **键值对（Key-Value）对象** 的容器。如果把 `Styles` 比作 CSS 样式表，那么 `ResourceDictionary` 就是存储 **设计令牌（Design Tokens，如颜色画刷、间距、字体尺寸、矢量图标、转换器）** 的全局常量池。
引入资源字典
```
<Application xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             x:Class="YourProject.App">

    <Application.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <!-- 引用语法规则：avares://你的程序集名称/路径 -->
                <ResourceInclude Source="avares://YourProject/Resources/Colors.axaml" />
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Application.Resources>

    <Application.Styles>
        <FluentTheme />
        <!-- 这里是之前配置的样式 -->
    </Application.Styles>
</Application>
```

# styles
默认的结构如何下所示,在styels中写内容
```
<Styles xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Design.PreviewWith>
        <Border Padding="20">
            <!-- 可以在这里放预览控件，在 IDE 预览器中实时查看效果 -->
            <Button Classes="primary" Content="预览按钮" />
        </Border>
    </Design.PreviewWith>

    <!-- 样式规则将写在这里 -->
</Styles>
```
