### redis 设置 和使用

#### ubuntu gcc环境安装

```bash
apt-get install build-essential

$ apt-cache depends build-essential
build-essential
 |Depends: libc6-dev
 Depends: <libc-dev>
 libc6-dev
 Depends: gcc
 Depends: g++
 Depends: make
 make:i386
 Depends: dpkg-dev
 Conflicts: build-essential:i386﻿​
```

#### redis 安装

解压 拷贝到`/opt/`
进入redis-5.0.7 运行`make`
报错jemalloc/jemalloc.h: No such file or directory

使用`make MALLOC=libc`
提示Hint: It's a good idea to run 'make test' ;)

运行`make test`
提示You need tcl 8.5 or newer in order to run the Redis test

不用管 其实已经可以启动
进入src启动
`./redis-server`

#### 修改配置

修改redis.conf

```ini
#ip访问
#bind 127.0.0.1
#端口
port 16379
#客户端自动断开时间（秒）
timeout 180
#后台启动
daemonize yes
#日志级别
loglevel notice
#日志路径
logfile "/opt/redis-5.0.7/log/redis.log"
#指定在多长时间内，有多少次更新操作，就将数据同步到数据文件，可以多个条件配合
save 900 1
save 300 10
save 60 10000
#数据库密码
requirepass wersdf234
#数据库使用最大内存
maxmemory 256M
#修改默认策略
maxmemory-policy volatile-lru﻿​
```

加载配置文件启动
`./bin/redis-server ./redis.conf`

查看进程
`ps -aux | grep redis`

查看日志(前面已经配置了日志路径)
`cat ./log/redis.log`

访问
`redis-cli -h host -p port -a password`

强制关闭
`ps -aux | grep redis` 第二列是pid
`kill -9 pid`

正常关闭
`./redis-cli -p -a password shutdown`
或者已经登陆到了redis-cli之后,执行`shutdown`

#### 设置成服务
复制`/redis/utils/redis_init_script`到`/etc/init.d/`
修改/etc/init.d/redis内容

```ini
#正确的端口
REDISPORT=6379
#redis-server目录
EXEC=/usr/local/bin/redis-server
#redis-cli目录
CLIEXEC=/usr/local/bin/redis-cli
#PIDFILE目录，同redis.conf配置文件
PIDFILE=/var/run/redis_${REDISPORT}.pid
#redis.conf配置文件地址
CONF="redis/${REDISPORT}.conf"﻿​
```
因为设置了密码下面的关闭命令 

```bash
echo "Stopping ..."
$CLIEXEC -p $REDISPORT shutdown
#改成
echo "Stopping ..."
$CLIEXEC -p $REDISPORT -a 设置的密码 shutdown
```

#### redis常用操作

查询所有值
```
12keys *key ?   
```

?代表一个字符
创建值
```
12set keyName valueset username aaa  
```

查看键名类型
```
1type username  
```

查看值
```
1get username  
```

删除值
```
1del username  
```

修改键名
```
1rename username username111  
```

查看是否存在
```
1exists username  
```

查看生存时间
```
1ttl username  
```

设定过期时间(秒)场景：验证码，访问频率
```
1expire username 10  
```

移除过期时间
```
1persist username  
```

切换数据库
```
1select 0/1/2/3  
```

#### redis数据类型(string，hash)


string 通常用于保存单个字符串或者json
存
set keyname value
当键名不存在时存，存在就不存
setnx keyname value 解决分布式锁方案之一
取
get keyname
截取value
getrange keyname start(0) end()
先得到旧值再更新
getset keyname value
获取长度
strlen keyname
自增1
incr keyname
自增任意
incrby keyname 增量
自减1
decr keyname
自减任意
decyby keyname 减量
字符串拼接
append keyname value

hash
key vaule 的map容器
单个增加
hset hname field value
多个增加
hmset hname field1 value1 field2 value2 
取单个
hget hname 
取多个
hmget hname field1 field2
取所有字段和值
hgetall hname
取字段
hkeys hname
取字段数量
hlen hname
删除
hdel hname field1 field2

 