
---

title: Iterable and Iterator
date:  2018.5.7
categories:  Topics
tags:  [iterable,iterator]


---


## 迭代工具

Python中有一类工具叫做迭代工具，他们能从左至右扫描对象。这包括了for循环、列表解析、in成员关系测试以及map内置函数等。

<!-- more -->

## 可迭代对象

可以用在上述迭代工具环境中，通过一次次迭代不断产生结果的对象。

可迭代对象分为两大类

1.实际保存的序列，即列表、元组，字符串；

2.“不一次性产生所有结果列表，而是可以在for循环中按需一次产生一个结果的对象”。

如：range函数返回值、zip函数返回值、enumerate函数返回值等等，与实际序列相对应，这个叫做按需产生对象的虚拟序列。

## 迭代器

可迭代对象支持内置函数iter，通过对可迭代对象调用iter函数，会返回一个迭代器，而“迭代器”支持内置函数next，通过不断对其调用next方法，会依次前进到序列中的下一个元素并将其返回，最后到达一系列结果的末尾时，会引发StopIteration异常。

补充说明一点，对迭代器调用iter方法，则会返回迭代器自身。



```python
L = [2,3,4]
I = iter(L)
print(iter(L))
print(I is L)
print(I is iter(I))
```

    <list_iterator object at 0x000001F3CB288EB8>
    False
    True
    

## 迭代过程

当任何可迭代对象传入到for循环或其他迭代工具中进行遍历时，迭代工具都是先通过iter函数获得与可迭代对象对应的迭代器，然后再对迭代器调用next函数，不断的依次获取元素，并在捕捉到StopIteration异常时确定完成迭代，这就是完整的迭代过程。这也称之为“迭代协议”


```python
L = [2,3,4]
I = iter(L)
print(next(I))
print(next(I))
print(next(I))
# print(next(I)) #报错 StopIteration   
```

    2
    3
    4
    

与手动迭代的示例过程相对应，下面是用for循环进行自动迭代的过程


```python
L = [2,3,4]
for x in L:
    print(x)
```

    2
    3
    4
    

## 文件对象

open函数返回的已打开的文件对象，也是可以一行一行的读取，直至文件结束，那很显然，他也是可迭代对象。


```python
f = open('myfile.txt')
print(f is iter(f))# 不管f是迭代对象还是迭代器，iter(f) 返回的是迭代器
print(next(f)) # “迭代器”支持内置函数next,说明文件对象是迭代器
print(next(f))
print(next(f)) 
```

    True
    hello text file
    
    goodbyt text file
    
    hahahahah
    

文件对象既是迭代器，又是可迭代对象。

reference:

http://python.jobbole.com/85240/

http://python.jobbole.com/86258/

https://zhuanlan.zhihu.com/p/24376869

https://zhuanlan.zhihu.com/p/32508947
