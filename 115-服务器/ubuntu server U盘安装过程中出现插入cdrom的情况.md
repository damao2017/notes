# ubuntu server U盘安装过程中出现插入cdrom的情况

拷贝安装的iso文件到u盘根目录

进入后先查看u盘的挂在目录，然后重新挂在u盘，再挂在u盘里面的iso文件到cdrom

```
df -h
umount /dev/sdd4 　　　　#直接挂载可能会有问题
mount /dev/sdd4 /mnt
mount -o loop /mnt/ubuntu-16.04.4-server-amd64.iso /cdrom
```

