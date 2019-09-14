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




## flask（李辉）

### 单元测试



```python
# python单元测试

# https://read.helloflask.com/c9-test
import unittest

from module_foo import sayhello


class SayHelloTestCase(unittest.TestCase):  # 测试用例

    def setUp(self):  # 测试固件
        pass

    def tearDown(self):  # 测试固件
        pass

    def test_sayhello(self):  # 第 1 个测试
        rv = sayhello()
        self.assertEqual(rv, 'Hello!')

    def test_sayhello_to_somebody(self)  # 第 2 个测试
        rv = sayhello(to='Grey')
        self.assertEqual(rv, 'Hello, Grey!')


if __name__ == '__main__':
    unittest.main()
```

测试方法

```python
## 测试方法
#在测试方法里，我们使用断言方法来判断程序功能是否正常。以第一个测试方法为例，我们先把 sayhello() 函数调用的返回值保存为 rv 变量（return value），然后使用 self.assertEqual(rv, 'Hello!') 来判断返回值内容是否符合预期。如果断言方法出错，就表示该测试方法未通过。
# 下面是一些常用的断言方法：
assertEqual(a, b) 
assertNotEqual(a, b) 
assertTrue(x) 
assertFalse(x) 
assertIs(a, b) 
assertIsNot(a, b) 
assertIsNone(x) 
assertIsNotNone(x) 
assertIn(a, b) 
assertNotIn(a, b) 
# 这些方法的作用从方法名称上基本可以得知。
```

测试固件

```python
## 测试固件
import unittest

from app import app, db, Movie, User


class WatchlistTestCase(unittest.TestCase):

    def setUp(self):
        # 更新配置
        app.config.update(
            TESTING=True,
            SQLALCHEMY_DATABASE_URI='sqlite:///:memory:'
        )
        # 创建数据库和表
        db.create_all()
        # 创建测试数据，一个用户，一个电影条目
        user = User(name='Test', username='test')
        user.set_password('123')
        movie = Movie(title='Test Movie Title', year='2019')
        # 使用 add_all() 方法一次添加多个模型类实例，传入列表
        db.session.add_all([user, movie])
        db.session.commit()

        self.client = app.test_client()  # 创建测试客户端
        self.runner = app.test_cli_runner()  # 创建测试命令运行器

    def tearDown(self):
        db.session.remove()  # 清除数据库会话
        db.drop_all()  # 删除数据库表

    # 测试程序实例是否存在
    def test_app_exist(self):
        self.assertIsNotNone(app)

    # 测试程序是否处于测试模式
    def test_app_is_testing(self):
        self.assertTrue(app.config['TESTING'])
        
    # 测试主页
    def test_index_page(self):
        response = self.client.get('/')
        data = response.get_data(as_text=True)
        self.assertIn('Test\'s Watchlist', data)
        self.assertIn('Test Movie Title', data)
        self.assertEqual(response.status_code, 200)
```

运行测试

```python
# 运行测试
# 在程序结尾增加以下代码
if __name__ == '__main__':
    unittest.main()
```

使用下面的命令执行测试：

```bash
$ python test_watchlist.py
...............
----------------------------------------------------------------------
Ran 15 tests in 2.942s


OK
```

如果测试出错，你会看到详细的错误信息，进而可以有针对性的修复对应的程序代码，或是调整测试方法。

```bash
# 测试覆盖率
# 为了让让程序更加强壮，你可以添加更多、更完善的测试。那么，如何才能知道程序里有哪些代码还没有被测试？整体的测试覆盖率情况如何？我们可以使用 [Coverage.py]# (https://coverage.readthedocs.io/en/v4.5.x/)来检查测试覆盖率，首先安装它（添加 `--dev`参数将它作为开发依赖安装）：

$ pipenv install coverage --dev

#使用下面的命令执行测试并检查测试覆盖率：

$ coverage run --source=app test_watchlist.py


# 因为我们只需要检查程序脚本 app.py 的测试覆盖率，所以使用 `--source`选项来指定要检查的模块或包。

# 最后使用下面的命令查看覆盖率报告：

$ coverage report
Name     Stmts   Miss  Cover
----------------------------
app.py     146      5    97%
```

生成覆盖率文件

```python
同时在 .gitignore 文件后追加下面两行，忽略掉生成的覆盖率报告文件：


htmlcov/
.coverage
```

通过测试后，我们就可以准备上线程序了。结束前，让我们提交代码：

```bash
# 代码提交
$ git add .
$ git commit -m "Add unit test with unittest"
$ git push
```

## flask（七月）

flask高级编程

flask restful api

### restapi项目

#### 概述

该项目用于生产系统后台建立报表和常规的管理，但是相比往常的系统，有以下特点：
- 无权限控制
- 无删除、更新、创建等修改性操作
- 数据库查询复杂多样，而且对数据一致性要求高
- 数据来源多样，可能来源于多个数据系统
- 跨域，前后端分离

#### 工程创建

目录结构大致如下
```
.
├── README.md
├── api_single
│   ├── __init__.py
│   ├── app.py
│   ├── config.py
│   ├── libs
│   │   ├── __init__.py
│   │   ├── error.py
│   │   ├── error_code.py
│   │   └── time_helper.py
│   ├── require.txt
│   ├── secure.py
│   └── v1
│       ├── __init__.py
│       ├── fake.py
│       ├── fake_helper.py
│       ├── forms.py
│       ├── func_helper.py
│       ├── main.py
│       ├── model.py
│       ├── raw_sql_op.py
│       ├── time_test.py
│       └── view_model.py
└── raw_sql.sql
```
app.py 项目入口文件
config.py 项目配置文件
secure.py 安全信息文件
require.txt 需求文件
v1 版本内容文件夹，包含视图函数、模型文件、私有帮助函数库文件
libs 自定义帮助函数库文件夹



### restapi重构篇

```
.
├── app
│   ├── api
│   │   ├── __init__.py
│   │   └── statement.py
│   ├── libs
│   │   ├── error.py
│   │   └── tools.py
│   ├── models
│   │   ├── base.py
│   │   └── statement.py
│   └── service
│       └── statement.py
├── gunicorn_config.py
├── requirements.txt
├── settings.py
├── statement.py
└── unit_test

```

#### 入口文件

statement.py



#### 视图文件

app/api/statement.py



#### 帮助文件

app/service/statement.py



#### 需求文件

requirement.py



#### 错误处理



#### 视图



#### 数据库查询



#### 返回格式封装






## flask（狗书）

## flask（董伟明）

## django(胡阳)







## flask项目代码结构的建议

https://lepture.com/en/2018/structure-of-a-flask-project

http://charlesleifer.com/blog/structuring-flask-apps-a-how-to-for-those-coming-from-django/

## flask项目中restful

如果你要用RESTful来实现鉴权，应当使用BasicAuth，或者类似OAuth等Token式的鉴权，进而抛弃session表示。

毕竟RESTful着重强调无状态。

