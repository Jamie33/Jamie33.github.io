
---

title: Scope in Python
date:  2018.4.26
categories:  Topics
tags:  scope

---

Python中有作用域链，变量会由内到外找，先去自己作用域去找，自己没有再去上级去找，直到找不到报错



```python
name = "lzl"
def f1():
    name = "Eric"
    def f2():
        name = "Snor"
        print(name)
    f2()
f1()
```

    Snor
    
<!-- more -->

```python
name = "lzl"
def f1():
    name = "Eric"
    def f2():
        print(name)
    f2()
f1()
```

    Eric
    


```python
在函数未执行之前，作用域已经形成了，作用域链也生成了
```


```python
name = "lzl"
 
def f1():
    print(name)
 
def f2():
    name = "eric"
    f1()
 
f2()
```

    lzl
    


```python
name = "lzl"
 
def f1():
    print(name)
 
def f2():
    name = "eric"
    return f1
 
ret = f2()
ret()
```

    lzl
    

## [lambda :x for x in range(10)]


```python
li = [lambda :x for x in range(10)]
print(li)
for f in li:
    print(f())
```

    [<function <listcomp>.<lambda> at 0x000001B9CE11F6A8>, <function <listcomp>.<lambda> at 0x000001B9CE11F620>, <function <listcomp>.<lambda> at 0x000001B9CE11F598>, <function <listcomp>.<lambda> at 0x000001B9CE11F2F0>, <function <listcomp>.<lambda> at 0x000001B9CE11F158>, <function <listcomp>.<lambda> at 0x000001B9CE11F268>, <function <listcomp>.<lambda> at 0x000001B9CE11F1E0>, <function <listcomp>.<lambda> at 0x000001B9CE11F488>, <function <listcomp>.<lambda> at 0x000001B9CE11F400>, <function <listcomp>.<lambda> at 0x000001B9CE0742F0>]
    9
    9
    9
    9
    9
    9
    9
    9
    9
    9
    

上面结果所有的都是9，而不是想当然的1-9
因为循环10次已经在生产list结束了，所以函数中的x向上找，找到了for循环结束的9
然后函数在没有执行前，内部代码不执行
上面代码实质如下


```python
li = []
for x in range(10):    
    def niming(): #无参数的函数
        return x
    li.append(niming)
print(li)
for f in li:
    print(f())
```

    [<function niming at 0x000001B9CE0742F0>, <function niming at 0x000001B9CE074AE8>, <function niming at 0x000001B9CE074048>, <function niming at 0x000001B9CE074730>, <function niming at 0x000001B9CE10DF28>, <function niming at 0x000001B9CE10D730>, <function niming at 0x000001B9CE11F0D0>, <function niming at 0x000001B9CE11F378>, <function niming at 0x000001B9CE11F048>, <function niming at 0x000001B9CE11F9D8>]
    9
    9
    9
    9
    9
    9
    9
    9
    9
    9
    

## [lambda x=x:x for x in range(10)]

把代码稍微修改一下，观察结果

https://stackoverflow.com/questions/12332500/x-lambda-x-for-x-in-range3-what-will-x0-return


```python
li = [lambda x=x:x for x in range(10)]

print(li)
for f in li:
    print(f())
```

    [<function <listcomp>.<lambda> at 0x000001B9CE0748C8>, <function <listcomp>.<lambda> at 0x000001B9CE074510>, <function <listcomp>.<lambda> at 0x000001B9CE0741E0>, <function <listcomp>.<lambda> at 0x000001B9CE074AE8>, <function <listcomp>.<lambda> at 0x000001B9CE074A60>, <function <listcomp>.<lambda> at 0x000001B9CE11FE18>, <function <listcomp>.<lambda> at 0x000001B9CE11FEA0>, <function <listcomp>.<lambda> at 0x000001B9CE11FAE8>, <function <listcomp>.<lambda> at 0x000001B9CE11FB70>, <function <listcomp>.<lambda> at 0x000001B9CE11FBF8>]
    0
    1
    2
    3
    4
    5
    6
    7
    8
    9
    


```python
li = []
for x in range(10):    
    def niming(x=x): #有参数，且是默认参数，所以下面f()不放参数也可以运行 为参数提供默认值，调用函数时可传可不传该默认参数的值
        return x
    li.append(niming)
print(li)
for f in li:
    print(f())
```

    [<function niming at 0x000001B9CE074AE8>, <function niming at 0x000001B9CE074048>, <function niming at 0x000001B9CE074730>, <function niming at 0x000001B9CE11F0D0>, <function niming at 0x000001B9CE11F378>, <function niming at 0x000001B9CE11F048>, <function niming at 0x000001B9CE10D6A8>, <function niming at 0x000001B9CE10DF28>, <function niming at 0x000001B9CE10D730>, <function niming at 0x000001B9CE128C80>]
    0
    1
    2
    3
    4
    5
    6
    7
    8
    9
    

## 初探函数作用域



四个作用域：LEGB，即L本地作用域，E内嵌作用域，G全局作用域和B内置作用域。

在一个函数中定义的是本地作用域，而模块（也就是一个xxx.py文件）中定义的是全局作用域。而内置作用域，我们使用时是直接使用变量名而不需要导入任何模块，比如一些内置的函数名：print等等

全局作用域的作用范围仅限于单个文件，别被全局二字所迷惑，这里的全局指的是一个文件的顶层的变量名仅对于这个文件内部的代码而言是全局的，在python中听到全局，你就应该想到模块二字。

再说说本地作用域：每次对函数的调用都创建一个新的本地作用域，赋值的变量名除非声明为全局变量或非本地变量，否则均为本地变量。在默认的情况下，所有函数定义的内部变量名都位于本地作用域（与函数调用相关的）内。



```python
x = 88
def func():
    x = 99
    print(x)

func()
print(x)
```

    99
    88
    

在这一段程序中，本地变量名x覆盖了全局变量名x，此时本地和全局的两个变量虽然都叫x，但他们是完全不同的变量。


## L-E-G-B 变量索引机制

当在python中使用某个变量名时，python按照L-E-G-B的顺序依次搜索四个作用域，L本地作用域，E即上一层def或者lambda的本地作用域，之后是全局作用域G，最后是内置作用域B，并且在第一处能找到作用名的地方停下来，如果变量名在这一次搜索中没有找到，python会报错。

变量的LEGB索引机制：对一个变量，首先在本地（函数内）查找；之后查找嵌套函数的本地作用域，然后再是查找当前的全局作用域，最后是内置作用域。

因此按照LEGB法则中规定的变量搜索顺序，在本地作用域中的变量名是会在本地作用域中覆盖在全局作用域和内置作用域中有相同变量名的变量，全局变量名会覆盖内置的同名变量名。

这里我们提到的只是在本地作用域去引用或者覆盖全局变量和内置变量。

但是，请注意！如果试图去修改，即在函数内部试图改变函数外部声明的值，那就得用global和nonlocal关键字了。

### global关键字

之前我们说过python中的变量不用声明，直接赋值使用，但是这个global关键字看上去就像一个声明，但是他不是一个类型的声明，而是一个变量命名空间的声明，它告诉python函数打算生成一个或多个全局变量。应用他，就可以在函数内部对全局变量进行引用和修改


```python
x = 88
def func():
    global x
    x = 99

func()
print(x)
```

    99
    

在这个例子中，我们对X加了一个global声明，以便在def之内引用并修改位于全局的变量x，而不是产生一个新的本地变量x并将其覆盖


```python
x,y,z = 1,2,3

def all_global():
    global x
    x = y + z

all_global()
print(x)
```

    5
    

这个例子中，x，y，z都是全局变量，y和z只是引用值，而对于x，我们想改变他的值，因此用了global进行引用声明。

### 嵌套函数的本地作用域 E

python有一个很有意思的地方，就是def函数可以嵌套在另一个def函数之中。调用外层函数时，运行到的内层def语句仅仅是完成对内层函数的定义，而不会去调用内层函数，除非在嵌套函数之后又显式的对其进行调用。


```python
x = 99

def f1():
    x = 88
    def f2():
        print(x)
    f2()

f1()
```

    88
    

### 嵌套作用域的一个特殊之处

本地作用域在函数结束后就立即失效，而嵌套作用域在嵌套的函数返回后却仍然有效。


```python
def f1():
    x = 88
    def f2():
        print(x)
    return f2

action = f1()
action()
```

    88
    

这个例子非常重要，也很有意思，函数f1中定义了函数f2，f2引用了f1嵌套作用域内的变量x，并且f1将函数f2作为返回对象进行返回。最值得注意的是我们通过变量action获取了返回的f2，虽然此时f1函数已经退出结束了，但是f2仍然记住了f1嵌套作用域内的变量名x。

上面这种语言现象称之为闭包：一个能记住嵌套作用域变量值的函数，尽管作用域已经不存在。

## 工厂函数

这里有一个应用就是工厂函数，工厂函数定义了一个外部的函数，这个函数简单的生成并返回一个内嵌的函数，仅仅是返回却不调用，因此通过调用这个工厂函数，可以得到内嵌函数的一个引用，内嵌函数就是通过调用工厂函数时，运行内部的def语句而创建的


```python
def maker(n):
    k = 8
    def action(x):
        return x ** n + k
    return action

f = maker(2)
print(f)
```

    <function maker.<locals>.action at 0x000001B9CE10DF28>
    


```python
def maker(n):
    k = 8
    def action(x):
        return x ** n + k
    return action

f = maker(2)
print(f(4))
```

    24
    

这里我们可以看出，内嵌的函数action记住了嵌套作用域内得两个嵌套变量，一个是变量k，一个是参数n，即使后面maker返回并退出。我们通过调用外部的函数maker，得到内嵌的函数action的引用。这种函数嵌套的方法在后面要介绍的装饰器中会经常用到。这种嵌套作用域引用，就是python的函数能够保留状态信息的主要方法了。

## 关键字nonlocal

本地函数通过global声明对全局变量进行引用修改，那么对应的，内嵌函数内部想对嵌套作用域中的变量进行修改，就要使用nonlocal进行声明。

不是一个类型的声明，而是一个变量命名空间的声明


```python
x = 88 #全局变量
def func():
    global x  # 本地函数对全局变量修改，要用 global 声明 
    x = 99

func()
print(x)
```


```python
def test(num):
    in_num = num # 嵌套作用域中的变量 
    def nested(label):
        nonlocal in_num #内嵌函数对嵌套作用域中的变量修改
        in_num += 1
        print(label, in_num)
    return nested

F = test(0)# test函数已经退出调用，但F仍然记着变量in_num 
print(F)
F('a') #由于我们使用了nonlocal，就可以在nested函数内修改它
F('a') #前面 in_num 已经修改成 1
F('a') #前面 in_num 已经修改成 2


G = test(100) 
G('mm')
# 多次调用工厂函数返回的不同内嵌函数副本F和G，彼此间的内嵌变量in_num是彼此独立隔离的。
```

    <function test.<locals>.nested at 0x0000026EBB2B4BF8>
    a 1
    a 2
    a 3
    mm 101
    

我们在nested函数中通过nonlocal关键字引用了内嵌作用域中的变量in_num，那么我们就可以在nested函数中修改他，即使test函数已经退出调用，这个“记忆”依然有效。

对比下面的例子


```python
def test(num):
    in_num = num # 嵌套作用域中的变量
    def nested(label):
        print(label, in_num) #本地（函数内）作用域
    return nested

F = test(0)# test函数已经退出调用，但F仍然记着变量in_num 
print(F)
F('a') #本地作用域中的 in_num 引用了嵌套作用域中的变量 in_num
F('a') #in_mun 只是引用，不能修改，所以一直都是0
F('a') #in_mun 只是引用，不能修改，所以一直都是0
```

    <function test.<locals>.nested at 0x0000026EBB2B4A60>
    a 0
    a 0
    a 0
    
