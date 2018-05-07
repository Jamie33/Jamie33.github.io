
---

title: How to get frequency of elements in list
date:  2018.5.7
categories:  Skills
tags:  frquency
keywords:  [frequency,list]

---


```python
#创建一个随机序列
from random import randint
data = [randint(0,10) for x in range(0,20)]
data
```


<!-- more -->

    [2, 4, 7, 9, 0, 3, 9, 1, 5, 5, 3, 1, 6, 4, 2, 2, 3, 10, 9, 10]



## 方法一：统计出现频率,并筛选出现最多的3个元素


```python
#方法一：统计出现频率,并筛选出现最多的3个元素
data = [2, 4, 7, 9, 0, 3, 9, 1, 5, 5, 3, 1, 6, 4, 2, 2, 3, 10, 9, 10]

#统计频率
f = {}
for i in data:
    if i not in f:
        f[i] = 0
    f[i] += 1
print(f)

#排序，筛选

rank = sorted(f.items(),key = lambda x:x[1],reverse = True)
print(rank[:3])
```

    {2: 3, 4: 2, 7: 1, 9: 3, 0: 1, 3: 3, 1: 2, 5: 2, 6: 1, 10: 2}
    [(2, 3), (9, 3), (3, 3)]
    

## 方法二：用 Python 字典 fromkeys() 函数


```python
#方法二：用 Python 字典 fromkeys() 函数用于创建一个新字典，以序列seq中元素做字典的键，value为字典所有键对应的初始值。
d = dict.fromkeys(data,0) 
print(d)
for i in data:
    d[i] += 1
print(d)
#使用 collections.Counter, Counter类的目的是用来跟踪值出现的次数。它是一个无序的容器类型，以字典的键值对形式存储，其中元素作为key，其计数作为value。计数值可以是任意的Interger（包括0和负数）
from collections import Counter
c = Counter(d)
print(c)
m = c.most_common(3) # Counter.most_common(n)方法得到频率最高的n个元素的列表
print(m)
```

    {2: 0, 4: 0, 7: 0, 9: 0, 0: 0, 3: 0, 1: 0, 5: 0, 6: 0, 10: 0}
    {2: 3, 4: 2, 7: 1, 9: 3, 0: 1, 3: 3, 1: 2, 5: 2, 6: 1, 10: 2}
    Counter({2: 3, 9: 3, 3: 3, 4: 2, 1: 2, 5: 2, 10: 2, 7: 1, 0: 1, 6: 1})
    [(2, 3), (9, 3), (3, 3)]
    
