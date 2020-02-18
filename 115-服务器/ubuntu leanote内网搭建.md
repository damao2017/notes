# ubuntu leanote内网搭建

## 1.下载mongodb

下载地址 https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-3.0.1.tgz

解压

```
tar -xzvf mongodb-linux-x86_64-2.6.4.tgz
```

配置环境变量

```
sudo vim /etc/profile

在末尾增加：
export PATH=$PATH:/home/ntkd/soft/mongodb-linux-x86_64-3.0.1/bin

source /etc/profile   --立刻使环境变量生效
```

在mongodb目录下建立data和log文件夹

```
mkdir log
touch logmongodb.log
mkdir data
```

在mongodb目录下新建一个文件，文件名任意，在这里我取名为：mongodb.conf

```ini
port=19001  --指定启动的端口
dbpath=data/  --指定数据存放的路径
logpath=log/mongodb.log  --指定日志文件路径
logappend=true          --日志累加
fork=true --后台默认启动
auth=true --开启认证，在配置好用户名密码后打开，第一次启动先关闭
```

启动：

```
mongod -f mongodb.conf
```

启动会有提示，需要关闭两个设置

打开/etc/rc.local文件输入，开机启动时候就关闭

```
echo never >> /sys/kernel/mm/transparent_hugepage/enabled
echo never >> /sys/kernel/mm/transparent_hugepage/defrag
```

导入leanote数据

```
mongorestore -h localhost -d leanote --dir /home/ntkd/soft/leanote/mongodb_backup/leanote_install_data
```

增加验证，创建一个超级用户

启动mongodb

```
mongo
```

创建admin库，增加root角色的用户

```
use admin

db.createUser(
 {
     user: "ntkd",
     pwd: "kdzx123",
     roles: [ { role: "root", db: "admin" } ]
 }
)

db.system.users.find()
```

重启mongodb，找到进程id后kill

```
ps -aux | grep mongo
kill id
mongod -f mongodb.conf
```

改了端口，登录需要指定端口，使用数据库需要验证，登录

```
mongo --port 19001
use admin
db.auth('账号','密码')
```

首先切换到leanote数据库下

```
use leanote
```

 添加一个用户root, 密码是abc123

```
db.createUser({
     user: 'leanote',
     pwd: 'abc123',
     roles: [{role: 'dbOwner', db: 'leanote'}]
});
```

验证账号是否正确

```
db.auth("root", "abc123");﻿​
```

## 2.配置leanote

修改leanote/conf/app.conf

修改

```
site.url=http://192.168.253.132:9000  #为你自己的服务器地址

# mongdb 修改mongodb连接参数
db.host=127.0.0.1
db.port=19001
db.dbname=leanote # required
db.username=leanote # if not exists, please leave it blank
db.password=kdzx123 # if not exists, please leave it blank

# You Must Change It !! About Security!!
app.secret=V8 #
```

然后就可以启动leanote

```
 cd leanote/bin
 bash run.sh
```

## **安装whk**

```
/usr/local/bin/wkhtmltopdf
apt-get -f install
```

打开windows c:\Windows\fonts\simsun.ttc拷贝到linux服务器/usr/share/fonts/目录下,再次生成pdf中文显示正常

fc-list 查看字体

---

内建的角色 
数据库用户角色：read、readWrite; 
数据库管理角色：dbAdmin、dbOwner、userAdmin； 
集群管理角色：clusterAdmin、clusterManager、clusterMonitor、hostManager； 
备份恢复角色：backup、restore； 
所有数据库角色：readAnyDatabase、readWriteAnyDatabase、userAdminAnyDatabase、dbAdminAnyDatabase 
超级用户角色：root // 这里还有几个角色间接或直接提供了系统超级用户的访问（dbOwner 、userAdmin、userAdminAnyDatabase） 
内部角色：__system 
角色说明： 
Read：允许用户读取指定数据库 
readWrite：允许用户读写指定数据库 
dbAdmin：允许用户在指定数据库中执行管理函数，如索引创建、删除，查看统计或访问system.profile 
userAdmin：允许用户向system.users集合写入，可以找指定数据库里创建、删除和管理用户 
clusterAdmin：只在admin数据库中可用，赋予用户所有分片和复制集相关函数的管理权限。 
readAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的读权限 
readWriteAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的读写权限 
userAdminAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的userAdmin权限 
dbAdminAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的dbAdmin权限。 
root：只在admin数据库中可用。超级账号，超级权限