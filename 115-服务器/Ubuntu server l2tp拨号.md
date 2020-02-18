# Ubuntu server l2tp拨号

**经测试，使用Ubuntu 14.10 server（utopic）**

**装系统时，不要初始化配置网络参数！！！后期手工配置ip﻿！！！**

需要的文件：

ppp_2.4.7-1+2ubuntu1_amd64.deb

xl2tpd_1.3.6+dfsg-4_amd64.deb

## 1.将下载好的xl2tp.deb，ppp.deb上传到服务器，使用dpkg安装

```
dpkg -i 文件名 
```

## 2.将配置文件/etc/xl2tpd/xl2tpd.conf备份后删除，新建一个空的xl2tpd.conf，添加：

```
[lac myvpn]
lns = 132.233.2.68
redial = yes
redial timeout = 15
require pap = no
require chap = yes
require authentication = yes
name = zf.nt@work1
ppp debug = yes
pppoptfile = /etc/ppp/options.xl2tpd.myvpn
```

## 3.新建文件/etc/ppp/options.xl2tpd.myvpn，内容：

```
ipcp-accept-local
ipcp-accept-remote
refuse-eap
noccp
noauth
idle 36000
defaultroute
replacedefaultroute
usepeerdns
lock
mtu 1452
connect-delay 2000
```

## 4.修改/etc/ppp/chap-secrets，输入用户账号和密码

```
# Secrets for authentication using CHAP
# client server secret IP addresses
zf.nt@work1 * zf*%^!% *
```

## 5.切换到root用户，创建内网路由，防止拨号以后网络断开

```
route add -net 132.0.0.0 netmask 255.0.0.0 gw 132.233.33.145
```

5.1 设置开机自动添加路由

```
vim /etc/rc.local

在exit 0前面添加

#添加系统默认路由
route add -net 132.0.0.0 netmask 255.0.0.0 gw 132.233.12.161
```

## 6.切换到root用户，启动xl2tp服务并拨号

```
service xl2tpd start

echo 'c myvpn' > /var/run/xl2tpd/l2tp-control      #启动

echo 'd myvpn' > /var/run/xl2tpd/l2tp-control      #关闭
```

日志文件：`tail -f /var/log/syslog` 可以观察

