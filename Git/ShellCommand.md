# git branch
- `-M main` 用于重命名当前分支
# git fetch
- `git fetch`从一个或多个远程仓库下载所有你还没有在本地仓库中的引用更新,**不会自动合并**
# git pull
- 自动合并
# git check
- `-b temp` 创建新的分支temp
# git reset
- 作用：已出暂存区
- `git reset xx.txt` 解除跟踪
# git rm
- `--cached sss.txt` 解除跟踪 
- #取消控制
# git remote add
- `origin xxx` origin代表仓库的名字，xxx是连接
# git config 
- ` --local user.name`记录你的身份信息，在commit时提供
# 配置当前终端的用户信息
`git config user.name "clsout"`
`git  cofig user.email "x"`
`git config credential.username "closout"`
# Cherrypick
将某次或几次的提交直接合并到当前分支
# 提交
`git commit -m "注释"`
# 添加
`git add -f xxx.txt` 添加一个文件
