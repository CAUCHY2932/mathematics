## json格式化

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
