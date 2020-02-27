# ubuntu rc.local  开机运行命令  不执行命令

 添加到/etc/rc.local的exit 0 前面 

```bash
ntkd@ubuntu-12-14:~$ cat /etc/rc.local 
#!/bin/sh -e
#
# rc.local
#
# This script is executed at the end of each multiuser runlevel.
# Make sure that the script will "exit 0" on success or any other
# value on error.
#
# In order to enable or disable this script just change the execution
# bits.
#
# By default this script does nothing.

#添加默认路由
route add -net 132.0.0.0 netmask 255.0.0.0 gw 132.233.12.161

sleep 1
#启动nexus
/opt/nexus/nexus-2.14.9-01/bin/nexus start

exit 0

```

 发现/opt/nexus/nexus-2.14.9-01/bin/nexus start启动不起来 

 查看错误日志 cat boot.log﻿

 nexus 不能以root用户运行，所以必须在rc.local中以普通用户运行 

```bash
#启动nexus
su - ntkd /opt/nexus/nexus-2.14.9-01/bin/nexus start
```

