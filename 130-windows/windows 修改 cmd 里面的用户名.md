# windows 修改 cmd 里面的用户名

1.打开运行输入regedit 回车

2.定位到HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList

3.选中下面名字最长的项，双击右侧ProfileImagePath，修改C:\Users\XXX>后的用户名

4.重启系统，然后会进入临时用户，修改C:\Users目录下的用户名目录和刚刚改的一样

5.重启即可完成