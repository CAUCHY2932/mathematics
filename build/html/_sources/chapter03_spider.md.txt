# 爬虫

## 第一次爬虫

数据库
SQL NoSQL MongoDB MySQL Redis
课程推荐

Linux运维
Linux Shell 网络 Nginx Ansible Git
### 爬虫示例

```python
#coding:utf-8
import requests
import urllib
from bs4 import BeautifulSoup
from lxml import etree
import json

url='http://auth.alipay.com/login/index.htm'
response=requests.get(url)

rr = response.text

html=BeautifulSoup(rr,'lxml')

#创建css选择器
items=html.select('script[type="text/javascript"]')
for item in items:
    if "https://qr.alipay.com" in item.text:
        print(item)

# print(items)

# s = etree.HTML(rr)

# print(rr)

# if 'https://qr.alipay.com' in rr:

# print('find')

# dictobj=s.xpath('//*[@id="J-barcode-container"]/canvas')

#

# with open("news.json", "a+", encoding='utf-8') as f:

# f.write(json.dumps(dictobj, ensure_ascii=False) + '\n')


```

