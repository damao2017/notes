# windows设置系统同步时间频率

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\W32Time\TimeProviders\NtpClient]
"SpecialPollInterval"

默认是604800，秒，7天

改成900，15分钟

3600，1小时

重启windows time 服务