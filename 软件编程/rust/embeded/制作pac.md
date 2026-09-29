1. 创建工作区
	1. 创建一个文件夹
	2. 在里面使用git init
	3. 在vscode 中启用相关的检查
	4. 讲memory.x 文件写在工作区中
	5. 将.cargo.toml 中协商 -c link ,可以查看cortex-m-rt 中的示例
	6. 在cargo.toml 中写上
```toml
 [workspace]

resolver = "2"
members = [
    "app",
    "pac",
]
``` 

2. 创建app bin crate
	1. 正常在里面添加 `cortex-m  -rt  panic_halt`
3. 创建pac lib crate
	1.  在其中通过svd2rust 和form生成src
	2. 补充cargo.toml 
```toml
cortex-m = {version="0.7.9"}
cortex-m-rt = {version="0.7.7",optional = true }
critical-section = "1.2.0"
vcell = "0.1.3"
[features]
rt=["cortex-m-rt/device"]
critical-section=[]
```