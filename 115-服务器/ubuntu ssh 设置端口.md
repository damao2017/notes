# ubuntu ssh 设置端口

修改方法如下：注意是sshd_config
在`/etc/ssh/sshd_config`中找到Port 22，将其修改为60000

或使用/usr/sbin/sshd -p 60000指定端口。

如果用户想让22和60000端口同时开放，只需在/etc/ssh/sshd_config增加一行内容如下：
[root@localhost /]# vi /etc/ssh/sshd_config
Port 22
Port 60000
保存并退出

[root@localhost /]#service ssh restart