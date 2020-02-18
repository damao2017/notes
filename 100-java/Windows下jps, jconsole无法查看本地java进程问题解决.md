### Windows下jps, jconsole无法查看本地java进程问题解决

先通过本地java代码运行：

System.out.println(System.getProperties()); 
查看属性java.io.tmpdir=C:/Users/%USER%/AppData/Local/Temp/，注此处%USER%为变量代表操作系统用户名


进入该目录，看到有个hsperfdata_%USER%目录，进入该目录，发现该目录下没有任何文件。


经验证当前用户并无权限在此文件夹下创建文件。


空白处右击 属性->安全


发现组或用户名中没有我当前使用的用户，点击 编辑->添加 当前使用的用户，并设置权限为完全控制


至此当前用户具有该目录的完全控制权限。并通过检验当前用户可在当前目录创建文件。