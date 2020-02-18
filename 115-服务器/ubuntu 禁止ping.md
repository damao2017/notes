# ubuntu 禁止ping

##临时效果，重启失效
##令ubuntu不响应ping 
echo 1 > /proc/sys/net/ipv4/icmp_echo_ignore_all 
##令Ubuntu响应ping 
echo 0 > /proc/sys/net/ipv4/icmp_echo_ignore_all 


永久有效，写入配置文件/etc/rc.local里
一定要将命令添加在exit 0之前。
里面可以直接写命令或者执行Shell脚本文件sh。