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

进入工程

```bash
# 清除生成缓存
make clean 
# 重新生成
make html
```





https://www.jianshu.com/p/728aac51cc53

### rst语法

[http://rst.ninjs.org/](http://rst.ninjs.org/)

https://www.notex.ch/editor



https://pythonhosted.org/an_example_pypi_project/sphinx.html

https://zh-sphinx-doc.readthedocs.io/en/latest/contents.html

https://www.xncoding.com/2017/01/22/fullstack/readthedoc.html



### 正式篇

https://cloudconvert.com/md-to-rst







### markdown







## 编程技巧





## 理解开发流程

![图片](https://dn-coding-net-production-pp.codehub.cn/2778bdc2-9827-4f10-8abd-da4d71c3ab4e.png)

腾讯云开发者平台的开发流程是基于 Git 分支的。通过 Git 分支，多个开发者可以同时进行各自的任务开发，互不干扰。等开发完成再通过我们的合并请求（MR）及其评审机制，将代码合并至主线，不断迭代。

通过下面的文字，我们一起了解一下这个流程是怎样的，以及为什么要这么做。

### 代码仓库

我们的每个项目有一个代码仓库，即远程仓库，每个开发者都有一个自己的本地仓库。所有的本地仓库通过推送和拉取代码的方式保持和远程仓库同步。在最开始我们需要初始化远程仓库，有以下三种方式：

- 创建项目时初始化仓库
- 在本地创建一个新仓库并推送至远程仓库
- 从本地推送已有仓库至远程仓库

具体可查看[帮助文档](https://dev.tencent.com/help/git-base),其他更多关于 Git 的使用问题可以查看 [Git 文档](https://git-scm.com/book/zh/v2)。

### 创建分支

每个代码仓库都有一个默认分支，但接到新任务时我们一般不在这个默认分支上改动，而是新建一个分支。这样做的好处是，当有新的任务中断当前任务时，我们能够随时切换，从而保证每条任务线的代码都是互不干扰的。

### 代码提交

进入新分支之后，我们就可以进行开发了。一般我们会在完成一个小功能点时进行代码跟踪 `git add` 和提交 `git commit` ，如果你想在提交代码时关联某个任务，可以 commit 信息中写上 `#id` （任务引用 ID ）：

```shell
  git commit -m "关联任务 #100"
```

### 合并请求

当我们在新分支上完成任务时，就可以创建一个合并请求（MR），请求合并到默认分支。与此同时，我们可以邀请其他成员来进行代码评审。这里有一个技巧，当你按如下格式提交分支至线上时，会自动创建 MR 。

```shell
  git push 主机名 本地分支名:mr/线上目标分支名/本地分支名
```

在代码评审时，如果对代码有疑问可以直接评论。

![图片](https://dn-coding-net-production-pp.codehub.cn/bd3fc453-47d4-4784-9603-514a27a6b17a.png)

如果评审者觉得没有问题，就可以点击“允许合并”（对这个 MR 进行 +1 ）。等所有评审者都允许合并之后，MR 发起者就可以合并分支了。

![图片](https://dn-coding-net-production-pp.codehub.cn/908adeb8-9126-45d8-84e9-a150f7641af7.png)

### 版本发布

当完成一个里程碑的所有任务开发之后，我们可以在某个节点将此时的默认分支标记为一个版本，并发布这个版本。

![图片](https://dn-coding-net-production-pp.codehub.cn/cf537bab-0a88-4960-9e91-8afdfc41e4fa.png)

### 体验一下整个流程

为了让你更深刻地理解这个流程，假设你现在接到了一个新的任务。你创建了一个新分支 `new-feature` ，准备在这个分支进行开发。

点击[这里](/u/dtid_9e65f14616561408/p/dev-demo/git/branches)前往查看 `new feature` 分支。

## gitbook

本地安装gitbook需要nodejs，目前安装gitbook最方便的方式就是通过npm安装

```bash
sudo npm install -g gitbook-cli
sudo npm install -g gitbook
```

安装完之后，你可以检验下是否安装成功。

```bash
gitbook -V
```

安装好 Gitbook 之后，我们就可以创建图书了。

Gitbook 的基本用法非常简单，基本上就只有两步:

1. 使用 gitbook init 初始化书籍目录
2. 使用 gitbook serve 编译书籍

首先，进入一个目录，例如之前我们创建好的 gitbook，执行初始化命令：



首先，进入一个目录，例如之前我们创建好的 gitbook，执行初始化命令：

```text
sudo gitbook init
```

然后我们的 gitbook 空目录会多出两个文件：

```text
gitbook/
├── README.md
└── SUMMARY.md
```

README.md 和 SUMMARY.md 是两个必须文件，README.md 是对书籍的简单介绍。SUMMARY.md 是书籍的目录结构。

使用下面的命令来启动

```bash
gitbook serve
```

使用下面的命令来构建

```bash
gitbook build
```

