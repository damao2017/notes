# ubuntu crontab

##  编辑器选择 

crontab 编辑器 默认选择了nano，可以通过select-editor重新选择。

```bash
ntkd@ubuntu:~$ select-editor

Select an editor.  To change later, run 'select-editor'.
  1. /bin/ed
  2. /bin/nano        <---- easiest
  3. /usr/bin/vim.basic
  4. /usr/bin/vim.tiny

Choose 1-4 [2]: 3
```

## 格式

```
[分钟] [小时] [每月的某一天] [每年的某一月] [每周的某一天] [执行的命令]

在以上各个字段中，还可以使用以下特殊字符：
星号（*）：代表所有可能的值，例如month字段如果是星号，则表示在满足其它字段的制约条件后每月都执行该命令操作。
逗号（,）：可以用逗号隔开的值指定一个列表范围，例如，“1,2,5,7,8,9”
中杠（-）：可以用整数之间的中杠表示一个整数范围，例如“2-6”表示“2,3,4,5,6”
正斜线（/）：可以用正斜线指定时间的间隔频率，例如“0-23/2”表示每两小时执行一次。同时正斜线可以和星号一起使用，
例如*/10，如果用在minute字段，表示每十分钟执行一次。


* * * * *                  # 每隔一分钟执行一次任务  
0 * * * *                  # 每小时的0点执行一次任务，比如6:00，10:00  
6,10 * 2 * *            # 每个月2号，每小时的6分和10分执行一次任务  
*/3,*/5 * * * *          # 每隔3分钟或5分钟执行一次任务，比如10:03，10:05，10:06  

```

## 常用命令

```bash
#编辑
crontab -e
#查看
crontab -l

#服务的开启，每次修改完配置文件，都要执行重启服务
service cron restart
```

## ubuntu 默认crontab日志没有打开

```bash
打开日志方法
修改rsyslog
sudo vim /etc/rsyslog.d/50-default.conf
找到cron
cron.*              /var/log/cron.log     
重启rsyslog
sudo  service rsyslog  restart
```

**注意**

1.新创建的cron job，不会马上执行，至少要过2分钟才执行。如果重启cron则马上执行。

2.在crontab中%是有特殊含义的，表示换行的意思。

如果要用的话必须进行转义\%，

date ‘+%Y%m%d’   -->>  date ‘+\%Y\%m\%d’。

## 常见例子

```
实例1：每1分钟执行一次command
命令：* * * * * command
 
实例2：每小时的第3和第15分钟执行
命令：3,15 * * * * command
 
实例3：在上午8点到11点的第3和第15分钟执行
命令：3,15 8-11 * * * command
 
实例4：每隔两天的上午8点到11点的第3和第15分钟执行
命令：3,15 8-11 */2 * * command
 
实例5：每个星期一的上午8点到11点的第3和第15分钟执行
命令：3,15 8-11 * * 1 command
 
实例6：每晚的21:30重启smb
命令：30 21 * * * /etc/init.d/smb restart
 
实例7：每月1、10、22日的4 : 45重启smb
命令：45 4 1,10,22 * * /etc/init.d/smb restart
 
实例8：每周六、周日的1 : 10重启smb
命令：10 1 * * 6,0 /etc/init.d/smb restart
 
实例9：每天18 : 00至23 : 00之间每隔30分钟重启smb
命令：0,30 18-23 * * * /etc/init.d/smb restart
 
实例10：每星期六的晚上11 : 00 pm重启smb
命令：0 23 * * 6 /etc/init.d/smb restart
 
实例11：每一小时重启smb
命令：* */1 * * * /etc/init.d/smb restart
 
实例12：晚上11点到早上7点之间，每隔一小时重启smb
命令：* 23-7/1 * * * /etc/init.d/smb restart
 
实例13：每月的4号与每周一到周三的11点重启smb
命令：0 11 4 * mon-wed /etc/init.d/smb restart
 
实例14：一月一号的4点重启smb
命令：0 4 1 jan * /etc/init.d/smb restart
 
实例15：每小时执行/etc/cron.hourly目录内的脚本
命令：01 * * * * root run-parts /etc/cron.hourly
说明：run-parts这个参数了，如果去掉这个参数的话，后面就可以写要运行的某个脚本名，而不是目录名了
 ﻿​
```

