# Windows关闭445端口

1、运行regedit打开注册表
2、注册表选项”HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\NetBT\Parameters“
3、在右边空白处右击新建“QWORD（64位）值”，然后重命名为“SMBDeviceEnabled”，再把这个子键的值改为0。XP直接新建DWORD值。
4、重启 