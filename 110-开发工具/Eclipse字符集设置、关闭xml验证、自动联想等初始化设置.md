# Eclipse字符集设置、关闭xml验证、自动联想等初始化设置

## 字符集设置

change file
window -> Preferences -> General -> Content Types

change workspace
window -> Preferences -> General -> Workspace -> Text file encoding

change web
Window->Preferences->Web->JSP Files->ISO 10646/Unicode(UTF-8)

 

##  关闭 xml验证

 window -> Preferences -> Validation

Xml Schema Validator、Xml Validator

后面的build的勾去掉

 

## Java代码联想

Window --> Perferences --> Java --> Editor --> Content Assist

`Auto activation triggers for Java:` 改成 `.@abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ`

Window --> Perferences --> Java --> Editor --> Content Assist --> Advanced

勾选 Java Proposals



## XML代码联想

 Window --> Perferences -->Xml --> Xml Files --> Editor --> Content Assist

`<=:` 改为 `<=: abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ`


a前面有个空格