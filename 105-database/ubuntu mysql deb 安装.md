## ubuntu mysql deb 安装

```bash
#libaio 如果系统中尚未存在库，则 可能需要安装该库：
sudo apt-get install libaio1
#使用以下命令预配置MySQL服务器软件包：
sudo dpkg-preconfigure mysql-community-server_*.deb
#提示：将被要求为root用户提供您的MySQL安装密码。
#对于MySQL服务器的基本安装，请安装数据库公用文件包，客户端包，客户端元包，服务器包和服务器元包（按此顺序）; 可以使用单个命令来执行此操作：
#注意：下面这条命令不能直接运行，应该拆开来按中括号里面以逗号分开的顺序进行安装，比如：
sudo dpkg -i mysql-common_*.deb
sudo dpkg -i mysql-community-client_*.deb
sudo dpkg -i mysql-client_*.deb
sudo dpkg -i mysql-community-server_*.deb
sudo dpkg -i mysql-server_*.deb
#如果中途被dpkg警告未满足的依赖关系 ，可以使用apt-get来修复它们，然后再运行中断的命令 ：
sudo apt-get -f install
```

