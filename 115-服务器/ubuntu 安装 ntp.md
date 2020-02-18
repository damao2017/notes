# ubuntu 安装 ntp

```bash
安装软件
ntkd@ubuntu-12-14:~$ sudo apt-get install ntp

查看软件是否运行
ntkd@ubuntu-12-14:~$ ps -aux | grep ntp
ntp       1678  0.0  0.1  31496  4472 ?        Ss   15:58   0:00 /usr/sbin/ntpd -p /var/run/ntpd.pid -g -u 105:113
ntkd      1692  0.0  0.0   8180  2120 pts/1    S+   15:59   0:00 grep --color=auto ntp

ntkd@ubuntu-12-14:~$ service --status-all
或者
ntkd@ubuntu-12-14:~$ service ntp status 


NTP配置文件/etc/ntp.conf
主要配置这里，添加可以访问服务的网段

# Clients from this (example!) subnet have unlimited access, but only if
# cryptographically authenticated.
#restrict 192.168.123.0 mask 255.255.255.0 notrust
restrict 132.0.0.0 mask 255.0.0.0 nomodify

重启服务
root@ubuntu-12-14:~# service ntp restart 
 * Stopping NTP server ntpd     [ OK ] 
 * Starting NTP server ntpd     [ OK ]

观察运行情况
root@ubuntu-12-14:~# watch ntpq -p
root@ubuntu-12-14:~# ntpq -p


```

客户机linux对时

 ntpdate 132.233.12.164

客户机windows对时

双击时间-internet时间-与internet时间服务器同步，输入132.122.12.164

xp或者2003直接点应用，会立马更新