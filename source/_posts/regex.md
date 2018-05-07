
---

title: Regex in Python
date:  2018.4.27
categories:  Topics
tags:  Regex

---

正则表达式是一种用来匹配字符串的强有力的武器。它的设计思想是用一种描述性的语言来给字符串定义一个规则，凡是符合规则的字符串，我们就认为它“匹配”了，否则，该字符串就是不合法的。

<!-- more -->

## 字符

.       字符在正则表达式代表着可以代表任何一个字符（包括它本身）

\d	    可以匹配一个数字 等同于[0-9]

\D	    等同于[^0-9]匹配非数字

\w	    等同于[a-z0-9A-Z_]匹配大小写字母、数字和下划线

\W	    等同于[^a-z0-9A-Z_]等同于上一条取非

A|B     可以匹配A或B，所以(P|p)ython可以匹配'Python'或者'python'

^       表示行的开头，^\d表示必须以数字开头

\$       表示行的结束，\d$表示必须以数字结束

\s      可以匹配空白字符,等价于 [\t\n\r\f]。（也包括Tab等空白符）

\S      匹配任意非空字符  



## 量词 

跟着一个字符或一个表达式后面，表示前面重复的次数

\+       匹配前面的字符1次或多次（>=1）

\*       匹配前面的字符0次或多次（>=0）

?       匹配前面的字符1次或者多次 (0 or 1)

{m}	    匹配前面表达式m次 （=m）

{m,}	匹配前面表达式至少m次 (>=m)

{,n}	匹配前面的正则表达式最多n次 (<=m)

{m,n}	匹配前面的正则表达式m到n次  (>m,<n)


注意点：
以上量词都是贪婪模式，会尽可能多的匹配，如果要改为非贪婪模式，通过在量词后面跟随一个?来实现



## 贪婪匹配

因为正则表达式默认是“贪婪”的，贪婪匹配，也就是匹配尽可能多的字符。

“+”代表是字符重复一次或多次。但是这个多次到底是多少次。所以它会尽可能“贪婪”地多给我们匹配字符，在这个例子里也就是匹配到最后一个“.”。我们怎么解决这种问题呢？只要在“+”后面加一个“？”就好了。

例子：理想的结果是@hit.结果是@hit.edu.

当需要用到 ‘+’ 或 '*' 这种一定先想好到底是用贪婪型还是懒惰型


```python
import re

key = r"chuxiuhong@hit.edu.cn"
p1 = r"@.+\."#我想匹配到@后面一直到“.”之间的，在这里是hit
#p1 = r'@[a-z]{3}\.'
pattern1 = re.compile(p1)
print (pattern1.findall(key))
```

    ['@hit.edu.']
    

## 范围
[] 代表匹配里面的字符中的任意一个

[^] 代表除了内部包含的字符以外都能匹配，[^abc] 匹配除了a,b,c之外的字符

[0-9]	0123456789任意之一

[a-z]	小写字母任意之一

[A-Z]	大写字母任意之一

[0-9a-zA-Z\_] 可以匹配一个数字、字母或者下划线；

[0-9a-zA-Z\_]+ 可以匹配至少由一个数字、字母或者下划线组成的字符串，比如'a100'，'0_Z'，'Py3000'等等；

[a-zA-Z\_][0-9a-zA-Z\_]* 可以匹配由字母或下划线开头，后接任意个由一个数字、字母或者下划线组成的字符串，也就是Python合法的变量；

[a-zA-Z\_][0-9a-zA-Z\_]{0, 19} 更精确地限制了变量的长度是1-20个字符（前面1个字符+后面最多19个字符）。

## 转义符

特殊字符：\.^$?+*{}[]()|
以上特殊字符要想使用字面值，必须使用\进行转义

## 向前向后查找


?<=后面跟着的是前缀要求，?=后面跟的是后缀要求。

exp1(?=exp2)    表示exp1后面的内容要匹配exp2（前瞻）

exp1(?!exp2)    表示exp1后面的内容不能匹配exp2 （负前瞻）

(?<=exp2)exp1   表示exp1前面的内容要匹配exp2 （后顾）

(?<!exp2)exp1   表示exp1前面的内容不能匹配exp2 （负后顾）


例如：我们要查找hello，但是hello后面必须是world，正则表达式可以这样写："(hello)\s+(?=world)",用来匹配"hello wangxing"和"hello world"只能匹配到后者的hello

(?<=exp1)exp(?=exp2) 表示exp前面的内容要匹配exp1,后面的内容要匹配exp2,即exp1expexp2


匹配到两个括号中间的


```python
import re

key = r"<html><body><h1>hello world</h1></body></html>"#这段是你要匹配的文本
p1 = r"(?<=<h1>).+?(?=</h1>)"#这是我们写的正则表达式规则
pattern1 = re.compile(p1)#我们在编译这段正则表达式
matcher1 = re.search(pattern1,key)#在源文本中搜索符合正则表达式的部分
print(matcher1.group(0))#打印出来

print(re.search(p1,key).group(0))
```

    hello world
    hello world
    

## 回溯引用

原本要匹配&lt;h1&gt;\&lt;/h1&gt;之间的内容，现在你知道HTML有多级标题，你想把每一级的标题内容都提取出来。你也许会这样写：

p = r"&lt;h[1-6]&gt;.*?&lt;/h[1-6]&gt;" 

这样一来，你就可以将HTML页面内所有的标题内容全部匹配出来。即&lt;h1&gt;\&lt;/h1&gt;到&lt;h6&gt;\&lt;/h6&gt;的内容都可以被提取出来。但是我们之前说过，写正则表达式困难的不是匹配到想要的内容，而是尽可能的不匹配到不想要的内容。在这个例子中，很有可能你就会被下面这样的用例玩坏。

比方说

&lt;h1&gt;hello world&lt;/h3&gt;


```python
import re

key = r"<h1>hello world</h3>"
p1 = r"<h([1-6])>.*?</h\1>"
pattern1 = re.compile(p1)
m1 = re.search(pattern1,key)
print(m1.group(0))#这里是会报错的，因为匹配不到，你如果将源字符串改成</h1>
#结尾就能看出效果
```

    <h1>hello world</h1>
    

看到\1了吗？原本那个位置应该是[1-6]，但是我们写的是\1，我们之前说过，转义符\干的活就是把特殊的字符转成一般的字符，把一般的字符转成特殊字符。普普通通的数字1被转移成什么了呢？在这里1表示第一个子表达式，也就是说，它是动态的，是随着前面第一个子表达式的匹配到的东西而变化的。比方说前面的子表达式内是[1-6]，在实际字符串中找到了1，那么后面的\1就是1，如果前面的子表达式在实际字符串中找到了2，那么后面的\1就是2。

类似的，\2,\3,....就代表第二个第三个子表达式。

所以回溯引用是正则表达式内的一个“动态”的正则表达式，让你根据实际的情况变化进行匹配。

## re.match函数

re.match 尝试从字符串的起始位置匹配一个模式，如果不是起始位置匹配成功的话，match()就返回none。

<span class="mark">re.match(pattern, string, flags=0)</span>

pattern 匹配的正则表达式

string  要匹配的字符串

flags   标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等


可以使用group(num) 或 groups() 匹配对象函数来获取匹配表达式。

group(num=0)  匹配的整个表达式的字符串，group() 可以一次输入多个组号，在这种情况下它将返回一个包含那些组所对应值的元组。

groups()  返回一个包含所有小组字符串的元组，从 1 到 所含的小组号。


```python
import re 

print(re.match('www', 'www.google.com'))  # 在起始位置匹配，成功
print(re.match('com', 'www.google.com'))  # 不在起始位置匹配，失败

#group 分组，从左到右
print(re.match('(.*\d)\.(.*\d)\.', 'www1.google2.com3').group()) 
print(re.match('(.*\d)\.(.*\d)\.', 'www1.google2.com3').group(1)) 
print(re.match('(.*\d)\.(.*\d)\.', 'www1.google2.com3').group(2)) 

#group 嵌套，从外到内，再从左到右
print(re.match('((.*\d)\.(.*\d))\.', 'www1.google2.com3').group(1))  # group（1）是最外层
print(re.match('((.*\d)\.(.*\d))\.', 'www1.google2.com3').group(2))  # group（2）内层从左向右
print(re.match('((.*\d)\.(.*\d))\.', 'www1.google2.com3').group(3)) # group（3）内层从左向右
```

    <_sre.SRE_Match object; span=(0, 3), match='www'>
    None
    www1.google2.
    www1
    google2
    www1.google2
    www1
    google2
    

## re.search方法

re.search 扫描整个字符串并返回第一个成功的匹配。

<span class="mark">re.search(pattern, string, flags=0)</span>

pattern 匹配的正则表达式

string 要匹配的字符串

flags 标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等

可以使用group(num) 或 groups() 匹配对象函数来获取匹配表达式。


```python
import re 

print(re.search('www', 'www.google.com'))  # 在起始位置匹配，成功
print(re.search('com', 'www.google.com'))  # 不在起始位置匹配,成功
```

    <_sre.SRE_Match object; span=(0, 3), match='www'>
    <_sre.SRE_Match object; span=(11, 14), match='com'>
    

## re.match与re.search的区别

re.match只匹配字符串的开始，如果字符串开始不符合正则表达式，则匹配失败，函数返回None；而re.search匹配整个字符串，直到找到一个匹配。

## 检索和替换、删除

re.sub用于替换字符串中的匹配项。

<span class="mark">re.sub(pattern, repl, string, count=0)</span>

pattern : 正则中的模式字符串。

repl : 替换的字符串，也可为一个函数。

string : 要被查找替换的原始字符串。

count : 模式匹配后替换的最大次数，默认 0 表示替换所有的匹配。


```python
phone = '2004-959-559 # 这是一个电话号码'

#删除注释部分
num = re.sub('#.*','',phone)
print(num)

#移除非数字的内容
num = re.sub('\D','',phone)
print(num)
```

    2004-959-559 
    2004959559
    

## 匹配汉字

[\u4e00-\u9fa5] 至少匹配一个汉字

这两个unicode值正好是Unicode表中的汉字的头和尾。


```python
import re
print(re.match('[\u4e00-\u9fa5]', '你好')) 
line = 'study in 南京大学'
print(re.match('.*([\u4e00-\u9fa5]+大学)',line).group(1)) #贪婪匹配
print(re.match('.*?([\u4e00-\u9fa5]+大学)',line).group(1)) #非贪婪匹配
```

    <_sre.SRE_Match object; span=(0, 1), match='你'>
    京大学
    南京大学
    

## 小测试


```python
import re
m =[ 'XXX出生于2011年6月1日','XXX出生于2011年6月','XXX出生于2011/6/1','XXX出生于2011-6-1','XXX出生于2011-06-01','XXX出生于2011-06']
for i in m:
    print(re.match('.*出生于(\d{4}[年/-]\d{1,2}([月/-]\d{1,2}.?|[月/-]$|$))',i).group(1))
```

    2011年6月1日
    2011年6月
    2011/6/1
    2011-6-1
    2011-06-01
    2011-06
    

## 普遍邮箱适用的正则表达式


```python
import re
text = '45206091@qq.com rogovik1994@icloud.com sd676876@ru.com'
reg =  r'[A-Za-z0-9.]+@[A-Za-z0-9]+\.[a-z]{3}'
print(re.findall(reg,text))
```

    ['45206091@qq.com', 'rogovik1994@icloud.com', 'sd676876@ru.com']
    

## re.findall() 与 re.search() 的区别

re.search匹配整个字符串，直到找到一个匹配,返回一个Match对象

和re.match一样
通过Match对象内的group编号或命名，获得对应的值

注意到group(0)永远是原始字符串，group(1)、group(2)……表示第1、2、……个子串



re.findall返回一个列表对象，包含所有匹配的字符串

正则表达式中没有使用分组，即没有括号，返回值是匹配到的完整字符串的列表

正则表达式中有使用分组，即有括号，返回值是各个group的值所组合出来的列表

所以要通过findall，想要获得整个字符串的话，就要使用不带括号的，即没有分组


```python
import re
text = '45206091@qq.com rogovik1994@icloud.com sd676876@ru.com'
reg =  r'[A-Za-z0-9.]+@[A-Za-z0-9]+\.[a-z]{3}'
print(re.search(reg,text))# 返回第一个匹配的Match对象
print(re.search(reg,text).group(0))# group(0)获取值
print(re.findall(reg,text))

## 邮箱正则表达式
reg2 = r'[A-Za-z0-9_]+@[a-zA-Z0-9]+\.(cn|com|ru|net|gmail)'

email = re.search(reg,text) # 匹配到第一个邮箱
email2 = re.findall(reg2,text) # 结果是['com', 'com', 'com'] 为什么？
print(email.group(0))
print(email2) 
# 在re.findall(),中的正则表达式有使用分组，即有括号，返回值是各个group的值所组合出来的列表
```

    <_sre.SRE_Match object; span=(0, 15), match='45206091@qq.com'>
    45206091@qq.com
    ['45206091@qq.com', 'rogovik1994@icloud.com', 'sd676876@ru.com']
    45206091@qq.com
    ['com', 'com', 'com']
    
