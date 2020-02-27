# ubuntu 防火墙ufw设置

启动、禁用、重置UFW

```
sudo ufw enable
sudo ufw disable
sudo ufw reset
```

查看防火墙状态

```
sudo ufw status
```

默认规则

```
sudo ufw default deny     #关闭所有外部对本机的访问（本机访问外部正常）。
```

增加网段访问

```
sudo ufw allow from 172.16.0.0/12
```

对特定的主机增加特定的端口

```
ufw allow from 132.228.195.205 to any port 10022
```

删除规则

```
ufw status numberedufw delete 3
```

 最长的规则

```
ufw allow proto tcp from 10.0.1.0/10 to 132.233.33.157 port 25
```

 

 

```
root@mysql-master:~# ufw status numbered
Status: active
     To             Action              From
     --             ------               ----
[ 1] Anywhere       ALLOW IN            132.233.0.0/16﻿​
```

From表示从哪个ip或者网段

Action表示动作

To表示要去向的地方，可以是IP，网段，端口和协议

---

一、安装UFW

首先，用如下命令来检查下系统上是否已经安装了UFW。

```
 sudo dpkg --get-selections | grep ufw
```

如还没有安装，可以使用apt命令来安装，如下所示：

```
$ sudo apt-get install ufw
```

在使用前，你应该检查下UFW是否已经在运行。用下面的命令来检查：

```
$ sudo ufw status
```

如果你发现状态是：inactive , 意思是没有被激活或不起作用。

二、使用方法

1、启用

```
sudo ufw enable
sudo ufw default deny #作用：开启了防火墙并随系统启动同时关闭所有外部对本机的访问（本机访问外部正常）。
```

2、关闭

```
sudo ufw disable
```

2、查看防火墙状态

```
sudo ufw status
```

3、开启/禁用相应端口或服务举例

```
sudo ufw allow 80 #允许外部访问80端口
sudo ufw delete allow 80 #禁止外部访问80 端口
sudo ufw allow from 192.168.1.1 #允许此IP访问所有的本机端口
sudo ufw deny smtp #禁止外部访问smtp服务，#以服务名代表端口，可以使用less /etc/services列出所有服务信息, 其中包括该服务使用了哪个端口和哪种协议
sudo ufw delete allow smtp #删除上面建立的某条规则，或者sudo ufw delete allow 80/tcp，如果出现无法删除，可以用序号：sudo ufw status numbered，然后通过序号删除sudo ufw delete 1
sudo ufw deny proto tcp from 10.0.0.0/8 to 192.168.0.1 port 22 #要拒绝所有的TCP流量从10.0.0.0/8 到192.168.0.1地址的22端口
#可以允许所有RFC1918网络（局域网/无线局域网的）访问这个主机（/8,/16是子网掩码）：
sudo ufw allow from 10.0.0.0/8
sudo ufw allow from 172.16.0.0/12
sudo ufw allow from 192.168.0.0/16
```

4、重置所有规则

```
sudo ufw reset
```

 

 5、相关文件

```
#软件配置文件夹/etc/ufw/
root@server-12-164:~# tree /etc/ufw/
/etc/ufw/
├── after6.rules
├── after.init
├── after.rules
├── applications.d
│   └── openssh-server
├── before6.rules
├── before.init
├── before.rules
├── sysctl.conf
└── ufw.conf

1 directory, 9 files

#用户自己的规则文件夹/lib/ufw/
root@server-12-164:~# tree /lib/ufw/
/lib/ufw/
├── ufw-init
├── ufw-init-functions
├── user6.rules
└── user.rules

0 directories, 4 files
```

---

真机设置：

```
sudo ufw status
sudo ufw enable
sudo ufw default deny
sudo ufw allow from 132.233.0.0/16
```

 

 

 