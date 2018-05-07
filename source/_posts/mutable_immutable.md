
---

title: Mutable and Immutable
date:  2018.4.27
categories:  Topics
tags:  [mutable,immutable]

---

immutable指对象一经创建，即不可修改。对象是不是immutable取决于数据类型，比如

整型（integer）、字符串（string）和元组（tuple）都是immutable，

而列表（list）、字典（dictionary）、集合（set）都是mutable。

<!-- more -->

### 不可变的元祖 tuple (immutable)

tuple和list非常类似，但是tuple一旦初始化就不能修改

tuple不能变了，它也没有append()，insert()这样的方法。其他获取元素的方法和list是一样的，你可以正常地使用索引，但不能赋值成另外的元素。

不可变的tuple有什么意义？因为tuple不可变，所以代码更安全。如果可能，能用tuple代替list就尽量用tuple。


```python
t = (1, 2) # 当你定义一个tuple时，在定义的时候，tuple的元素就必须被确定下来
print(t)

p = ()  # 定义一个空的tuple
print(type(p))

c = (1) #  要定义一个只有1个元素的tuple，如果你这么定义
print(type(c)) 
'''
定义的不是tuple，是1这个数！这是因为括号()既可以表示tuple，又可以表示数学公式中的小括号，
这就产生了歧义，因此，Python规定，这种情况下，按小括号进行计算，计算结果自然是1。
'''

a = (1,)  #所以只有1个元素的tuple定义时必须加一个逗号,，来消除歧义
print(type(a))
```

    (1, 2)
    <class 'tuple'>
    <class 'int'>
    <class 'tuple'>
    

### 一个“可变的”tuple


```python
t = ('a', 'b', ['A', 'B'])
print('before change, this tuple is'.format(t))
t[2][0] = 'X'
t[2][1] = 'Y'
print('after change,this tuple is'.format(t))
```

    before change, this tuple is
    after change,this tuple is
    

这个tuple定义的时候有3个元素，分别是'a'，'b'和一个list。不是说tuple一旦定义后就不可变了吗？怎么后来又变了？

表面上看，tuple的元素确实变了，但其实变的不是tuple的元素，而是list的元素。tuple一开始指向的list并没有改成别的list，所以，tuple所谓的“不变”是说，tuple的每个元素，指向永远不变。即指向'a'，就不能改成指向'b'，指向一个list，就不能改成指向其他对象，但指向的这个list本身是可变的！

理解了“指向不变”后，要创建一个内容也不变的tuple怎么做？那就必须保证tuple的每一个元素本身也不能变,就是immutable对象。

### 可哈希 hashable 

哈希表是在一个关键字和一个较大的数据之间建立映射的表，能使对一个数据序列的访问过程更加迅速有效。用作查询的关键字必须唯一且固定不变，于是只有immutable的对象才可以作为关键字，也叫hashable.
如上所述，整型（integer）、字符串（string）和元组（tuple）都可以作为关键字，而list不可以。


```python
d = {1:2, 'Ben':123, (1,2,3):456}
c = {1:2, 'Ben':123, [1,2,3]:456} # 报错，因为用做字典键的是可变的list
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-9-9b59cc95aefa> in <module>()
          1 d = {1:2, 'Ben':123, (1,2,3):456}
    ----> 2 c = {1:2, 'Ben':123, [1,2,3]:456} # 报错，因为用做字典键的是可变的list
    

    TypeError: unhashable type: 'list'


### 可变与不可变的特性

list列表是可变的，所以它的方法大多是就地修改，像 insert(),append(),extend()，sort(),reverse()然后没有返回，或者说返回值为None. pop()方法，它在末端删除一个元素，并可以将删除的元素作为返回值返回给调用者

tuple元祖是不可变的，所以它的方法大多无法就地修改，


```python
L = [1,2,3,4,5]
L = L.insert(6,2) 
print(L[2]) 
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-10-ad4d9f0257b3> in <module>()
          1 L = [1,2,3,4,5]
          2 L = L.insert(6,2)
    ----> 3 print(L[2])
    

    TypeError: 'NoneType' object is not subscriptable


这里犯了一个常见的错误，因为我们说过插入是就地修改，而不是返回修改后的新列表。Insert方法的返回值是None，然后你把None值赋给了L，这样就底失去之前列表的引用。

给tuple元祖排序

1.先将其转化为列表，本地排序后再转化回元组：

list.sort() 列表方法就地修改，无返回值

2.内置sorted函数：

sorted(可迭代对象)函数，返回一个已经排好序的list序列


```python
T = ('cc','bb','dd','aa')
tmp = list(T)
tmp.sort()
T = tuple(tmp)
print(T)

C = ('cc','bb','dd','aa')
print(sorted(C))

['aa', 'bb', 'cc', 'dd']
```

    ('aa', 'bb', 'cc', 'dd')
    ['aa', 'bb', 'cc', 'dd']
    




    ['aa', 'bb', 'cc', 'dd']


