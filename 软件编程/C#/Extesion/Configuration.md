# 使用步骤
1. 构建`Builder` `new ConfigurationBuilder()`
2. 设置基本路径 `.SetBasePath(Directory.GetCurrentDirectory())`
3. 添加文件 `.AddJsonFile("appsettings.json", optional: false, reloadOnChange: true)`
	1. 添加文件时也可以用路径
4. 构建`builder.Build();`
```cs
var config = new ConfigurationBuilder() .SetBasePath(Directory.GetCurrentDirectory()) .AddJsonFile("appsettings.json", optional: false, reloadOnChange: true) .Build();
```

5. 通过**变量\[key]** 来拿值
```cs
string title = config["AppSettings:Title"];
```


# 绑定
- 其中**GetSection("AppSettings")** 是指定的**Json**中的一个**Key**，比如`AppSettings:{xxx}`
- `Get<AppSettings>();` 指定的是类型
- 如果是从根上取的，可以忽略`GetSection`
```cs
var mySettings = config.GetSection("AppSettings").Get<AppSettings>();
```

# 添加多个路径
如果要添加多个路径下的文件，且不想字段互相干扰，就只能创建多个Config 对象，可以装在集合之中