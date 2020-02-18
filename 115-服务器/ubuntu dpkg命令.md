# ubuntu dpkg命令

安装

```
dpkg -i name.deb
```

查看已安装列表

```
dpkg -l
dpkg -l name
```

cd 到安装包的目录
安装：
dpkg -i 安装包名字


dpkg的详细用法：
dpkg -i 安装一个Debian包裹文件，如你手动下载的文件。
dpkg -c 列出的内容。
dpkg -I 从中提取包裹信息。
dpkg -r 移除一个已安装的包裹。
dpkg -L 列出 安装的所有文件清单。同时请看 dpkg -c 来检查一个 .deb 文件的内容。

dpkg -P
完全清除一个已安装的包裹。和 remove 不同的是，remove 只是删掉数据和可执行文件，purge 另外还删除所有的配制文件。
dpkg -s
显示已安装包裹的信息。同时请看 apt-cache 显示 Debian 存档中的包裹信息，以及 dpkg -I 来显示从一个 .deb 文件中提取的包裹信息。