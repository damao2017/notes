# win7 搭建 ntp server 配置

NTP是网络时间协议(Network Time Protocol)，它是用来同步网络中各个计算机的时间的协议。它的用途是把计算机的时钟同步到世界协调时UTC，其精度在局域网内可达0.1ms，在互联网上绝大多数的地方其精度可以达到1-50ms。为了保证同一个网络内的设备保持时间的一致，所以部署NTP服务器做时钟源，所有的网络设备都向NTP服务器校时。在Win Server 2012服务器搭建NTP服务如下：

1、打开运行窗口，输入regedit命令后点击确定，进入注册表。



2、打开注册表，进入目录：

HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\Parameters，把 Type设定值修改为”NTP”。



3、进入如下目录：

HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpServer ，把Enabled数值修改为1。



4、打开运行窗口，输入services.msc命令，点击确定，进入服务管理页面。



5、找到Windows Time服务，双击打开该服务。



6、在常规页面，把启动类型修改为自动，点击启动，然后点击确定。



7、重启Windows Time服务。以管理员身份运行cmd，输入net stop w32time && net start w32time重启该服务。



8、设置防火墙运行123端口访问。输入netsh firewall add portopening protocol = UDP port =123 name = NTPSERVER命令设置。



9、设置完成。


 