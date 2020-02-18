# ubuntu 查看端口

```bash
$netstat -tunlp

(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:20022           0.0.0.0:*               LISTEN      -               
tcp6       0      0 :::23306                :::*                    LISTEN      -               
tcp6       0      0 :::20022                :::*                    LISTEN      -               

#普通用户显示信息不全，使用超级管理员运行
>netstat -tunlp 
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:20022           0.0.0.0:*               LISTEN      660/sshd        
tcp6       0      0 :::23306                :::*                    LISTEN      1317/mysqld     
tcp6       0      0 :::20022                :::*                    LISTEN      660/sshd        
```

