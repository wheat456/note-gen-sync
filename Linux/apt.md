- 位置`/etc/apt/sources.list`和`/etc/apt/sources.list.d/*.list`
	- `/etc/apt/sources.list`官方软件
	- `/etc/apt/sources.list.d/*.list`第三方软件的源
# 新版
在请创建一个名为 `/etc/apt/sources.list.d/<name>.sources`
```
Types: deb deb-src
URIs: http://deb.debian.org/debian
Suites: trixie trixie-updates
Components: main non-free-firmware
Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg
```