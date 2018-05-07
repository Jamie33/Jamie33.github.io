
---

title: What does named tuple do?
date:  2018.5.7
categories:  Skills
tags:  Skills
keywords:  namedtuple

---


```python
student=('Jim','16','male','Jim123@qq.com')

# name
print(student[0])

# age
print(student[1])

# sex
print(student[2])
    
#email
print(student[3])

#用元祖的下标索引来访问元组中的值，0，1，2，3名字没有意义
```

    Jim
    16
    male
    Jim123@qq.com

<!-- more -->   

## 方法一：利用对下标索引赋值，达到有意义


```python
#方法一：利用对下标索引赋值，达到有意义
NAME,AGE,SEX,EMAIL = range(0,4)
# name
print(student[NAME])

# age
print(student[AGE])

# sex
print(student[SEX])
    
#email
print(student[EMAIL])
```

    Jim
    16
    male
    Jim123@qq.com
    

## 方法二：collections 模块 namedtuple 方法


```python
#方法二：collections 模块 namedtuple 方法
from collections import namedtuple
student = namedtuple('student',['name','age','sex','email'])
s = student('Jim','16','male','Jim123@qq.com')
s.name
```




    'Jim'




```python
s.sex
```




    'male'




```python
s.age
```




    '16'




```python
s.email
```




    'Jim123@qq.com'


