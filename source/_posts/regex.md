
---

title: Python 中的正则表达式 
date:  2018.7.19
categories:  Topics
tags:  Regex
keywords:  Regex

---

# Regex in Python

> 在编写处理字符串的程序或网页时，经常会有查找符合某些复杂规则的字符串的需要。正则表达式就是用于描述这些规则的工具。换句话说，正则表达式就是记录文本规则的代码。

## 字符

.       匹配除换行符之外的任意字符，当re.S(DOTALL)标记被指定时，这可以匹配包括换         行符的任意字符 

\d	    可以匹配一个数字 等同于[0-9]

\D	    等同于[^0-9]匹配非数字

\w	    等同于[a-z0-9A-Z_]匹配大小写字母、数字和下划线

\W	    等同于[^a-z0-9A-Z_]等同于上一条取非

A|B     可以匹配A或B，所以(P|p)ython可以匹配'Python'或者'python'

^       表示行的开头，^\d表示必须以数字开头

\$      表示行的结束，\d$表示必须以数字结束

\s      可以匹配空白字符,等价于 [\t\n\r\f]。（也包括Tab等空白符）

\S      匹配任意非空字符 

\n      匹配一个换行符

\t      匹配一个制表符

\A      匹配字符串开头

\Z      匹配字符串结尾，如果存在换行，只匹配到换行前的结束字符串

\z      匹配字符串结尾，如果存在换行，同时还会匹配换行符

\G      匹配最后匹配完成的位置 （不懂需要理解）

<!--more-->

## 量词 

跟着一个字符或一个表达式后面，表示前面重复的次数

\+       匹配前面的字符1次或多次（>=1）

\*       匹配前面的字符0次或多次（>=0）

?       匹配前面的字符0次或者1次 (0 or 1)

{m}	    匹配前面表达式m次 （=m）

{m,}	匹配前面表达式至少m次 (>=m)

{,n}	匹配前面的正则表达式最多n次 (<=m)

{m,n}	匹配前面的正则表达式m到n次  (>m,<n)


注意点：
以上量词都是贪婪模式，会尽可能多的匹配，如果要改为非贪婪模式，通过在量词后面跟随一个?来实现



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

(?:...) 不作为分组

## 转义符

特殊字符：\.^$?+*\{\}\[\]()|
以上特殊字符要想使用字面值，必须使用\进行转义

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
print (re.findall(r'@.+?\.',key))
```

    ['@hit.edu.']
    ['@hit.']
    

注意，如果 `.*?`放到字符串结尾，可能匹配不到任何内容了，因为它会匹配尽可能少的字符。下面例子结果，就没匹配到内容

`.*?` 尽量匹配少的内容
`.*`  尽量匹配多的内容


```python
import re

test = 'https://Jamie/python'
result1 = re.search(r'http.*?Jamie/(.*?)',test)
result2 = re.search(r'http.*?Jamie/(.*)',test)
print('result1',result1.group(1))
print('result2',result2.group(1))
```

    result1 
    result2 python
    

## 向前向后查找


?<=后面跟着的是前缀要求，?=后面跟的是后缀要求。

exp1(?=exp2)    表示exp1后面的内容要匹配exp2,返回exp1（前瞻）

exp1(?!exp2)    表示exp1后面的内容不能匹配exp2,返回exp1 （负前瞻）

(?<=exp2)exp1   表示exp1前面的内容要匹配exp2,返回exp1  （后顾）

(?<!exp2)exp1   表示exp1前面的内容不能匹配exp2,返回exp1  （负后顾）


```python
import re

print('--------------  exp1(?=exp2) ---------------')
pattern = r'\w(?=\d)' # 匹配数字的前一位，\w：单词字符[A-Za-z0-9]
print(re.findall(pattern,'abc1 edf1 xyz1')) 
print(re.findall(pattern,'jamie201807101740jamie')) 
print(re.findall(r'\w+(?=\d)','abc1 edf1 xyz1'))  # 匹配数字的前多位，\w：单词字符[A-Za-z0-9]


print('--------------  exp1(?!exp2) ---------------')
p = r'[A-Za-z](?!\d)'
print(re.findall(p,'jamie201807101740jamie'))  #匹配后面不是数字的一位字符，
print(re.findall(r'[A-Za-z]+(?!\d)','jamie201807101740jamie')) #匹配后面不是数字的字符串


print('--------------  (?<=exp2)exp1 ---------------')
pat = r'(?<=\d)[A-Za-z]+' # 匹配前面是数字的字母 返回exp1
print(re.findall(pat,'3abc21,5def31,7xyz41'))


print('--------------  (?<!exp2)exp1 ---------------')
pa = r'(?<!\d)[A-Za-z]+' # 匹配前面不是数字的字母 返回exp1
print(re.findall(pa,'abc21,def31,xyz41,Jamie124324Jamie'))
```

    --------------  exp1(?=exp2) ---------------
    ['c', 'f', 'z']
    ['e', '2', '0', '1', '8', '0', '7', '1', '0', '1', '7', '4']
    ['abc', 'edf', 'xyz']
    --------------  exp1(?!exp2) ---------------
    ['j', 'a', 'm', 'i', 'j', 'a', 'm', 'i', 'e']
    ['jami', 'jamie']
    --------------  (?<=exp2)exp1 ---------------
    ['abc', 'def', 'xyz']
    --------------  (?<!exp2)exp1 ---------------
    ['abc', 'def', 'xyz', 'Jamie', 'amie']
    

### 组合使用

例如：我们要查找hello，但是hello后面必须是world，正则表达式可以这样写："(hello)\s+(?=world)",用来匹配"hello wangxing"和"hello world"只能匹配到后者的hello

(?<=exp1)exp(?=exp2) 表示exp前面的内容要匹配exp1,后面的内容要匹配exp2,即exp1expexp2

匹配到两个括号中间的


```python
import re

key = r"<html><body><h1>hello world</h1></body></html>"#这段是你要匹配的文本
p1 = r"(?<=<h1>).+?(?=</h1>)"#这是我们写的正则表达式规则
matcher1 = re.search(p1,key)#在源文本中搜索符合正则表达式的部分
print(matcher1.group(0))#打印出来

print(re.search(p1,key).group(0))
```

    hello world
    hello world
    

## 回溯引用

原本要匹配`<h1>\</h1>`之间的内容，现在你知道HTML有多级标题，你想把每一级的标题内容都提取出来。你也许会这样写：

`p = r"<h[1-6]>.*?</h[1-6]>"`

这样一来，你就可以将HTML页面内所有的标题内容全部匹配出来。即`<h1>\</h1>到<h6>\</h6>`的内容都可以被提取出来。但是我们之前说过，写正则表达式困难的不是匹配到想要的内容，而是尽可能的不匹配到不想要的内容。在这个例子中，很有可能你就会被下面这样的用例玩坏。

比方说

`<h1>hello world</h3>`


```python
import re

key = r"<h1>hello world</h1>"
p1 = r"<h([1-6])>.*?</h\1>"
m1 = re.search(p1,key)
print(m1.group(0))#这里是会报错的，因为匹配不到，你如果将源字符串改成</h1>
#结尾就能看出效果
```

    <h1>hello world</h1>
    

看到\1了吗？原本那个位置应该是[1-6]，但是我们写的是\1，我们之前说过，转义符\干的活就是把特殊的字符转成一般的字符，把一般的字符转成特殊字符。普普通通的数字1被转移成什么了呢？在这里1表示第一个子表达式，也就是说，它是动态的，是随着前面第一个子表达式的匹配到的东西而变化的。比方说前面的子表达式内是[1-6]，在实际字符串中找到了1，那么后面的\1就是1，如果前面的子表达式在实际字符串中找到了2，那么后面的\1就是2。

类似的，\2,\3,....就代表第二个第三个子表达式。

所以回溯引用是正则表达式内的一个“动态”的正则表达式，让你根据实际的情况变化进行匹配。

如果我们要找从字符串中找出联系重复的数字，如11,22


```python
import re
# 找出第一次出现的重复数字
test = '..1234567891011213141516171820212223'
reg = r'([0-9a-zA-Z])\1+' #回溯引用,表示与前面相同的字符
m =re.search(reg,test)
print(m.group())
```

    11
    

(\d)\1的意思是，第一个是数字，第二个数字跟第一个数字相同。正则表达式中，用了一个专门的术语来描述\1这种概念，即回溯引用。

四个相同数字  (\d)\1\1\1  或   (\d)\1{3}

多个相同的数字
(\d)\1+

## re.match()

re.match 尝试从字符串的起始位置匹配一个模式，如果不是起始位置匹配成功的话，match()就返回none。

<span class="mark">re.match(pattern, string, flags=0)</span>

pattern 匹配的正则表达式

string  要匹配的字符串

flags   标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等


可以使用group(num) 或 groups() 匹配对象函数来获取匹配表达式。

group(num=0)  匹配的整个表达式的字符串

group() 可以一次输入多个组号，在这种情况下它将返回一个包含那些组所对应值的元组。

groups()  返回一个包含所有小组字符串的元组，从 1 到 所含的小组号。

命名分组 (?P<name>...)


```python
import re 

print(re.match('www', 'www.google.com'))  # 在起始位置匹配，成功
print(re.match('com', 'www.google.com'))  # 不在起始位置匹配，失败

# group 分组，从左到右
print(re.match('(.*\d)\.(.*\d)\.', 'www1.google2.com3').group()) # 所有匹配项
print(re.match('(.*\d)\.(.*\d)\.', 'www1.google2.com3').group(0)) # 所以匹配项
print(re.match('(.*\d)\.(.*\d)\.', 'www1.google2.com3').group(1)) # 第一个匹配项
print(re.match('(.*\d)\.(.*\d)\.', 'www1.google2.com3').group(2)) # 第二个匹配项
print(re.match('(.*\d)\.(.*\d)\.', 'www1.google2.com3').group(1,2)) 
print(re.match('(.*\d)\.(.*\d)\.', 'www1.google2.com3').groups()) 


# group 嵌套，从外到内，再从左到右
print(re.match('((.*\d)\.(.*\d))\.', 'www1.google2.com3').group(1))  # group（1）是最外层
print(re.match('((.*\d)\.(.*\d))\.', 'www1.google2.com3').group(2))  # group（2）内层从左向右
print(re.match('((.*\d)\.(.*\d))\.', 'www1.google2.com3').group(3)) # group（3）内层从左向右


# 命名分组
m = re.match('((?P<name>.*\d)\.(?P<website>.*\d))\.', 'www1.google2.com3')
print(m.group('name'))
print(m.group('website'))


# groupdict()
print(m.groupdict())
```

    <_sre.SRE_Match object; span=(0, 3), match='www'>
    None
    www1.google2.
    www1.google2.
    www1
    google2
    ('www1', 'google2')
    ('www1', 'google2')
    www1.google2
    www1
    google2
    www1
    google2
    {'name': 'www1', 'website': 'google2'}
    

## re.search()

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
    

match和search一旦匹配成功，就是一个match object对象，而match object对象有以下方法：

start() 返回匹配开始的位置,子串第一个字符的索引

end() 返回匹配结束的位置，子串最后一个字符的索引+1

span() 返回一个元组包含匹配 (开始,结束) 的位置


```python
import re

m = re.search(r'\d+','adf1234adsf')
print(m.start())
print(m.end())
print(m.span())
```

    3
    7
    (3, 7)
    

## re.match与re.search的区别

re.match只匹配字符串的开始，如果字符串开始不符合正则表达式，则匹配失败，函数返回None；而re.search匹配整个字符串，直到找到一个匹配。

## re.sub 检索和替换、删除

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
    


```python
import re
test = 'hello James, nihao James'
replacedtest = re.sub(r'hello (\w+), nihao \1','Jamie',test) # 匹配到整个字符串，并用Jamie替换掉整个字符串
print(replacedtest)

retest= re.sub(r'hello (\w+), nihao \1',r'\1',test) # 匹配到整个字符串，并用组1替换掉整个字符串
print(retest)

s = 'hello world ! hello hz !'
print(re.findall(r'(\w+) (\w+)',s)) # 可以看到匹配到2了项
r = re.sub(r'(\w+) (\w+)',r'\2 \1',s) # 每项中的2组位置互换
print(r)

s = 'Hello, *world*!'
s1 = 'Hello, *world* *good* !'
pattern = r'\*([^\*]+)\*'
print(re.search(pattern,s).group(1)) # 匹配项中的组1
print(re.sub(pattern,r'<em>\1<／em>',s)) 

print(re.findall(pattern,s1))
print(re.sub(pattern,r'<em>\1<／em>',s1)) # 匹配到两项，每项的组1替换匹配项
```

    Jamie
    James
    [('hello', 'world'), ('hello', 'hz')]
    world hello ! hz hello !
    world
    Hello, <em>world<／em>!
    ['world', 'good']
    Hello, <em>world<／em> <em>good<／em> !
    

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

reg =  r'[A-Za-z0-9_]+@[A-Za-z0-9]+\.[a-z]{3}

因为

\w 等同于[a-z0-9A-Z_]匹配大小写字母、数字和下划线

所以

reg =  r'\w+@\w+\.[a-z]{3}


```python
import re
text = '45206091@qq.com rogovik1994@icloud.com sd676876@ru.com  russell_gardner@icloud.com'
reg =  r'[A-Za-z0-9_]+@[A-Za-z0-9]+\.[a-z]{3}'
regx = r'\w+@\w+\.[a-z]{3}'
print(re.findall(reg,text))
print(re.findall(regx,text))
```

    ['45206091@qq.com', 'rogovik1994@icloud.com', 'sd676876@ru.com', 'russell_gardner@icloud.com']
    ['45206091@qq.com', 'rogovik1994@icloud.com', 'sd676876@ru.com', 'russell_gardner@icloud.com']
    

## re.findall() 与 re.search() 的区别

re.search匹配整个字符串，直到找到一个匹配,返回一个Match对象

和re.match一样
通过Match对象内的group编号或命名，获得对应的值

注意到group(0)永远是原始字符串，group(1)、group(2)……表示第1、2、……个子串



re.findall返回一个列表对象，包含所有匹配的字符串

正则表达式中没有使用分组，即没有括号，返回值是匹配到的完整字符串的列表

正则表达式中有使用分组，即有括号，返回值是各个group的值所组合出来的列表

所以要通过findall，想要获得整个字符串的话，有两种方法

第一：使用不带括号的，即没有分组

第二：使用括号，但是要用特殊符号 (?:) 不作为分组 


```python
import re
text = '45206091@qq.com rogovik1994@icloud.com sd676876@ru.com'
reg =  r'[A-Za-z0-9.]+@[A-Za-z0-9]+\.[a-z]{3}'
print(re.search(reg,text))# 返回第一个匹配的Match对象
print(re.search(reg,text).group(0))# group(0)获取值
print(re.findall(reg,text))

# 自己写的邮箱正则表达式，想想结果
reg2 = r'[A-Za-z0-9_]+@[a-zA-Z0-9]+\.(cn|com|ru|net|gmail)'

email = re.search(reg,text) # 匹配到第一个邮箱
email2 = re.findall(reg2,text) # 结果是['com', 'com', 'com'] 为什么？
print(email.group(0))
print(email2) 
# 在re.findall(),中的正则表达式有使用分组，即有括号，返回值是各个group的值所组合出来的列表


reg3 = r'[A-Za-z0-9_]+@[a-zA-Z0-9]+\.(?:cn|com|ru|net|gmail)'
print(re.findall(reg3,text))
# 在re.findall(),中的正则表达式有使用分组，如果想获取整个字符串，再括号前添加 ？：
```

    <_sre.SRE_Match object; span=(0, 15), match='45206091@qq.com'>
    45206091@qq.com
    ['45206091@qq.com', 'rogovik1994@icloud.com', 'sd676876@ru.com']
    45206091@qq.com
    ['com', 'com', 'com']
    ['45206091@qq.com', 'rogovik1994@icloud.com', 'sd676876@ru.com']
    

## 判断小数

https://www.hackerrank.com/challenges/introduction-to-regex/editorial


```python
import re
test = '+.4' 
test1 = '.5' 
test2 = '-+4.5'
test3 = '12.'   
test4 = '4.0o0' # 不是小数，内含字母 
reg = r'^[-+]?\d*\.\d+$'
print(bool(re.search(reg,test)))
print(bool(re.search(reg,test1)))
print(bool(re.search(reg,test2)))
print(bool(re.search(reg,test3)))
print(bool(re.search(reg,test4)))
```

    True
    True
    False
    False
    False
    

## re.compile(pattern, flags=0)

这个方法可以将正则字符串编译成正则表达式对象，以便在后面的匹配中复用

https://www.cnblogs.com/huxi/archive/2010/07/04/1771073.html

prog = re.compile(pattern)

result = prog.match(string)

等价于

result = re.match(pattern, string)

re提供了众多模块方法用于完成正则表达式的功能。这些方法可以使用Pattern实例的相应方法替代，唯一的好处是少写一行re.compile()代码，但同时也无法复用编译后的Pattern对象。


```python
import re

test = '3232xc'
reg = r'\d+'

pattern = re.compile(reg)
result1 = pattern.match(test)
#上面两行等价于下面一行
result2 = re.match(reg,test)
print(result1.group(0),result2.group(),sep='\n')
```

    3232
    3232
    

### Pattern 

Pattern对象是一个编译好的正则表达式，通过Pattern提供的一系列方法可以对文本进行匹配查找。

Pattern不能直接实例化，必须使用re.compile()进行构造。 

Pattern提供了几个可读属性用于获取表达式的相关信息：


pattern: 编译时用的表达式字符串。

flags: 编译时用的匹配模式。数字形式。

groups: 表达式中分组的数量。

groupindex: 以表达式中有别名的组的别名为键、以该组对应的编号为值的字典，没有别名的组不包含在内。


### pattern.match(string[, pos[, endpos]])  

从string的pos下标处起尝试匹配pattern；如果pattern 一开始就可匹配，则返回一个Match对象；如果匹配过程中pattern无法匹配，或者匹配未结束就已到达endpos，则返回None。 

pos和endpos的默认值分别为0和len(string)；re.match()无法指定这两个参数，参数flags用于编译pattern时指定匹配模式。 

注意：这个方法并不是完全匹配。当pattern结束时若string还有剩余字符，仍然视为成功。想要完全匹配，可以在表达式末尾加上边界匹配符'$'。


```python
import re

test = '1aaadaa'
test1 = '1aaadaab'

pattern = re.compile(r'aa')
pattern1 = re.compile(r'aa$')


print(pattern.match(test)) # 返回none,因为一开始久没匹配到
print(pattern.match(test,5))
print(pattern1.match(test,5))
print(pattern1.match(test1,5))
```

    None
    <_sre.SRE_Match object; span=(5, 7), match='aa'>
    <_sre.SRE_Match object; span=(5, 7), match='aa'>
    None
    

### pattern.search(string[, pos[, endpos]]) 

这个方法用于查找字符串中可以匹配成功的子串。从string的pos下标处起尝试匹配pattern，如果pattern结束时仍可匹配，则返回一个Match对象；若无法匹配，则将pos加1后重新尝试匹配；直到pos=endpos时仍无法匹配则返回None。 

pos和endpos的默认值分别为0和len(string))；re.search()无法指定这两个参数，参数flags用于编译pattern时指定匹配模式。 


```python
import re

test = 'hello world!'
pattern = re.compile(r'world')
print(pattern.search(test))
```

    <_sre.SRE_Match object; span=(6, 11), match='world'>
    

### pattern.findall(string[, pos[, endpos]]) 

搜索string，以列表形式返回全部能匹配的子串。 


```python
import re

test = 'aaadaa'
pattern = re.compile('aa')
print(pattern.findall(test))
```

    ['aa', 'aa']
    

### finditer(string[, pos[, endpos]]) 

返回string中所有与pattern相匹配的全部字串，返回形式为迭代器。若匹配成功，match()/search()返回的是Match对象，finditer()返回的也是Match对象的迭代器，获取匹配结果需要调用Match对象的group()、groups或group(index)方法。


```python
import re

test = 'aaadaa'
reg = 'aa'
m = re.finditer(r'(?={})'.format(reg),test)
x = re.findall(r'(?={})'.format(reg),test)
print(list(m))
print(x)
```

    [<_sre.SRE_Match object; span=(0, 0), match=''>, <_sre.SRE_Match object; span=(1, 1), match=''>, <_sre.SRE_Match object; span=(4, 4), match=''>]
    ['', '', '']
    

## 匹配叠加项

re.findall() 
返回不叠加的所有匹配项的列表对象
Return all non-overlapping matches of pattern in string, as a list of strings. The string is scanned left-to-right, and matches are returned in the order found.

不叠加时什么意思
'aaadaa'里面不叠加的算，有2个'aa',叠加的算有3个'aa'


```python
import re
test = 'aaadaa'
reg = 'aa'
i = re.findall(reg,test)
print(i)
```

    ['aa', 'aa']
    

如何匹配叠加的所有项？

pattern.search(string[, pos[, endpos]]) 从string的pos下标处和endpos下标处之间匹配pattern

re.search()无法指定这两个参数


```python
# 第一种遍历全部项

import re

test = 'aaadaa'
pattern = re.compile('aa')
for i in range(len(test)):
    p = pattern.match(test,i)
    if p:
        print(p.group())
```

    aa
    aa
    aa
    


```python
#第三种 从匹配到的项的开头索引下一位作为下一次查找的开头，逐渐减少匹配范围

import re

test = 'aaadaa'
reg = 'aa'
pattern = re.compile(reg)
r = pattern.search(test)
while r:
    print(r.group())
    r = pattern.search(test,r.start()+1)
```

    aa
    aa
    aa
    

## 修饰符

正则表达式可以包含一些可选标志修饰符来控制匹配的模式。修饰符被指定为一个可选的表示。

re.I   使匹配对大小写不敏感

re.L   做本地化识别(locale-aware)匹配

re.M   多行匹配，影响^和$

re.S   使.匹配包括换行在内的所有字符

re.U   根据Unicode字符集解析字符。这个标志影响\w,\W,\b,\B

re.X   该标志通过给与你更灵活的格式以便你将正则表达式写得更易于理解

在网页匹配中，较为常用的是 re.S 和 re.I


```python
import re

test = '''Hello Jamie33 worldthis is a Regex Demo
'''
print(re.match(r'He.*?(\d+).*?Demo$',test).group(1))
```

    33
    


```python
现在稍作修改，字符串里面加一个换行符，看看结果如何
```


```python
import re

test = '''Hello Jamie33 world
this is a Regex Demo
'''
print(re.match(r'He.*?(\d+).*?Demo$',test).group(1))
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-9-3373d4ec7df0> in <module>()
          4 this is a Regex Demo
          5 '''
    ----> 6 print(re.match(r'He.*?(\d+).*?Demo$',test).group(1))
    

    AttributeError: 'NoneType' object has no attribute 'group'


结果报错，为什么加了一个换行符，就匹配不到了。这是因为`.`匹配的是除换行符之外的任意字符，当遇到换行符时`.*？`就不能匹配了，所以失败。这里只需要加一个修饰符`re.S`就可以修正这个错误


```python
import re

test = '''Hello Jamie33 world
this is a Regex Demo
'''
print(re.match(r'He.*?(\d+).*?Demo$',test,re.S).group(1))
```

    33
    
