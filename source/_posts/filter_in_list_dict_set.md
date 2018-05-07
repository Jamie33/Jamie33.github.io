
---

title: How to filter in list,set and dict
date:  2018.5.7
categories:  Skills
tags:  filter
keywords:  [filter,list,set,dict]

---

## 过滤掉列表中的负数【列表的三种方法】


```python
#用列表生成式建立一个10到-10之间随机列表
from random import randint
data = [randint(-10,10) for i in range(10)]
print(data)

<!-- more -->

#方法一：for 循环
c = []
for i in data:
    if i > 0:
        c.append(i)
print(c)

#方法二：列表解析
d = [i for i in data if i > 0]
print(d)

#方法三：filter 函数
f = filter(lambda x:x > 0,data)
print(list(f))
```

    [-6, 0, 4, -6, -7, 2, 9, -10, 2, 4]
    [4, 2, 9, 2, 4]
    [4, 2, 9, 2, 4]
    [4, 2, 9, 2, 4]
    


```python
#用字典生成式建立一个20人的字典
from random import randint
p = {i:randint(60,100) for i in range(1,21)}
print(p)
p ={1: 88, 2: 80, 3: 76, 4: 92, 5: 88, 6: 93, 7: 91, 8: 100, 9: 60, 10: 68, 11: 85, 12: 95, 13: 73, 14: 77, 15: 65, 16: 82, 17: 67, 18: 87, 19: 89, 20: 92}
#筛出字典中值高于90的项

#方法一：for 循环
h = {}
for k,v in p.items():
    if v > 90:
        h[k] = p[k]
print(h)

#方法二：字典解析

g = {k:v for k,v in p.items() if v>90}
print(g)
```

    {1: 85, 2: 93, 3: 60, 4: 65, 5: 97, 6: 83, 7: 79, 8: 61, 9: 80, 10: 69, 11: 95, 12: 98, 13: 91, 14: 94, 15: 64, 16: 60, 17: 85, 18: 95, 19: 64, 20: 60}
    {4: 92, 6: 93, 7: 91, 8: 100, 12: 95, 20: 92}
    {4: 92, 6: 93, 7: 91, 8: 100, 12: 95, 20: 92}
    


```python
#用列表生成式建立一个10个数的集合
s = set([randint(-10,10) for x in range(10)])
print(s)

#筛出集合中能被3整除的元素

#方法一：for 循环
n = set()#创建空集合
for i in s:
    if i%3 == 0:
       n.add(i)
print(n)

#方法二：列表解析
w = set([i for i in s if i%3 == 0])
w1 = {i for i in s if i%3 == 0}#和字典解析的区别在于前面没有冒号
print(w)
print(w1)
```

    {2, 6, 9, -8, -7, -3, -1}
    {9, -3, 6}
    {9, -3, 6}
    {9, -3, 6}
    
