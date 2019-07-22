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


### 类变量

### 类方法

### 实例方法

### 静态方法

### 实例变量

### 上下文管理器



## 正则表达式和json

### 正则表达式

### json和xml




## python高级用法

#### 


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
# 实现key之后的代码段
# 利用函数

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

### 元组推导式

### 字典推导式

