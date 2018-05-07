
---

title: Module and Package in Python
date:  2018.5.7
categories:  Topics
tags:  [module,package]

---


## Module 模块

模块是一个.py文件

导入模块的方法以及调用相应模块中的方法（函数），类，属性，如下

<!-- more -->

```python
#导入模块
import modname
#调用
modname.itemname


#导入模块中特定的方法
from modname import itemname
#调用
itemname


#导入模块中全部方法
from modname import *
#调用
itemname
#注意：导入所有除了以一个下划线( _ )开头的所有方法)
```


Pycache 下的 XXX.pyc 编译文件只有在模块中import 或者 from … import 才会保存

Python XXX.py  运行是不会产生 Pycache!

## 模块的搜索路径

当你导入一个模块，Python 解析器对模块位置的搜索顺序是：

1、当前目录

2、如果不在当前目录，Python 则搜索在 shell 变量 PYTHONPATH 下的每个目录。

3、如果都找不到，Python会察看默认路径。UNIX下，默认路径一般为/usr/local/lib/python/。

模块搜索路径存储在 system 模块的 sys.path 变量中。变量里包含当前目录，PYTHONPATH和由安装过程决定的默认目录。


```python
import sys
print(sys.path) #返回的是列表
sys.path.append('') #添加到搜索路径中
```

    ['', 'c:\\users\\win7\\appdata\\local\\programs\\python\\python36\\python36.zip', 'c:\\users\\win7\\appdata\\local\\programs\\python\\python36\\DLLs', 'c:\\users\\win7\\appdata\\local\\programs\\python\\python36\\lib', 'c:\\users\\win7\\appdata\\local\\programs\\python\\python36', 'c:\\users\\win7\\appdata\\local\\programs\\python\\python36\\lib\\site-packages', 'c:\\users\\win7\\appdata\\local\\programs\\python\\python36\\lib\\site-packages\\IPython\\extensions', 'C:\\Users\\Win7\\.ipython']
    

## from modname import *

用用引用模块中所有函数,类,变量

如果想用\*通配符，又不想引用模块中的所有变量，可以在模块中用变量__all__进行限制，添加想引用的函数,类,变量。

## Package 包

包就目录中包含 __init__.py 文件。

通常是使用用“圆点模块名”的结构化模块命名空间，例如 A.B模块表示 A包中名为B的子模块


导入包中模块的方法以及调用相应模块中的方法（函数），类，属性，如下


```python
import package.subpackage.modname
package.subpackage.modname.itemname


from package.subpackage import modname
modname.itemname


from package.subpackage.modname import itemname
itemname
```

注意

使用 from package import item 方式导入包时，这个子项（item）既可以是包中的一个子模块（或一个子包），也可以是包中定义的其它命名，像函数、类或变量。

from package import item
(其中，item 可是使一个子包/一个子模块/包中定义的函数，类或变量)


使用类似 import item.subitem.subsubitem 这样的语法时，这些子项必须是包，最后的子项可以是包或模块，但不能是前面子项中定义的类、函数或变量。

import item.subitem.subsubitem
(其中每一项，除了最后一项，必须是包；
 最后一项，可以是模块/包，不可以是前面一项即包中定义的类、函数或变量)

## from package import *

以为会导入所有的子模块，不会，而且会出问题

所以执行 from package import * 时，在包中的 __init__.py 代码定义一个名为 __all__ 的列表，把需要的模块名加入列表，就会按照列表中给出的模块名进行导入。

## 包内引用



如果包中使用了子包结构（就像示例中的 project 包），导入子模块两种方式：绝对导入和相对导入

在同一个包中，用相对导入更方便
在不同包中，用绝对导入更方便，前提是没有很多层子包


```python
project/                          #Top-level package
      __init__.py               #Initialize the package
      A/                  #Subpackage for projcet A
              __init__.py
              PA.py
              ...
      B/                  #Subpackage for project B
              __init__.py
              PB.py
              PB2.py
              ...
      C/                  #Subpackage for project C
              __init__.py
              PC.py
              ...
```

绝对导入

在 PB.py 中导入不同包C中的PC.py 模块

from project.C. import PC



相对导入

以 PB.py 为例（基于当前模块的相对位置）

from . import PB2 (.表示当前包，在 PB.py 中导入同一个包中的 PB2.py 模块)

from ..C import PC.py(..表示上级包project，..C 表示上级包project下的C包,在 PB.py 中导入不同包C中的PC.py 模块)

from ..import A (在 PB.py 中导入上级包project包中的子包A)

前面的知识点
from package import item
(其中，item 可以是一个子包/一个子模块/包中定义的函数，类或变量)
