# ubuntu 权限修改chmod

### ubuntu 权限修改chmod

```
drwxr-xr-x. 2 root root 4096 May  7 19:00 innerTest
```

第一位是文件还是目录 
后面三个一组 
第一组三个是所有者权限 
第二组三个是同组权限 
第三组三个是其他人

r就是read的缩写。 
w就是write的缩写。 
x是execute的缩写。

`r = 4， w = 2， x = 1` 
`chmod` 改变权限

```
#修改为所有者rw，同组r，其他人什么权限也没有
chmod 640 file
```

