# 使用步骤
1. 构建`Builder` `new ConfigurationBuilder()`
2. 设置基本路径 `.SetBasePath(Directory.GetCurrentDirectory())`
3. 添加文件 `.AddJsonFile("appsettings.json", optional: false, reloadOnChange: true)`
	1. 添加文件时也可以用路径
4. 构建`builder.Build();`
5. 通过**变量\[key]** 来拿值