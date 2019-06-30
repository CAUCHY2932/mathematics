# 其他

这里包含了一些常用的工具和命令

## 文档工具

### mkdocs

#### 安装

> https://markdown-docs-zh.readthedocs.io/zh_CN/latest/user-guide/configuration/

```bash
# 安装mkdocs
pip install mkdocs
# mkdocs生成工程
mkdocs new doc_young
# mkdocs设置

# 切换文件夹
cd doc_young
# 修改配置文件mkdocs.yml
site_name: My Docs
theme: readthedoc
# 启动
mkdocs serve
```

#### 安装第三方主题

```bash
pip install material
theme: material
```

#### 后台启动并指定端口

```bash
nohup mkdocs serve --dev-addr 10.0.18.19:8000 &
nohup mkdocs serve --dev-addr 0.0.0.0:8000 & # 全网指定端口
mkdocs serve -a 0.0.0.0:8000
```

#### nginx

```bash
网站的结构配置文件编辑mkdocs.yml就好了，比如我的是site_name: Sigma blog
nav:
    - Home: index.md
    - Python:
        - Game: python_game.md
    - 收藏夹:
        - 代码: favorites.md
    - About: about.md这样就有次级目录了。修改文件保存后，网站就会实时加载修改，很方便。做好了后要发布，执行命令mkdocs build把site文件夹的东西拷到服务器就好了，至此大功告成。我服务器用的是nginx，原来已经有网站了，所以就做了nginx端口转发，之前用的uwsgi，有点转不过弯来，google了下其实转到静态文件所在的目录就好了。
location /blog/ {
        alias /home/xiaoming/xiaomingblog/site/;
	}当然，不用nginx也是可以的，随便用什么吧。要是无所谓的话，直接mkdocs serve -a 0.0.0.0:8000就ok，简单粗暴。反正性能安全神马的，那都是后话了。

作者：Sigma
链接：https://zhuanlan.zhihu.com/p/53025806
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

#### MkDocs material

目录

本文概述
mkdocs-material介绍
安装
初始化项目
修改主题
运行
发布到GitHub pages
发布到个人HTTP Server
mkdocs.yml注意事项
添加页面
添加扩展
markdown语法

mkdocs



pip install mkdocs mkdocs-material
若下载慢，可更换安装源为豆瓣

pip install --trusted-host pypi.douban.com -i http://pypi.douban.com/simple/ mkdocs mkdocs-material
初始化项目

mkdocs new my-project
会生成my-project目录，进入该目录里，可以看到默认放置了一些文件，包括mkdocs.yml，这是主配置文件

修改主题

mkdocs.yml里添加:

theme:
  name: material
运行

- 在my-project目录里执行

mkdocs serve
通过浏览器访问本地ip的8000端口（比如http://127.0.0.1:8000/） 查看效果，如图所示

image

发布到GitHub pages

通过mkdocs gh-deploy自动编译出html并发布至GitHub pages，步骤如下

初始化repo

1.在github上创建一个repo，名字叫my-project（可以是其他名，这里先假设叫my-project），创建repo时候选择初始化带有README.md
2.将repo同步到本地，使用git clone

导入项目

1.将mkdocs根目录（即my-project目录）下的所有东西移到刚刚git clone下来的git目录下
2.然后可以将最早创建的mkdocs根目录（即my-project目录）删除了

发布

在本地git目录下执行

mkdocs gh-deploy
发布到个人HTTP Server

通过mkdocs build编译出html并手动同步至http server的根目录

生成站点文件

在git目录下执行命令

mkdocs build
命令执行完毕后可以看到site目录

发布至http server

将site目录里的所有东西拷贝到http server的根目录下

mkdocs.yml注意事项

由于是yaml格式，因此首先要符合yaml的语法要求

docs下需要一个index.md，作为站点首页

文档层次结构虽然可以很多层，但最佳实践是控制在2层内，最多不要超过3层，否则展示会不够友好

添加页面

在my-project/docs/里放置.md文件，可以自行组织目录结构

然后在mkdocs.yml里添加，比如这样:

```yml
nav:
  - 介绍: index.md
  - 安装:
      - 本地环境搭建: install/local.md
      - 发布至GitHub Pages: install/github-pages.md
      - 发布至自己的HTTP Server: install/http-server.md
  - 语法:
      - 语法总览: syntax/main.md
      - 标题: syntax/headline.md
      - 段落: syntax/paragraph.md
```

上面的index.md就是放置在my-project/docs/index.md
上面的local.md就是放置在my-project/docs/install/local.md
添加扩展

只有添加了扩展，才能完美使用mkdocs-material官方支持的所有markdown语法

mkdocs.yml里添加:

```bash
markdown_extensions:
  - admonition
  - codehilite:
      guess_lang: false
      linenums: false
  - toc:
      permalink: true
  - footnotes
  - meta
  - def_list
  - pymdownx.arithmatex
  - pymdownx.betterem:
      smart_enable: all
  - pymdownx.caret
  - pymdownx.critic
  - pymdownx.details
  - pymdownx.emoji:
      emoji_generator: !!python/name:pymdownx.emoji.to_png
  - pymdownx.inlinehilite
  - pymdownx.magiclink
  - pymdownx.mark
  - pymdownx.smartsymbols
  - pymdownx.superfences
  - pymdownx.tasklist
  - pymdownx.tilde

```

页面以及跳转文字的配色
中文搜索
最佳实践

如果希望自己所写的markdown可以兼容各个markdown编辑器，那么只需了解markdown的传统语法即可

如果想让自己所写的markdown发布到web服务器，例如GitHub Pages、自己搭建的HTTP Server，那么可以考虑使用本文所介绍的语法，以实现丰富多样的渲染效果。

笔者建议：尽量使用传统语法，只在必要时候才使用本文介绍的语法。因为排版简洁、条理清晰才能带来最舒服的阅读感受。

https://blog.51cto.com/cyent/2351243?source=dra

- 支持中文搜索

https://cyent.github.io/markdown-with-mkdocs-material/

### sphinx



### rst



### markdown







## 编程技巧

