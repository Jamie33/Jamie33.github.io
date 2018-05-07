
---

title: How to sort value in dict
date:  2018.5.7
categories:  Skills
tags:  [dict,value]


---


```python
from random import randint
#建一个字典
data = {x: randint(60,100) for x in 'xyzabc'} #字典推导式
print(data)
#
sorted(data)
```

    {'x': 74, 'y': 90, 'z': 64, 'a': 79, 'b': 79, 'c': 99}
    




    ['a', 'b', 'c', 'x', 'y', 'z']

<!-- more -->

## 第一种方法 sorted


```python
#第一种方法 sorted
v = sorted(data.items(),key = lambda x:x[1])
v
    
```




    [('z', 64), ('x', 74), ('a', 79), ('b', 79), ('y', 90), ('c', 99)]



## 第二种方法 zip+sorted


```python
#第二种方法 zip+sorted

n = data.keys()
s = data.values()
i = data.items()
```




    dict_items([('x', 74), ('y', 90), ('z', 64), ('a', 79), ('b', 79), ('c', 99)])




```python
zip(s,n)
```




    <zip at 0x10d40bf08>




```python
l = list(zip(data.values(),data.keys()))
l
```




    [(74, 'x'), (90, 'y'), (64, 'z'), (79, 'a'), (79, 'b'), (99, 'c')]




```python
sorted(l)
```




    [(64, 'z'), (74, 'x'), (79, 'a'), (79, 'b'), (90, 'y'), (99, 'c')]


