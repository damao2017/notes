# ubuntu 设置网络ip和dns

对于Ubuntu和CentOS 6配置都适用。

准备：

```
ifconfig 查看，只有lo端口，没有其他端口
使用
ifconfig -a 查看 能看到端口名称​
```

1、修改IP地址

打开/etc/network/interfaces

```
sudo vim /etc/network/interfaces
```

加入以下语句：

```
auto eth0 #要设置的网卡
iface eth0 inet static #设置静态IP；如果是使用自动IP用dhcp，后面的不用设置，一般少用
addressxxx.xxx.xxx.xxx #IP地址
netmaskxxx.xxx.xxx.xxx #子网掩码
gatewayxxx.xxx.xxx.xxx #网关
```

2、修改DNS

打开/etc/resolv.conf

```
sudo vim /etc/resolv.conf
```

注意：上面设置的文件重启后会覆盖，如果要持久的保存，需要修改：/etc/resolvconf/resolv.conf.d/base

改为如下内容：

```
search localdomain #如果本Server为DNS服务器，可以加上这一句，如果不是，可以不加
nameserver 172.16.3.4 #希望修改成的DNS
nameserver 172.16.3.3 #希望修改成的DNS
```

3、重启服务生效

先运行一次，然后在rc.local里加入这个重启网络配置的命令：

```
sudo /etc/init.d/networking restart #使网卡配置生效
sudo /etc/init.d/resolvconf restart #使DNS生效﻿​
```

 