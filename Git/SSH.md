# 生成密钥
```shell
ssh-keygen -t ed25519 -C "your_email@example.com"
```
- `-t rsa` 指定~={yellow}密钥的类型=~
- `-b 4096`：指定~={yellow}密钥的位数=~
- `-f /path/to/your/custom/keyname`：指定生成密钥的完整路径和文件名。例如，`C:\Users\21069\.ssh\my_custom_key`（Windows）或 `/home/username/.ssh/my_custom_key`（Linux/Mac）。

# 测试是否连同
````shell
ssh -T git@github.com
````

# 非默认路径的ssh密钥
```shell
eval "$(ssh-agent -s)" # 启动 SSH 代理 
ssh-add F:\\hugo\\ssh\\id_ed25519
```
## 第二种方法
在.ssh目录下创建config
```
Host 002pepsi # 账户名，可以自定义比如 123z,与ssh -T git@123z
    HostName github.com 真实地址
    User git
    IdentityFile C:\Users\21069\.ssh\blog
```

但是在设置远程地址时`git remote set-url origin git@blog:username/repo.git`,blog 这个位置应该与config 中的host 位置对应，这样git 就成功匹配了密钥
# 命令行输入密钥
`“ssh -i” 是在 SSH 命令中用于直接指定密钥路径的选项。通过使用 “ssh -i /path/to/your/private/key user@hostname” 这样的命令格式`
# 显示远程地址
`git remote -v`
# 移除远程地址
`git remote remove origin`
# 添加远程地址
`git remote set-url origin new_remote_url`同时可以修改

```shell
git remote add origin new_remote_url
```
- 