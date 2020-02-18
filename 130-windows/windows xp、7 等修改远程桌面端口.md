# windows xp、7 等修改远程桌面端口

第一步，修改注册表项 
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\Wds\rdpwd\Tds\tcp 
将PortNumber修改为十进制的数字，就是你希望的端口号，例如：88888

第二步，修改注册表项 
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp 
将PortNumber修改为十进制的数字，就是你希望的端口号，例如：88888

第三步，添加防火墙规则

windows 7 防火墙高级设置下，新建入站规则，添加TCP类型的端口（你希望的端口号）例如：88888

win xp 在防火墙，例外tab里面添加端口。

第四步，重启远程桌面服务

重启机器