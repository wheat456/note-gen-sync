# 设置ssh端口
- `nano /etc/ssh/sshd_config`
- 添加`port 65420`, 可以注释22端口使其失效
- 重启`sudo systemctl restart sshd`
