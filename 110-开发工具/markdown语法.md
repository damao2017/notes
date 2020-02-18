# markdown语法

## 1.1  内容目录[TOC] 

```markdown
[TOC] 
```
[TOC] 

## 2.0 代码

```markdown
代码块
​```markdown
markdown
​```

`单行代码`
```

```markdown
markdown
```

`单行代码`

## 2.1 字体

```markdown
**加粗**
*斜体*
***斜体加粗***
~~删除~~
```

- **加粗**
- *斜体*
- ***斜体加粗***
- ~~删除~~

## 2.2 引用

```markdown
>
>>
>>>
```

> 这里是一个>引用

> > 这里是两个>>引用

> > > 这里是三个>>>引用

## 2.3 分割线

```markdown
***
---
```

***

---



## 2.4 图片

```markdown
![image-20191128100805983](.\image-20191128100805983.png "截图测试")
![alt显示在图片下面的文字，相当于对图片内容的解释](图片地址 "图片的标题，当鼠标移到图片上时显示的内容。title可加可不加")

可以截图后直接复制粘贴进typora
```



![测试图片](.\image-20191128100805983.png "截图测试")

## 2.5 超链接

```markdown
<a href="http://baidu.com" target="_blank">baidu</a>
[百度](http://baidu.com)
按ctrl鼠标点这个链接就会打开网页
邮箱：<example@w3cschool.cn>
网址：<www.baidu.com>
---
[1]:https://github.com "GitHub"
[2]:https://www.zhihu.com "知乎"
我经常去的几个网站[GitHub][1]、[知乎][2]。
```

<a href="http://baidu.com" target="_blank">baidu</a>

[百度](http://baidu.com)

邮箱：<example@w3cschool.cn>

网址：<www.baidu.com>

---

我经常去的几个网站[github ddddd][1]、[ssss 知乎][2]。

[1]:https://github.com "GitHub"
[2]:https://www.zhihu.com "知乎"

## 2.6 列表

### 2.6.1 无序列表

```markdow
- 列表1
+ 列表2
* 列表3

注意：- + * 跟内容之间都要有一个空格
```

- 列表1
- 列表2

---

### 2.6.2 有序列表

```markdown
1. 列表11
2. 列表22
3. 列表33

注意：序号跟内容之间要有空格
```

1. 列表11
2. 列表22
3. 列表33

---

### 2.6.3 上下级列表

```markdown
- 上级列表
[tab]- 下级列表
  		[tab]- 下级列表
    		[tab]- 下级列表
```

- 上级列表
  - 下级列表
    - 下级列表
      - 下级列表

---

1. 一级列表
   1. 2.1
   2. 2.2
      1. 3.1
      2. 3.2

## <div id="jump2">2.7 表格</div>

```markdown
|表头|表头|表头|
|:--|:--:|--:|
|内容|内容|内容|
|内容|内容|内容|

第二行分割表头和内容。
- 有一个就行，为了对齐，多加了几个
文字默认居左
-两边加：表示文字居中
-右边加：表示文字居右
注：原生的语法两边都要用 | 包起来。此处也可以省略
表头|表头|表头
---|:--:|--:
内容|内容|内容
内容|内容|内容
```

| 表头 | 表头 | 表头 |
| :-- | :--: | --: |
| 内容 | 内容 | 内容 |
| 内容 | 内容 | 内容 |

## <span id="2.8-锚点">2.8 锚点</span>

```markdown
typora方式，跳转到标题处
跳转到：[2.9 注脚](#2.9-注脚)
跳转到：[2.6.1 无序列表](#2.6.1-无序列表)

html方法1：
设置锚点 <a href='#jump'>第一个题目</a>
带有锚点的题目 其中href值为你要跳跃的锚点的 #+name值(#指代这是一个锚点)
设置跳转 <a name='jump'>跳转的锚点</a>
要跳到的锚点处，保持name值与题目href值一直即可(也可以加##修改成大标题哦)

html方法2：跳转到任意位置
设置锚点 
## <div id="jump2">2.7 表格</div>
## <a id="jump3">2.8 锚点</a> 这种方式锚点会变成查链接,会变色
## <span id="2.8-锚点">2.8 锚点</span>
锚点在`<div id="jump2">表格</div>`上
跳转到: [2.7 表格](#jump2)
跳转到: [2.8 锚点](#2.8-锚点) 空格使用-来代替

```


typora方式，跳转到标题处

跳转到：[2.9 注脚](#2.9-注脚)

跳转到：[2.6.1 无序列表](#2.6.1-无序列表)

---

设定锚点：<a name='jump1'>锚点1</a>

跳转到：<a href='#jump1'>锚点1</a>

---

锚点在`<div id="jump2">表格</div>`上

跳转到: [2.7 表格](#jump2)

跳转到: [2.8 锚点](#2.8-锚点)




## 2.9 注脚

```markdown
使用 Markdown[^1]可以效率的书写文档, 直接转换成 HTML[^2], 你可以使用简书或者支持Markdown的编辑器进行书写。

[^1]:Markdown是一种纯文本标记语言

[^2]:HyperText Markup Language 超文本标记语言
```

使用 Markdown[^1]可以效率的书写文档, 直接转换成 HTML[^2], 你可以使用简书或者支持Markdown的编辑器[^3]进行书写。

[^1]:Markdown是一种纯文本标记语言

[^2]:HyperText Markup Language 超文本标记语言
[^3]:测试注脚



## 3.0 数学公式

```markdown
$$
\sum_{i=1}^n a_i=0
$$

$$
f(x_1,x_x,\ldots,x_n) = x_1^2 + x_2^2 + \cdots + x_n^2
$$

$$
\sum^{j-1}_{k=0}{\widehat{\gamma}_{kj} z_k}
$$
```

$$
\sum_{i=1}^n a_i=0
$$

$$
f(x_1,x_x,\ldots,x_n) = x_1^2 + x_2^2 + \cdots + x_n^2
$$

$$
\sum^{j-1}_{k=0}{\widehat{\gamma}_{kj} z_k}
$$

## 3.1 HTML代码

### 3.1.1 div

```html
<div class="footer">
   © 2004 Foo Corporation
</div>
```

<div class="footer">
   © 2004 Foo Corporation
</div>

### 3.1.2 table

```html
<table>
    <tr>
        <th rowspan="2">值班人员</th>
        <th>星期一</th>
        <th>星期二</th>
        <th>星期三</th>
    </tr>
    <tr>
        <td>李强</td>
        <td>张明</td>
        <td>王平</td>
    </tr>
</table>
```

<table>
    <tr>
        <th rowspan="2">值班人员</th>
        <th>星期一</th>
        <th>星期二</th>
        <th>星期三</th>
    </tr>
    <tr>
        <td>李强</td>
        <td>张明</td>
        <td>王平</td>
    </tr>
</table>

## 3.2 todo list

```markdown
近期任务安排:
- [x] 整理Markdown手册
- [ ] 改善项目
   - [x] 优化首页显示方式
   - [x] 修复闪退问题
   - [ ] 修复视频卡顿
- [ ] A3项目修复
   - [x] 修复数值错误
```

近期任务安排:
- [x] 整理Markdown手册
- [ ] 改善项目
   - [x] 优化首页显示方式
   - [x] 修复闪退问题
   - [ ] 修复视频卡顿
- [ ] A3项目修复
   - [x] 修复数值错误

## 3.3 流程图

```flow
st=>start: 开始
io=>inputoutput: 验证
op=>operation: 选项
cond=>condition: 是 或 否？
sub=>subroutine: 子程序
e=>end: 结束

st->io->op->cond
cond(yes)->e
cond(no)->sub->io
```

## 3.4 时序图

```sequence
participant 客户端 as A
participant 服务端 as B
participant 通行证中心 as C
Note over A:用户输入通行证账号、密码
A->C: 发送账号、密码
Note over C:验证账号、密码
C-->>A:返回token
A->B:发送token
B->C:验证token
C-->>B:验证成功
B-->>A:登陆成功
Note left of A:左边注释
B->B:自交互
Note right of C:右边注释
```

## 3.5 转义字符\\

```markdown
前面添加\即可
\\ 反斜杠
\` 反引号
\* 星号
\_ 下划线
\{\} 大括号
\[\] 中括号
\(\) 小括号
\# 井号
\+ 加号
\- 减号
\. 英文句号
\! 感叹号
```

\\ 反斜杠
\` 反引号
\* 星号
\_ 下划线
\{\} 大括号
\[\] 中括号
\(\) 小括号
\# 井号
\+ 加号
\- 减号
\. 英文句号
\! 感叹号

