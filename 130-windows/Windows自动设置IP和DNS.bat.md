# Windows自动设置IP和DNS.bat

```bash
@echo off
echo **************************************************************************
echo * 修改IP地址、DNS *
echo * Windows 7 Copyright (C) 2016-01-12 *
echo **************************************************************************
echo 正在修改IP地址和DNS服务器地址,请耐心等待…………
echo 正在更改本机IP地址...
netsh interface ipv4 set address name="本地连接" source=static addr=132.233.33.158 mask=255.255.255.240 gateway=132.233.33.145 >nul
echo 正在添加本机首选DNS服务器...
netsh interface ipv4 set dns name="本地连接" source=static addr=132.228.176.246 register=PRIMARY
echo 正在添加备用DNS服务器...
netsh interface ipv4 add dns name="本地连接" addr=132.228.127.251
netsh interface ipv4 add dns name="本地连接" addr=132.233.16.220
netsh interface ipv4 add dns name="本地连接" addr=132.233.144.10
netsh interface ipv4 add dns name="本地连接" addr=132.233.141.10
echo 检查当前本机配置...
ipconfig /all
pause
```

