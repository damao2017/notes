# docker常用操作

docker info

docker search nginx

docker pull tomcat

docker images

docker run --name testmysql -p 13306:3306 -v /home/ntdm/mysql/data:/var/lib/mysql -v /home/ntdm/mysql/mysql.cnf:/etc/mysql/mysql.conf.d/mysqld.cnf -e MYSQL_ROOT_PASSWORD=wersdf234 mysql:latest --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci

docker rm -f name

docker rmi -f name

 

docker ps

docker ps -a 查看所有容器的状态

启动容器
docker start name

docker inspect 78f8f421b569 查看docker信息

docker exec -it 0cbeed7f7794 /bin/bash


docker start/stop/restart 0cbeed7f7794

普通用户不用sudo执行docker命令
将用户加入到docker组
sudo adduser ntdm docker
newgrp - docker 手工切换到这个组

实时查看docker容器日志
$ sudo docker logs -f -t --tail 行数 容器名

例：实时查看docker容器名为s12的最后10行日志
$ sudo docker logs -f -t --tail 10 s12

复制主机的localtime
docker cp /etc/localtime 【容器ID或者NAME】:/etc/localtime