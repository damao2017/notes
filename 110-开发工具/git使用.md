# git使用

- 初始化

```
设置用户名和邮箱
$ git config --global user.name "Your Name"
$ git config --global user.email “email@example.com
```

- 新建git仓库

```bash
cd dir
git init
```

- 查看状态

```bash
git status
```

- 添加所有改动

```bash
   git add .
```

- git 提交

```bash
git commit -m 'feat(supermall):初始化项目'
```

- 本地仓库和远程仓库关联

```bash
git remote add origin https://aaa.com/aaa.git
git push -u origin master
```

报错：`error: failed to push some refs to `
如果远程仓库本来有东西，先执行

```bash
git pull --rebase origin master
```

再执行

```bash
git push -u origin master
```

- 克隆远程仓库

```bash
git clone https://github.com/damao2017/supermall.git
```

- 创建分支login

```bash
git checkout -b login
```

- 查看分支

   ```bash
   git branch
   ```

- 合并分支

```bash
#先切换到主分支
git checkout master
#合并login分支
git merge login
```

- 推送到远程仓库

```bash
git push 
```

- 将本地的分支推送到远程

```bash
git push -u origin login
```



```
Eclipse里面设置
Preferences-Team-Git-Cloning repositories
设置默认的下载路径
```

- git ignore 模板文件

```
## .gitignore for Grails 1.2 and 1.3

# .gitignore for maven 
target/
*.releaseBackup

# web application files
#/web-app/WEB-INF

# IDE support files
/.classpath
/.launch
/.project
/.settings
/*.launch
/*.tmproj
/ivy*
/eclipse

# default HSQL database files for production mode
/prodDb.*

# general HSQL database files
*Db.properties
*Db.script

# logs
/stacktrace.log
/test/reports
/logs
*.log
*.log.*

# project release file
/*.war

# plugin release file
/*.zip
/*.zip.sha1
 
# older plugin install locations
/plugins
/web-app/plugins
/web-app/WEB-INF/classes
 
# "temporary" build files
target/
out/
build/
 
# other
*.iws
 
#.gitignore for java
*.class
 
# Package Files #
*.jar
*.war
*.ear
 
## .gitignore for eclipse
 
*.pydevproject
.project
.metadata
bin/**
tmp/**
tmp/**/*
*.tmp
*.bak
*.swp
*~.nib
local.properties
.classpath
.settings/
.loadpath
 
# External tool builders
.externalToolBuilders/
 
# Locally stored "Eclipse launch configurations"
*.launch
 
# CDT-specific
.cproject
 
# PDT-specific
.buildpath
 
## .gitignore for intellij
 
*.iml
*.ipr
*.iws
.idea/
 
## .gitignore for linux
.*
!.gitignore
!.gitattributes
!.editorconfig
!.eslintrc
!.travis.yml
*~
 
## .gitignore for windows
 
# Windows image file caches
Thumbs.db
ehthumbs.db
 
# Folder config file
Desktop.ini
 
# Recycle Bin used on file shares
$RECYCLE.BIN/
 
## .gitignore for mac os x
 
.DS_Store
.AppleDouble
.LSOverride
Icon
 
 
# Thumbnails
._*
 
# Files that might appear on external disk
.Spotlight-V100
.Trashes

## hack for graddle wrapper
!wrapper/*.jar
!**/wrapper/*.jar
```

