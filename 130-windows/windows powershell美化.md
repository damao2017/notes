# windows powershell美化

安装依赖posh-git
Install-Module posh-git -Scope CurrentUser

它会提示你先安装 NuGet，直接选择“是”。

Import-Module posh-git

可能会报错

因为在此系统上禁止运行脚本。有关详细信息，请参阅 https:/go.microsoft.com/fwlink/
?LinkID=135170 中的 about_Execution_Policies。

更改权限
Set-ExecutionPolicy -Scope CurrentUser Bypass


不受信任的存储库
你正在从不受信任的存储库安装模块。如果你信任该存储库，请通过运行 Set-PSRepository
cmdlet 更改其 InstallationPolicy 值。是否确实要从“PSGallery”安装模块?
[Y] 是(Y)  [A] 全是(A)  [N] 否(N)  [L] 全否(L)  [S] 暂停(S)  [?] 帮助

如果你觉得这个警告烦人，直接执行以下代码信任该仓库即可：
Set-PSRepository -Name PSGallery -InstallationPolicy Trusted


对不同文件进行高亮
Install-Module DirColors -Scope CurrentUser
Import-Module DirColors



安装oh-my-posh
Install-Module oh-my-posh -Scope CurrentUser
Import-Module oh-my-posh
Set-Theme Agnoster # 主题可以按Tab键切换

命令行高亮的模块
Install-Module PSReadLine -Scope CurrentUser
Import-Module PSReadLine


安装字体
git clone https://github.com/powerline/fonts.git
cd .\fonts\
.\install.ps1

安装好后执行以下代码删除多余文件：
cd ..
Remove-Item -Force .\fonts\
打开你的 PowerShell 界面，在标题栏右键选中“默认值”，在“字体”选项中使用著名的“逮虾户”字体 (DejaVu Sans Mono for Powerline)。


把配置写进$PROFILE
在 PowerShell 里键入以下命令：
$PROFILE
看到一个路径

写入
#chcp 65001  就是换成UTF-8代码页
chcp 65001
#语法高亮
Import-Module PSReadLine
#PowerShell支持 Git
Import-Module posh-git
#终端样式
Import-Module oh-my-posh
Set-Theme PowerLine
#路径和文档颜色
Import-Module DirColors
Update-DirColors ~/Documents/WindowsPowerShell/Modules/DirColors/1.1.2/dircolors-solarized/dircolors.256dark

保存