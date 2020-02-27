# ubuntu 权限

<user list> <host list> = <operator list> <tag list> <command list>

user list 用户/组，或者已经设置的用户的别名列表, 用户名直接 username，用户组加上%，比如%admin,
host list 主机名或别名列表
operator list runas用户，即可以以哪个用户、组的权限来执行
command list 可以执行的命令或列表
tag list 这个经常用到的是 NOPASSWD: ，添加这个参数之后可以不用输入密码。

再来看看默认配置里面的对应配置的说明

\## root 用户可以在任意主机上以任意用户、组的权限执行任意命令，说明root用户具有最高级的权限。
root ALL=(ALL:ALL) ALL

\## admin组的用户都拥有最高级权限。
%admin ALL=(ALL) ALL

\## sudo 组用户和root用户权限一样
%sudo ALL=(ALL:ALL) ALL

使用route命令可以不输入密码
ntkd ALL=(root) NOPASSWD:/sbin/route

PROCESS=`ps -ef|grep $1|grep -v grep|grep -v PPID|awk '{ print $2}'`