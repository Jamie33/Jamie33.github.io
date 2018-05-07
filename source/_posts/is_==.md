
---

title: is and ==
date:  2018.5.7
categories:  Topics
tags:  is


---


## python中的比较、相等性


```python
L1 = [1,2,['A','B']]
L2 = [1,2,['A','B']]
L3 = L1
print(L1 == L2, L1 is L2)
print(L1 == L3, L1 is L3)
```

    True False
    True True
    
<!-- more -->

==测试两个对象值的相等性，即递归的比较所有内嵌对象；值是否相等

is 测试两个对象的一致性，即是否是在一个内存空间。内存地址是否一致

## 真值问题

Python把任意的空数据结构视为假，把任何非空数据结构视为真

使用None，一般都起到一个空的占位符的作用，在真值判断的时候为false



```python
L = [None] * 10
print(L)
```

    [None, None, None, None, None, None, None, None, None, None]
    

内置bool函数可以测试一个对象是真还是假


```python
print(bool([1,2]))
print(bool())
print(bool(True))
print(bool(None))
print(bool('abcde'))
```

    True
    False
    True
    False
    True
    
