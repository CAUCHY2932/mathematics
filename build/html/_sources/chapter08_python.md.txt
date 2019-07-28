# python



## python基础

### python基本类型



### python中表示组的概念和定义

### 变量和运算符

### 分支、循环、条件和枚举

###  包、模块、函数

###  变量作用域





## python函数式编程
### python函数

### 闭包


### 匿名函数、高阶函数
#### lambda

#### map

#### reduce

#### filter



### 装饰器

#### 带参装饰器



## python面向对象

### 基本概念

#### 类
类名首字母大写，且不使用下划线，应该使用形如：StudentAddress之类的驼峰命名法
```python
class Student:
    pass
```
#### 类变量
```python
class Student:
    name = ''
    age = 0
```

#### 类方法
```python
class Student:
    def print_info():
        pass
```
#### 实例化

```python
class Student:
    name = ''
    age = 0
    

s = Student()
```
#### 实例方法
```python
class Student:
    def print_info(self, s):
        print(s)
stu = Student()

stu.print_info('nihao')
```
#### 静态方法

#### 实例变量

### 上下文管理器



## 正则表达式和json

### 正则表达式

### json

#### json格式化

python自带json模块

```python
import json


# loads方法，将字符串转换成python形式
obj = """
{
    "name": "john",
    "addr": "china chengdu",
    "gender": "male"
}
"""
# 生成的是dict对象
result = json.loads(obj)
print("type is {}".format(type(result)))
print(result)

# 相反，dumps将python对象转换成json对象(字符串)

asjson = json.dumps(result)
print("asjson type is {}".format(type(asjson)))
print(asjson)

```

所以，json模块就是在字符串和python对象之间进行转换，loads生成python对象，dumps生成字符串。

json.load()用来对文件对象进行转换

```python
import json

db = json.load(open('file.json'))
print(type(db))
print(db)
```
### xml


## python高级用法

### 枚举

### 一切皆是对象


## pythonic



### 字典代替swtich
官方推荐if-else
```c
switch (day)
{
    case 0:
        dayName = "sunday";
        break;
    case 1:
        dayName = "Monday";
        break;

    default :
        dayName = "Unknown";
        break;
}
```


#### 数据用法
```python
day = 0
switcher = {
    0: 'Sunday',
    1: 'Monday',
    2: 'Tuesday',
}
# 字典映射
# 没有实现default
day_name = switcher[day]
print(day_name)
day_name_2 = switcher.get(6, 'Unknown')
print(day_name_2)
```
#### 函数用法
```python
def get_sunday():
    return 'Sunday'
def get_monday():
    return 'Sunday'
def get_tuesday():
    return 'Sunday'
def get_default():
    return 'Unknown'

switcher = {
    0: get_sunday,
    1: get_monday,
    2: get_tuesday,
}
day = 2
day_name = switcher.get(day, get_default)()
```


### 表驱动法


#### 数据用法

#### 函数用法

### 列表推导式

```python
# map和filter也可以，但首推，列表推导
a = [x**2 for x in range(10) if x % 2 == 0]
print(a)
```

### 元组推导式

```python
# 元组推导(生成器)，可迭代
c = (x**2 for x in range(10) if x%2==0)
print(c)
```

### 字典推导式

```python
# 字典推导
d = {k:v for k, v in zip(range(10), range(6, 23))}
print(d)
demo_dict = {
    'a': 1,
    'b': 2,
    'c': 3
}
e = [k for k, v in demo_dict.items()]
print(e)
```



### None类型

None是python的'空'

```python
# 空字符串，空列表 0 False 都不是None
# 值比较==， 类型比较is，
print([]==None)
print(''==None)
print(0==None)

print([] is None)
print('' is None)
print(0 is None)

```

qita

```python

def judge(a):
    if not a:
        print('s')
    else:
        print('f')

    if a is None:
        print('s')
    else:
        print('f')

judge([])

judge(None)
# a 或是not a判空
# is None 判断布尔值
# None 不存在
# False 假

# if None
# if False


# python里
# if '' [] 对应None 对应False
# 判空和True False对应
```

## 利用python进行数据分析



