# 建立maven私服仓库Nexus

官网地址https://www.sonatype.com/download-oss-sonatype 
我下载的版本是nexus-2.14.4-03-bundle.tar.gz 
2.X需要jdk1.7以上,3.X需要jdk1.8支持，先装jdk

解压到一个文件夹Nexus，目录如下 

--nexus-2.14.4
--sonatype-work

使用nexus查看支持的命令
使用nexus install 安装wondows 服务
使用nexus start 启动服务

启动成功后，打开localhost:8081/nexus,进入如下页面   点击右上角login登陆，默认账号密码 admin/admin123，后面可根据需求自行修改。

---

从中央库下载索引到私服中央库 
依次点击1.左边的Repositories 2.点击Central 3.点击下面的Configuration 4.找到Download Remote Indexes 5.选择true 6.右键Central选择repair index或者update index都行 
nexus的仓库类型分为以下四种：
- group: 仓库组
- hosted：宿主
- proxy：代理
- virtual：虚拟   

Public Repositories: 仓库组

- 3rd party: 无法从公共仓库获得的第三方发布版本的构件仓库
- Apache Snapshots: 用了代理ApacheMaven仓库快照版本的构件仓库
- Central: 用来代理maven中央仓库中发布版本构件的仓库
- Central M1 shadow: 用于提供中央仓库中M1格式的发布版本的构件镜像仓库
- Codehaus Snapshots: 用来代理CodehausMaven 仓库的快照版本构件的仓库
- Releases: 用来部署管理内部的发布版本构件的宿主类型仓库
- Snapshots:用来部署管理内部的快照版本构件的宿主类型仓库

至此私服就开始向中央库下载更新索引了（前提你要有网络,否则会失败） 

点击左边的Administration下的Scheduled Tasks可以查看当前任务进度。如果状态为running则标示任务已经开始执行。 

更新索引会花费比较多的时间大概半小时到1小时左右。 
如果想看进度可以打开nexus-2.14.4-03文件夹->logs->wapper.log查看日志。 

如果日志中出现 
`jvm 1 | 2017-07-21 15:48:19,615+0800 INFO [pxpool-1-thread-1] admin org.sonatype.nexus.index.NexusScanningListener - Scanning of repositoryID="central" started.` 
标示更新索引已经开始。如果之后再没有信息表示正在进行。如果更新失败log里也会显示。如果显示finished则表示更新索引成功。 
`jvm 1 | 2017-07-21 15:53:18,583+0800 INFO [pxpool-1-thread-1] admin org.sonatype.nexus.index.NexusScanningListener - Scanning of repositoryID="central" finished` 
到这里，maven的私服算是搭建完成了。

 nexus的仓库类型分为以下四种：

​        group: 仓库组

​        hosted：宿主

​       proxy：代理

​       virtual：虚拟