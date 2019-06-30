# python-web

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



## flask（狗书）

## flask（董伟明）

## django(胡阳)
