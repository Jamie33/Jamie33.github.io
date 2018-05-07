
---

title: How to find common keys in dict
date:  2018.5.7
categories:  Skills
tags:  commonkey
keywords:  [dict,common key]

---

## 字典生产式


```python
from random import randint,sample 
# random.sample(sequence, k)，从指定序列中随机获取指定长度的片断。sample函数不会修改原有序列。
s1 = {x: randint(3,4) for x in sample('abcdef',randint(4,6))}
s2 = {x: randint(3,4) for x in sample('abcdef',randint(4,6))}
s3 = {x: randint(3,4) for x in sample('abcdef',randint(4,6))}
print(s1)
print(s2)
print(s3)
```

    {'d': 4, 'a': 3, 'e': 3, 'f': 4}
    {'c': 4, 'b': 3, 'e': 3, 'f': 4}
    {'e': 3, 'c': 4, 'd': 3, 'a': 3}
    
<!-- more -->

## 方法一：通过if 找到每个字典的公共键


```python

common_key = [i for i in s1 if i in s2 and i in s3]
print(common_key)

key = []
for i in s1:
    if i in s2 and i in s3:
        key.append(i)
print(key)
```

    ['e']
    ['e']
    

## 方法二：用集合set的数学意义上的交集、并集


```python
#.keys() 返回的是一个set-like 对象，所以set具备的集合计算也可以用
common_key = s1.keys()&s2.keys()&s3.keys()
print(common_key)
```

    {'e'}
    


```python
#把多个字典合并到一起

'''
This PEP proposes to change the .keys(), .values() and .items() methods of the built-in dict type 
to return a set-like or unordered container object whose contents are 
derived from the underlying dictionary rather than a list which is a 
copy of the keys, etc
'''
# 其中.keys().items() 返回的是一个set-like 对象，所以set具备的集合计算也可以用
help(dict.keys)
help(dict.values)
help(dict.items)

# 用 .keys(), .items()方法键，键值对并集，交集，差集
print(s1.keys())  #返回一个字典所有的键
print(s1.keys()|s2.keys()|s3.keys()) # 并集
print(s1.keys()&s2.keys()&s3.keys()) # 交集

print(s1.items()) #返回一个字典所有的键值对,键值对的交集要求键和值都一样
print(s1.items()&s2.items()&s3.items()) #交集



# 用 class dict(iterable, **kwarg) 创建一个并集字典,会影响相同键不同值的项  
union = dict(s1,**s2)  
print (union)
```

    Help on method_descriptor:
    
    keys(...)
        D.keys() -> a set-like object providing a view on D's keys
    
    Help on method_descriptor:
    
    values(...)
        D.values() -> an object providing a view on D's values
    
    Help on method_descriptor:
    
    items(...)
        D.items() -> a set-like object providing a view on D's items
    
    dict_keys(['d', 'a', 'e', 'f'])
    {'e', 'd', 'a', 'f', 'c', 'b'}
    {'e'}
    dict_items([('d', 4), ('a', 3), ('e', 3), ('f', 4)])
    {('e', 3)}
    {'d': 4, 'a': 3, 'e': 3, 'f': 4, 'c': 4, 'b': 3}
    

## 方法三：适用于多个字典


```python
#第一步用 map 函数返回所有字典所有的键
'''

map()函数接收两个参数，一个是函数，一个是Iterable，
map将传入的函数依次作用到序列的每个元素，并把结果作为新的Iterator返回。
由于结果r是一个Iterator，Iterator是惰性序列，
因此通过list()函数让它把整个序列都计算出来并返回一个list。
'''

s = [s1,s2,s3]
r = map(lambda x:x.keys(),s)
r1 = map(dict.keys,s)  # r 和 r1 是同等效果，得出讨论函数与方法的区别,dict.keys是函数  
print(list(r1))
# 用 reduce 函数算出前一项与后一项的交集

'''
reduce把一个函数作用在一个序列[x1, x2, x3, ...]上，这个函数必须接收两个参数，
reduce把结果继续和序列的下一个元素做累积计算
reduce(f, [x1, x2, x3, x4]) = f(f(f(x1, x2), x3), x4)
'''



from functools import reduce
print(reduce(lambda a,b:a&b,r))
# 前面如果有print把r1这个 Iterator 序列全部打出来，r1里面就是空的，所以这里如果用r1就会报错



print(list(r1)) #空序列，因为前面print完了

print(reduce(lambda a,b:a&b,r1)) 



```

    [dict_keys(['d', 'a', 'e', 'f']), dict_keys(['c', 'b', 'e', 'f']), dict_keys(['e', 'c', 'd', 'a'])]
    {'e'}
    []
    []
    


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-73-b7dcf2afc47f> in <module>()
         32 print(list(r1)) #空序列，因为前面print完了
         33 
    ---> 34 print(reduce(lambda a,b:a&b,r1))
         35 
         36 
    

    TypeError: reduce() of empty sequence with no initial value


## 函数与方法的区别  


```python
#函数与方法的区别  
'''
在Python中，对这两个东西有明确的规定：

函数function —— A series of statements which returns some value to a caller. 
It can also be passed zero or more arguments which may be used in the execution 
of the body.

方法method —— A function which is defined inside a class body. If called as an 
attribute of an instance of that class, the method will get the instance object as 
its first argument (which is usually called self).

与类和实例无绑定关系的function都属于函数（function）；
与类和实例有绑定关系的function都属于方法（method）。
'''
#函数与方法 ？？？
s1.keys() == dict.keys(s1)
#(方法)s1.keys() == dict.keys(s1)(函数)？？？'

```




    True



## *和**符号


```python
'''
在python中，当*和**符号出现在函数定义的参数中时，表示任意数目参数收集。*arg表示任意多个无名参数，类型为tuple;**kwargs表示关键字参数，为dict，使用时需将*arg放在**kwargs之前，否则会有“SyntaxError: non-keyword arg after keyword arg”的语法错误
'''

# *允许你传入0个或任意个参数，这些可变参数在函数调用时自动组装为一个tuple。
def f(a,*args):
    print(args)

f(1,2,3,4)


a = [1,2,3,4,5]
b = (1,2,3,4,5)
c = range(1,6)
print(a)
print(b)
print(c)
print(*a)
print(*b)
print(*c)


def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    print(sum)

calc(1,2,3,4)


# **,关键字参数允许你传入0个或任意个含参数名的参数,这些关键字参数在函数内部自动组装为一个dict。
def d(**kargs):
    print(kargs)
    
d(a=1,b=2)

#在函数头部能混合一般参数、*参数以及**去实现更加灵活的调用方式。
def h(a,*args,**kargs):
    print(a,args,kargs)

h(1,2,3,x=4,y=5)

def person(name,age,**kw):
    print('name:',name,'age:',age,'other:',kw)
    
person('Adam', 45, gender='M', job='Engineer')




'''
上面是在函数定义的时候写的*和**形式，那反过来，如果*和**语法出现在函数调用中又会如何呢？他会解包参数的集合。例如，我们在调用函数时能够使用*语法，在这种情况下，它与函数定义的意思相反，他会解包参数的集合，而不是创建参数的集合。
'''

#通过一个元组给一个函数传递四个参数，并且让python将它们解包成不同的参数。
def func(a,b,c,d):
    print(a,b,c,d)

a = (1,2,3,4)
func(*a)

# 如果已经有一个list或者tuple，在函数调用时，在参数前加*，把list/tuple/range序列中的元素一个一个传到函数里面
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    print(sum)

num = [1,2,3,4]
calc(*num)


#在函数调用时，**会以键/值对的形式解包一个字典，使其成为独立的关键字参数。

def person(name,age,**kw):
    print('name:',name,'age:',age,'other:',kw)

    
#如果已经有一个dict,在参数前加**，把该dict的所有key-value用转换为关键字参数传进去,
extra = {'city': 'Beijing', 'job': 'Engineer'}
person('Jack', 24, **extra)
```

    (2, 3, 4)
    [1, 2, 3, 4, 5]
    (1, 2, 3, 4, 5)
    range(1, 6)
    1 2 3 4 5
    1 2 3 4 5
    1 2 3 4 5
    30
    {'a': 1, 'b': 2}
    1 (2, 3) {'x': 4, 'y': 5}
    name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}
    1 2 3 4
    30
    name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
    

## 创建字典的5种方法


```python
'''在python中，这两个是python中的可变参数，*arg表示任意多个无名参数，类型为tuple;**kwargs表示关键字参数，为dict，使用时需将*arg放在**kwargs之前，否则会有“SyntaxError: non-keyword arg after keyword arg”的语法错误
'''
#创建字典的5种方法

'''
class dict(**kwarg)
class dict(mapping, **kwarg)
class dict(iterable, **kwarg)
'''

#创建一个空字典
empty_dict = dict() 
print(empty_dict)

#用**kwargs可变参数传入关键字创建字典
a = dict(one=1,two=2,three=3) 
print(a)

#传入映射对象
b = dict(zip(['one','two','three'],[1,2,3]))
print(list(zip(['one','two','three'],[1,2,3])))
print(b)

#传入可迭代对象
c = dict([('one', 1), ('two', 2), ('three', 3)])
print(c)

c1 = dict([('one', 1), ('two', 2), ('three', 3),('three', 4),('three', 5)])
print(c1)
'''
可迭代对象中的每一项自身必须是可迭代的，并且每一项只能有两个对象。第一个对象成为新字典的键，第二个对象成为其键对应的值。如果键有重复，其值为最后重复项的值。
'''

#传入字典创建字典
d = dict({'one': 1, 'two': 2, 'three': 3})
print(d)


print(a == b == c == d)
```

    {}
    {'one': 1, 'two': 2, 'three': 3}
    [('one', 1), ('two', 2), ('three', 3)]
    {'one': 1, 'two': 2, 'three': 3}
    {'one': 1, 'two': 2, 'three': 3}
    {'one': 1, 'two': 2, 'three': 5}
    {'one': 1, 'two': 2, 'three': 3}
    True
    

## 获取字典中值最大的项


```python
#获取字典中值最大的项

a  = {'a': 4, 'b': 3, 'd': 3, 'e': 4, 'c': 3}

#max()函数


m = max(a.values())
print(m)

# 用Counter.most_common方法
from collections import Counter
c = Counter(a)
print(c)
print(c.most_common(1))
```

    4
    Counter({'a': 4, 'e': 4, 'b': 3, 'd': 3, 'c': 3})
    [('a', 4)]
    


```python
dir(a)
```




    ['__class__',
     '__contains__',
     '__delattr__',
     '__delitem__',
     '__dir__',
     '__doc__',
     '__eq__',
     '__format__',
     '__ge__',
     '__getattribute__',
     '__getitem__',
     '__gt__',
     '__hash__',
     '__init__',
     '__init_subclass__',
     '__iter__',
     '__le__',
     '__len__',
     '__lt__',
     '__ne__',
     '__new__',
     '__reduce__',
     '__reduce_ex__',
     '__repr__',
     '__setattr__',
     '__setitem__',
     '__sizeof__',
     '__str__',
     '__subclasshook__',
     'clear',
     'copy',
     'fromkeys',
     'get',
     'items',
     'keys',
     'pop',
     'popitem',
     'setdefault',
     'update',
     'values']


