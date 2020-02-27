# ubuntu 更新源配置

先查看ubuntu是什么版本的。

```
cat /etc/os-release
```

```ini
NAME="Ubuntu"
VERSION="14.10 (Utopic Unicorn)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 14.10"
VERSION_ID="14.10"
HOME_URL="http://www.ubuntu.com/"
SUPPORT_URL="http://help.ubuntu.com/"
BUG_REPORT_URL="http://bugs.launchpad.net/ubuntu/"
```

 版本是14.10 Utopic 

备份
sudo cp /etc/apt/sources.list /etc/apt/sources.list.backup﻿

修改
vim /etc/apt/sources.list

最后更新源
sudo apt-get update



deb http://mirrors.ustc.edu.cn/ubuntu-old-releases/ubuntu/ utopic main restricted universe multiverse
deb http://mirrors.ustc.edu.cn/ubuntu-old-releases/ubuntu/ utopic-security main restricted universe multiverse
deb http://mirrors.ustc.edu.cn/ubuntu-old-releases/ubuntu/ utopic-updates main restricted universe multiverse
deb http://mirrors.ustc.edu.cn/ubuntu-old-releases/ubuntu/ utopic-proposed main restricted universe multiverse
deb http://mirrors.ustc.edu.cn/ubuntu-old-releases/ubuntu/ utopic-backports main restricted universe multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu-old-releases/ubuntu/ utopic main restricted universe multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu-old-releases/ubuntu/ utopic-security main restricted universe multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu-old-releases/ubuntu/ utopic-updates main restricted universe multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu-old-releases/ubuntu/ utopic-proposed main restricted universe multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu-old-releases/ubuntu/ utopic-backports main restricted universe multiverse