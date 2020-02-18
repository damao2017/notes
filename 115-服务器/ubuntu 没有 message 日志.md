# ubuntu 没有 message 日志

编辑

```
sudo vim /etc/rsyslog.d/50-default.conf
```

找到

```
#*.=info;*.=notice;*.=warn;\ 
# auth,authpriv.none;\ 
# cron,daemon.none;\ 
# mail,news.none -/var/log/messages﻿
```

将注释去掉

重启日志服务

```
sudo /etc/init.d/rsyslog restart
```

