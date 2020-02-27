# eclipse查找替换注释

```
1.Eclipse ctrl+f 打开查找框 
2.选中 Regular expressions (正则表达式)

去掉/* */（eclipse）     /\*(.|[\r\n])*?\*/
去掉//（eclipse）         //.*$
去掉import（eclipse）     import.*$
去掉空行（eclipse）        ^\s*\n
去掉空行（ue）    %[ ^t]++^p
```

