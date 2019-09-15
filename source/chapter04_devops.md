# 环境

包含linux和一些常用的命令


## encoding

unzip 解压含有中文的目录时乱码
可以采用如下解决方案
unzip -O CP936 xxx.zip

### 在windows上CSV文件可读，但是在linux下编码会出现错误

我们可以采取更改编码的方式进行交换

## ubuntu

### 用户总结

使用ubuntu时，默认是没有root的，

需要sudo passwd root，之后进行密码设置

su root

切换用户即可

### 基本操作

```bash
# 1.端口操作
sudo ufw status  # 查看端口开启情况
sudo ufw allow 80  # 打开80端口
sudo ufw enable  # 防火墙开启开机自启
sudo ufw reload  # 防火墙重启
# 2.重置密码
sudo passwd
# 3.软件安装
# 3.1.chrome
sudo wget http://www.linuxidc.com/files/repo/google-chrome.list -P /etc/apt/sources.list.d/
wget -q -O - https://dl.google.com/linux/linux_signing_key.pub  | sudo apt-key add
sudo apt update
sudo apt install google-chrome-stable
# 1.1 安装命令（ppa源）
sudo add-apt-repository ppa:jonathonf/vim
sudo apt update
sudo apt install vim
# 1.2 卸载命令
sudo apt remove vim
sudo add-apt-repository --remove ppa:jonathonf/vim
```

### python3起别名

在~/.bashrc中添加一个别名即可python实现python3

```bash
sudo vim ~/.bashrc

alias python="python3"
alias pip="pip3"
source ~/.bashrc
```

### 中科大源

ubuntu 18.04

```bash 
deb https://mirrors.ustc.edu.cn/ubuntu/ cosmic main restricted universe multiverse 


deb-src https://mirrors.ustc.edu.cn/ubuntu/ cosmic main restricted universe multiverse 
deb https://mirrors.ustc.edu.cn/ubuntu/ cosmic-updates main restricted universe multiverse 
deb-src https://mirrors.ustc.edu.cn/ubuntu/ cosmic-updates main restricted universe multiverse 
deb https://mirrors.ustc.edu.cn/ubuntu/ cosmic-backports main restricted universe multiverse 
deb-src https://mirrors.ustc.edu.cn/ubuntu/ cosmic-backports main restricted universe multiverse 
deb https://mirrors.ustc.edu.cn/ubuntu/ cosmic-security main restricted universe multiverse 
deb-src https://mirrors.ustc.edu.cn/ubuntu/ cosmic-security main restricted universe multiverse 
deb https://mirrors.ustc.edu.cn/ubuntu/ cosmic-proposed main restricted universe multiverse 
deb-src https://mirrors.ustc.edu.cn/ubuntu/ cosmic-proposed main restricted universe multiverse
```



更换之后，更新源

```bash
sudo apt-get update
```

### 安装mysql

> https://blog.csdn.net/weixx3/article/details/80782479

```bash
sudo apt install mysql-client-core-5.7 
sudo apt install mysql-server
sudo mysql_secure_installation
密码1234
sudo mysql -uroot -p # 无需密码正确

GRANT ALL PRIVILEGES ON *.* TO root@localhost IDENTIFIED BY "123456";
```

### vim配置

```bash
git clone https://git.oschina.net/eccozhou/vimrc.git ~/.vim_runtime
sh ~/.vim_runtime/install_awesome_vimrc.sh
```

### 安装docker

```bash
# 配置镜像站
curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://f1361db2.m.daocloud.io
sudo apt-get install docker.io
sudo docker search superset
sudo docker pull amancevice/superset
sudo docker image ls
mkdir -p /opt/docker/superset
docker run -d -p 8087:8088 -v /opt/docker/superset:/home/superset amancevice/superset
sudo docker container ls
docker exec -it bi superset-init # bi替换为id或是容器名，初始化superset
docker exec -it bi superset load_examples # 载入示例数据（可选）
```

### 安装ssh

```bash
sudo apt install openssh-server

```

### 安装jdk

```bash
sudo apt install openjdk-8-jdk-headless
```





### 更改root密码

```bash
sudo passwd root
```

### sudo命令执行慢

解决 Ubuntu 下 sudo 命令执行速度慢的问题
1、首先如果当用登录的用户名不在"/etc/sudoers"文件中，是不能执行sudo命令的。可以用root身份手动修该文件，把当前登录用户名加入该文件中。
2、用"hostname "命令查看当前主机的主机名称。例如，该命令返回"yzh ".
3、用vi打开"/etc/hosts"文件，并将"ubuntu"加入到 "127.0.0.1"这行中。
例如：
127.0.0.1       localhost      ubuntu
这个问题是最近装Ubuntu Server 18.04 LTS时遇到的，之前用Ubuntu Server 16.04 LTS并没有发现这个问题.

症状：sudo速度非常慢，提交命令之后大概需要10秒左右才有输入sudo密码或者开始运行。su命令症状相同。

原因：Ubuntu Server被设计成一种类似于分布式的操作系统网结构，允许/etc/sudoers中的成员不在本机上。从而sudo时会先从网络上寻找可能的sudoer然后才是本地. 而这10s左右的时间就是整个DNS流程的最长时间.

解决方案：首先输入hostname，得到本机当前的互联网名称（大概跟windows下的计算机名称差不多）。然后使用su或sudo打开/etc/hosts，添加一行：

127.0.0.1<TAB>计算机名<TAB>计算机名.localdomain



## vim 

### 常用设置

```bash
## edit the vimrc file

$ vim ~/.vimrc
## set the file

set ts=4
set expandtab
set autoindent
set number

# 另外，Python编程是靠缩进来规定语法的，当你使用vim写python时，要注意tab与空格的区别。一般我们写Python都是以4个空格表缩进标准的，所以在代码中不要把空格与tab混用（两者ASCII码是不同的），要不一直用空格，要不就一直用tab，不然会导致程序报错。推荐把vim的tab变为4个空格，增加编程效率。

# 设置Tab键的宽度[等同的空格个数]
set tabstop=4
# 每一次缩进对应的空格数
set shiftwidth=4
# 按退格键时可以一次删掉4个空格
set softtabstop=4

```

### 常用操作



#### 复制



#### 粘贴









#### 删除





#### vim行首和行尾添加字符串

每行的行首都添加一个字符串：%s/^/要插入的字符串 
每行的行尾都添加一个字符串：%s/$/要插入的字符串

解释： 
% 代表针对被编辑文件的每一行进行后续操作 
$ 代表一行的结尾处 
^ 代表一行的开头处

在全部内容的行首添加//号注释

:% s/^/\/\//g

在2~50行首添加//号注释

:2,50 s/^/\/\//g

在2~50行首删除//号

:2,50 s/^\/\///g



```bash
# 在root用户家目录下的.vimrc中设置，对所有用户生效
# 如何与外界剪贴板进行交互

# 在按下esc后
"+y 复制到系统寄存器
"+p 粘贴到vim

# 查看当前寄存器的内容

:reg

# 安装vim

rpm -qa|grep vim
yum -y install vim*

# 复制

n+yy
复制n行
块选择模式，选中然后y复制

# 粘贴

# 删除

n+dd
# 删除连当前行的n行
# 可视化选择模式，选中然后按d删除

# 插入

i
从当前插入
A
从当前行插入

# 搜索

# 保存和退出

# 撤销


# vi/vim 中如何在每行行首或行尾插入指定字符串
行首 :%s/^/your_word/
行尾 :%s/$/your_word/

按键操作：

注释：ctrl+v 进入列编辑模式,向下或向上移动光标,把需要注释的行的开头标记起来,然后按大写的I,再插入注释符,比如”#”,再按Esc,就会全部注释了。

删除：ctrl+v 进入列编辑模式,向下或向上移动光标,选中注释部分,然后按d, 就会删除注释符号（#）。

PS：当然不一定是shell的注释符”#”，也可以是”//”，或者其他任意的字符；vim才不知道什么是注释符呢，都是字符而已。

使用替换命令：

在全部内容的行首添加//号注释

:% s/^/\/\//g

在2~50行首添加//号注释

:2,50 s/^/\/\//g

在2~50行首删除//号

:2,50 s/^\/\///g


### 复制

n+yy
复制n行
块选择模式，选中然后y复制

### 粘贴

###  删除

n+dd
删除连当前行的n行
可视化选择模式，选中然后按d删除

### 插入

i
从当前插入
A
从当前行插入

### 搜索

### 保存和退出

### 撤销

### 常用配置


" 设定默认解码 
set fenc=utf-8 

set fencs=utf-8,usc-bom,euc-jp,gb18030,gbk,gb2312,cp936 
" 不要使用vi的键盘模式，而是vim自己的 
set nocompatible 

" history文件中需要记录的行数 
set history=100 

" 在处理未保存或只读文件的时候，弹出确认 
set confirm 

" 与windows共享剪贴板 
set clipboard+=unnamed 

" 侦测文件类型 
filetype on 

" 智能补全
set completeopt=longest,menu

" 载入文件类型插件 
filetype plugin on 

" 为特定文件类型载入相关缩进文件 
filetype indent on 

" 保存全局变量 
set viminfo+=! 

" 带有如下符号的单词不要被换行分割 
set iskeyword+=_,$,@,%,#,- 

" 语法高亮 
syntax enable
syntax on 

" 高亮字符，让其不受100列限制 
:highlight OverLength ctermbg=red ctermfg=white guibg=red guifg=white 
:match OverLength '\%101v.*' 

" 状态行颜色 
highlight StatusLine guifg=SlateBlue guibg=Yellow 
highlight StatusLineNC guifg=Gray guibg=White 

""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""" 
" 文件设置 
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""" 
" 不要备份文件（根据自己需要取舍） 
set nobackup 

" 不要生成swap文件，当buffer被丢弃的时候隐藏它 
setlocal noswapfile 
set bufhidden=hide 

" 字符间插入的像素行数目 
set linespace=0 

" 增强模式中的命令行自动完成操作 
set wildmenu 

" 在状态行上显示光标所在位置的行号和列号 
set ruler 
set rulerformat=%20(%2*%<%f%=\ %m%r\ %3l\ %c\ %p%%%) 

" 命令行（在状态行下）的高度，默认为1，这里是2 
set cmdheight=2 

" 使回格键（backspace）正常处理indent, eol, start等 
set backspace=2 

" 允许backspace和光标键跨越行边界 
set whichwrap+=<,>,h,l 

" 可以在buffer的任何地方使用鼠标（类似office中在工作区双击鼠标定位） 
set mouse=a 
set selection=exclusive 
set selectmode=mouse,key 

" 启动的时候不显示那个援助索马里儿童的提示 
set shortmess=atI 

" 通过使用: commands命令，告诉我们文件的哪一行被改变过 
set report=0 

" 不让vim发出讨厌的滴滴声 
set noerrorbells 

" 在被分割的窗口间显示空白，便于阅读 
set fillchars=vert:\ ,stl:\ ,stlnc:\ 

""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""" 
" 搜索和匹配 
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""" 
" 高亮显示匹配的括号 
set showmatch 

" 匹配括号高亮的时间（单位是十分之一秒） 
set matchtime=5 

" 在搜索的时候忽略大小写 
set ignorecase 

" 不要高亮被搜索的句子（phrases） 
set nohlsearch 

" 在搜索时，输入的词句的逐字符高亮（类似firefox的搜索） 
set incsearch 

" 输入:set list命令是应该显示些啥？ 
set listchars=tab:\|\ ,trail:.,extends:>,precedes:<,eol:$ 

" 光标移动到buffer的顶部和底部时保持3行距离 
set scrolloff=3 

" 不要闪烁 
set novisualbell 

" 我的状态行显示的内容（包括文件类型和解码） 
set statusline=%F%m%r%h%w\[POS=%l,%v][%p%%]\%{strftime(\"%d/%m/%y\ -\ %H:%M\")} 

" 总是显示状态行 
set laststatus=2 

""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""" 
" 文本格式和排版 
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""" 
" 自动格式化 
set formatoptions=tcrqn 

" 继承前一行的缩进方式，特别适用于多行注释 
set autoindent 

" 为C程序提供自动缩进 
set smartindent 

" 使用C样式的缩进 
set cindent 

" 制表符为4 
set tabstop=4 

" 统一缩进为4 
set softtabstop=4 
set shiftwidth=4 

" 不要用空格代替制表符 
set noexpandtab 

" 不要换行 
set nowrap 

" 在行和段开始处使用制表符 
set smarttab 

""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""" 
" CTags的设定 
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""" 
" 按照名称排序 
let Tlist_Sort_Type = "name" 

" 在右侧显示窗口 
let Tlist_Use_Right_Window = 1 

" 压缩方式 
let Tlist_Compart_Format = 1 

" 如果只有一个buffer，kill窗口也kill掉buffer 
let Tlist_Exist_OnlyWindow = 1 

" 不要关闭其他文件的tags 
let Tlist_File_Fold_Auto_Close = 0 

" 不要显示折叠树 
let Tlist_Enable_Fold_Column = 0 



### 常用配置2


在.vimrc中添加以下代码后，重启vim即可实现按TAB产生4个空格：
set ts=4  (注：ts是tabstop的缩写，设TAB宽4个空格)
set expandtab

对于已保存的文件，可以使用下面的方法进行空格和TAB的替换：
TAB替换为空格：
:set ts=4
:set expandtab
:%retab!

空格替换为TAB：
:set ts=4
:set noexpandtab
:%retab!

加!是用于处理非空白字符之后的TAB，即所有的TAB，若不加!，则只处理行首的TAB。
```

## git

### 常用操作
```bash

### 1.git用户配置
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com
### 2.ssh-keygen
$ cat ~/.ssh/id_rsa.pub
### 3.copy to github
### 4.git clone
git clone github.com...
或者
### 5.git init

mkdir mygit
cd mygit
git init
git remote add origin github.com..

### 6.git跟踪操作

$ git add -A

### 7.git提交操作

$ git commit -m 'descriptions'

### 8.git推送操作

$ git push

### 9.git push usage

- 为了提高克隆效率，我们统一采用新的仓库方式进行代码托管
  -- 首先在github生成仓库，并克隆到本地，再推送到云端
  -- 其次，在gitee中同步github，在另一台设备进行克隆
  --两台机器都使用git remote add origin/mirror xxx进行代码远程分支的管理

git remote add origin gitee.com...
git remote add mirror github.com...
git remote -v
git push origin master/dev
git push mirror master/dev

## 经常使用的组合

### 1.配置好本地和github、gitee的秘钥

### 2.在github新建一个仓库，初始化，并在gitee导入该仓库

### 3.再进行如下操作

#### git clone git@giteexxx

使用gitee克隆，速度较快

#### git remote add mirror git@githubxxx
添加远程分支

#### git push origin master

推送到远程分支,gitee

#### git push mirror master

推送到远程分支，github

### git安装后初始化

$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"

一次提交多个文件

git status 
git add -A
git commit -a -m"first commit"

提交到版本库

git push

git add -A  提交所有变化
git add -u  提交被修改(modified)和被删除(deleted)文件，不包括新文件(new)
git add .  提交新文件(new)和被修改(modified)文件，不包括被删除(deleted)文件

ssh-keygen -t rsa -C "youremail@example.com"
提交到远程

git push origin master
```

### 加速

最近Superset更新的很频繁啊，今天升级成了0.28.1版本，新踩的坑也跟大家分享一下

1.发现pip安装的assets文件目录下居然没有src文件夹了，这个真是惊呆了我搞了好久，后来问了大神才知道要去github上面单独下载...

github地址：[apache/incubator-superset](https://link.zhihu.com/?target=https%3A//github.com/apache/incubator-superset/tree/0.28/superset/assets/src)

快速下载方式：[http://kinolien.github.io/gitzip/](https://link.zhihu.com/?target=http%3A//kinolien.github.io/gitzip/)

2.前端部分的文件夹目录算是大改了，后端的viz.py没变，这里不赘述

2.1、superset\static\assets\src\visualizations\index.js

我这个版本文件位置没改，但是内容改了，不过这个本来也比较简单，还是照抄其他表的配置就行了

## python

### 永久修改镜像源

```bash
cd  ~
mkdir .pip
cd .pip && vi pip.conf
```

添加内容

```bash
[global]
index-url = http://pypi.douban.com/simple
[install]
trusted-host=pypi.douban.com
```

### 服务器部署常用镜像源

- 阿里巴巴 https://opsx.alibaba.com/mirror 
- 清华 https://mirrors.tuna.tsinghua.edu.cn/ 
- 中科大 https://mirrors.ustc.edu.cn/
- 豆瓣 http://pypi.doubanio.com/simple/


### python运维相关内容
```bash
# conda使用
conda create -n py_env python=python_version # conda创建环境
conda remove -n py_env --all # conda删除环境
conda install # conda安装第三方包
conda env list # conda查看当前的虚拟环境
conda list  # conda查看安装的包
# conda切换清华源
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --set show_channel_urls yes

source activate py_env # win下激活conda
source deactivate

conda activate py_env # mac下激活conda
conda deactivate

pip freeze > requirement.txt # 冻结包
pip install -r requirement.txt # 可以安装所有指定的包
```

#### conda配置数据分析环境

```bash
conda create -n data python=3.6.8
conda install numpy=1.16.2
conda install pandas=0.24.2
conda install matplotlib=3.0.3
conda install scikit-learn=0.20.3
conda install scikit-surprise=1.0.6
conda install seaborn=0.9.0
conda install scipy=1.2.1
conda install jupyter=1.0.0
pip install -i https://pypi.doubanio.com/simple dash==0.39.0
pip install -i https://pypi.doubanio.com/simple dash-daq==0.1.0
pip install -i https://pypi.doubanio.com/simple plotly_express==0.1.3
pip install -i https://pypi.doubanio.com/simple pymysql==0.9.3
```
#### python爬虫环境配置

```bash
conda install scrapy=1.5.2
conda install requests=2.21.0
pip install -i https://pypi.doubanio.com/simple pillow==6.0.0
pip install -i https://pypi.doubanio.com/simple PyExecJS=1.5.1
pip install -i https://pypi.doubanio.com/simple wget==3.2
pip install -i https://pypi.doubanio.com/simple BeautifulSoup4==4.7.1
conda install scrapy
conda install requests
conda install bs4
```

#### web开发
```bash
conda install flask
conda install django
conda install pymysql
conda install pymongo
```

#### gui开发
```bash
## python gui开发

pycharm和pyqt5

> https://blog.csdn.net/zhangziju/article/details/80243858

## 环境配置

pip install PyQt5 -i https://pypi.douban.com/simple
pip install PyQt5-tools -i https://pypi.douban.com/simple

# 在pycharm，然后在设置里面点击external tools，点击“+”，需要添加Qt Designer 和pyuic 两个选项。

### Qt Designer

Name：可自己定义
program：Qt Designer的安装路径
parameter：不填
directory： $FileDir$

### pyuic

Name：可自己定义
program：pyuic的安装路径
parameter：$FileName$ -o $FileNameWithoutExtension$.py

directory： $FileDir $

## 教程

完成之后可以在pycharm中的外部工具打开qtdesigner
生成的ui文件必须是在工程文件的根目录中，然后使用pyuic生成Python文件
```
### mac配置pip
```bash


## python mac配置pip

mkdir ~/.pip

vim ~/.pip/pip.conf
[global]
index-url=http://mirrors.aliyun.com/pypi/simple/
[install]
trusted-host=mirrors.aliyun.com

```

### python pylint

python安装目录下scripts/pylint.exe

arguments: --output-format=parseable --disable=R -rn --msg-template="{abspath}:{line}: [{msg_id}({symbol}), {obj}] {msg}" $FilePath$

- working firectory: $FileDir$



## mac mongodb安装

```bash
	- 使用homebrew
  - brew       install mongodb
  - 安装之后的验证，输入mongod，即可启动服务，输入mongo进入到mongobd到命令行的界面
  - brew       services start mongodb
  - mongo
```

## mac redis安装

```bash
	- brew install redis
  - redis-cli
  - set       'a' 'b'
  - get 'a'
  - 如果出错，就在终端里进行修改cd       /usr/local/etc/
  -  
  - command+shift+g，然后cd       /usr/local/etc，之后再使用文本编辑器打开redis.conf
  - redis-server       ./redis.conf
  - redis-cli
  - brew       services restart redis
  - 可以在配置文件里添加密码
```


## linux命令

### 常用命令

> http://www.magedu.com/74163.html?linux_wenzhang_zhihu_jinke_tiaocaobibei40ti_33967414

```bash
# linux命令-用户、权限管理
useradd xxx # 增加一个用户
userdel -rf xxx # 删除一个用户
whoami ## 查看当前用户
who ## 查看当前登录用户
exit ## 退出登录账户

## 添加用户账号
useradd [参数] 新建用户账号
-d #指定用户登录系统时的主目录，如果不使用该参数，系统自动在/home目录下建立与用户名同名目录为主目录
-m #自动建立目录
-g #指定组名称
#linux每个用户都要有一个主目录，主目录就是第一次登录系统，用户的默认当前目录（/home/用户）
#每一个用户都必须有一个主目录，所以用useradd创建用户的时候，一定给用户指定一个主目录
#用户的主目录一般要放到根目录的home目录下，用户的主目录和用户名是相同的
# 如果创建用户的时候，不指定组名，那么系统会自动创建一个和用户名一样的组名
useradd -d /home/abc abc -m #创建一个abc用户，如果目录不存在，就自动创建这个目录，同时用户属于abc组
useradd -d /home/a a -g test -m #创建一个用户名字叫a，主目录在/home/a，如果主目录不存在，就自动创建主目录，同时用户属于test组
cat /etc/passwd #查看系统当前用户名

passwd # 设置用户密码
# 超级用户可以使用passwd命令为普通用户设置或修改用户口令，用户也可以直接使用该命令来修改自己的口令，而无需再命令后面使用用户名
sudo passwd user1

userdel # 删除用户
userdel abc #删除abc用户，但不会自动删除用户的主目录
userdel -r abc #删除用户，同时删除用户的主目录

# 切换用户
su # su后面可以加“-”，加与不加的区别在于，su-会切换到对应的用户时，会将当前的工作目录自动转换到切换后的用户主目录
# 有些平台需要在命令前加sudo，这是因为某些操作需要管理员才能操作，
sudo su - root #切换到超级用户
su - 普通用户 # 切换到普通用户

# 查看有哪些用户组
cat /etc/group
或者
groupmod + 三次tab键

# 添加、删除组账号
groupadd groupdel
groupadd 新建组账号
groupdel 删除组账号

# 修改用户所在组
usermod
```

### 查看硬盘

```bash
# 查看硬盘的信息，并以人性化的方式建立可读性
df -h 
```

### 系统管理

```bash
## linux命令-系统管理
# 查询端口
netstat -tulpn ### 查询端口
lsof -i:9090

# 查询进程
ps -ef | grep xxx # 查询进程
ps aux | grep xxx

```

### 文件和目录操作

```bash
# 文件与目录操作
cp -r sourcePath targetPath # 复制文件
rm -rf xxx # 删除文件
mv -r sourcePath targetPath ### 移动文件
mv sourcePath targetPath ### 重命名文件

ls -lah # 列举所有文件，以人性化的方式
ls -l # 列举文件，以列表形式
cd # 切换工作目录
pwd # 显示当前路径

mkdir # 创建目录
rm -rf # 删除目录或文件 -f强制删除 -r 递归删除
cp # -r 递归拷贝 -v 显示进度 -f 强制性操作
mv # 移动文件 -f 强制性操作 -v 显示移动进度

# 建立链接文件
ln 源文件 链接文件
ln -s 源文件 链接文件 # 不加 -s 表示建立一个硬链接文件

# 压缩、解压
tar -zcvf target.tar.gz xxx ### 打包、压缩
tar -zxvf target.tar.gz ### 解压tar.gz
unzip xxx.zip ### 解压zip

### 上传和拷贝

从远处复制文件到本地目录
scp root@10.10.10.10:/opt/soft/nginx-0.5.38.tar.gz /opt/soft/
从远处复制目录到本地（将mongodb的目录复制到本地）
scp -r root@10.10.10.10:/opt/soft/mongodb /opt/soft/
上传本地文件到远程机器指定目录
scp /opt/soft/nginx-0.5.38.tar.gz root@10.10.10.10:/opt/soft/scptest
上传本地目录到远程机器指定目录
scp -r /opt/soft/mongodb root@10.10.10.10:/opt/soft/scptest
```

### 文本操作

```bash
### 重定向命令

ls > test.txt # 如果不存在，则创建，存在则覆盖其内容
ls >> 001.txt # 如果不存在，则创建，存在则追加
管道
ls -lh | more

clear # 清屏

# 查看或者合并文件内容
cat xxx.file
cat test1.file test2.file > hebing.txt
cat test1.file test2.file >> hebing.txt

# 文本搜索
grep [-选项] '搜索内容串' 文件名
-v 显示不包含匹配文件的所有行，求反
-n 显示匹配行及行号
-i 忽略大小写
搜索内容串可以是正则表达式

# 查找文件

find
find ./ -name test.sh 查找当前目录下所有名为test.sh的文件
find ./ -size 2M 查找在当前目录下等于2M的文件
find ./ -size +2M 查找在当前目录下大于2M的文件
find ./ -size -2M 查找在当前目录下小于2M的文件
find ./ -size +4k -size -5M 查找在当前目录下大于4K，小于5M的文件

```

### 压缩解压

```bash
# 归档管理

tar
-c 生成档案文件，创建打包文件
-v 列出详细过程，显示进度
-f 指定档案文件名称，f后面一定是tar文件， 所以必须放在选项最后
-t 列出档案中包含的文件
-x 解开档案文件
tar -cvf test.tar *
tar -xvf test.tar

# 文档压缩解压

gzip [选项] 被压缩文件

-d 解压
-r 压缩所有子目录

tar只进行打包，不进行压缩
但是在tar的命令中增加一个压缩的功能，实行一个先打包后压缩的过程
如 tar -zcvf xx.tar.gz *
解压为
tar -zxvf file.tar.gz
解压到指定目录
-C 大写
tar -zxvf test.tar.gz -C xxx/

# 文件压缩解压 bzip2

压缩用法
tar -jcvf xx.tar.bz2 *
解压用法
tar -jxvf xx.tar.bz2

# 文件压缩解压 zip unzip

zip压缩的目标文件不要指定扩展名，默认扩展名为zip
压缩文件
zip [-r] 目标文件（没有扩展名） 源文件
解压文件
unzip -d 解压后目录文件 压缩文件

# 查看命令位置 which
which ls
```

### 查看当前日历

cal
-y 显示整年日历

### 显示或设置时间

date
date [MMDDhhmm[[CC]YY[.ss]]+format
CC为年前两位yy为年的后两位
前两位的mm为月，后两位的mm为分钟，dd为天，hh为小时，ss为秒。如： date 010203042016.55
显示时间格式
（date '+%y,%m,%d,%H,%M,%S'）

### 查看进程信息

ps process status
-a 显示终端上的所有进程，包括其他用户的进程
-u 显示进程的详细状态
-x 显示没有控制终端的进程
-w 显示加宽，以便显示更多的信息
-r 只显示正在运行的进程
常用的为
ps -aux



### 动态显示进程

top 加上-d可以指定显示信息更新的时间间隔
执行top命令之后，可以按下按键得到对显示结果进行排序
M 根据内存使用量来排序

### 终止进程

kill
kill [-signal] pid

kill -9 pid

### 关机重启

reboot 重新启动操作系统
shutdown -r now 重新启动操作系统，shutdown会给别的用户提示
shutdown -h now 立刻关机，其中now相当于时间为0的状态
shutdown -h 20:25
init

### scp或pscp传输文件

```powershell
# 在windows下解压pscp，执行以下命令
.\pscp.exe root@129.28.189.50://root/git_repos/superset.tar.gz ./
./pscp .\finebi-20190415-product-full-bi51.tar.gz root@192.168.144.128:/root/
```

### 远程命令

```bash
ssh -p 4645 root@192.168.52.12
```

### 查看有多少行文件

```bash
ls | nl
```

## centos

### 1.centos7安装python3以及jupyter

```bash
# 1.1centos7安装python3
# https://docs.conda.io/en/latest/miniconda.html
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/Miniconda3-latest-Linux-x86_64.sh
sudo sh ./Miniconda3....
conda create -n jupyter
conda activate jupyter
```



### 1.2永久设置pip镜像源

```bash
# 1.2.1 直接使用pip安装
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple/ xxx

numpy pandas 

#### 1.2.2 设置永久镜像源

cat > ~/.pip/pip.conf

#文件内容如下：
[global]
index-url = http://mirrors.aliyun.com/pypi/simple/
[install]
trusted-host = mirrors.aliyun.com
```

### 安装jupyter lab

```bash
pip install jupyterlab
Consider using the `--user` option or check the permissions
pip install --user jupyterlab
jupyter notebook --generate-config

打开ipython
ipython
from notebook.auth import passwd
passwd()
Enter password: 123456
Verify password: 123456
‘sha1:e00ee9ab9a42:22e8c0dc771612348eeee698cde8ec77fba42e7f’
exit()

sha1:1ee6f09f898e:e352babd3864f2221d538de2b001d4db95da0e2a

把生成的密文‘sha:xx…’复制下来
修改默认配置文件 vi ~/.jupyter/jupyter_notebook_config.py

c.NotebookApp.ip='*'
c.NotebookApp.allow_root = True
c.NotebookApp.password = u'sha1:e00ee9ab9a42:22e8c0dc771612348eeee698cde8ec77fba42e7f'
c.NotebookApp.open_browser = False
c.NotebookApp.port =8888
c.ContentsManager.root_dir = '/data/jupyter/root'

c.NotebookApp.allow_remote_access = True


# 启动jupyter notebook：

nohup jupyter lab --allow-root &

登录jupyter lab

http://服务器ip地址:8888/lab

10.10.10.100:8888/lab
```

### 在centos7上安装mongodb
```
### 添加源

> vim /etc/yum.repos.d/MongoDB.repo

[mongodb-org-3.6]

name=MongoDB Repository

baseurl=https://repo.mongodb.org/yum/redhat/\$releasever/mongodb-org/3.6/x86_64/

gpgcheck=1

enabled=1

gpgkey=https://www.mongodb.org/static/pgp/server-3.6.asc

> yum -y install mongodb-org

---> Package mongodb-org.x86_64 0:3.6.11-1.el7 will be installed
--> Processing Dependency: mongodb-org-tools = 3.6.11 for package: mongodb-org-3.6.11-1.el7.x86_64
--> Processing Dependency: mongodb-org-shell = 3.6.11 for package: mongodb-org-3.6.11-1.el7.x86_64
--> Processing Dependency: mongodb-org-server = 3.6.11 for package: mongodb-org-3.6.11-1.el7.x86_64
--> Processing Dependency: mongodb-org-mongos = 3.6.11 for package: mongodb-org-3.6.11-1.el7.x86_64
--> Running transaction check

太慢可以使用清华rpm安装

法二

https://mirrors.tuna.tsinghua.edu.cn/help/mongodb/

mongodb安装镜像帮助

新建 /etc/yum.repos.d/mongodb.repo，内容为

[mongodb-org]
name=MongoDB Repository
baseurl=https://mirrors.tuna.tsinghua.edu.cn/mongodb/yum/el$releasever/
```

### repo源

/etc/yum.repos.d/
目录下是常用的repo文件
如sublime-text.repo

### centos7学习资料

centos7学习资料
https://wiki.centos.org/zh/FrontPage?action=show&redirect=zh

https://wiki.centos.org/FrontPage

### 端口操作

```bash
# centos7开启端口

添加端口
firewall-cmd --zone=public --add-port=80/tcp --permanent # 添加端口 
firewall-cmd --reload # 重新载入端口
查看
firewall-cmd --zone=public --query-port=80/tcp
删除
firewall-cmd --zone=public --remove-port=80/tcp --permanent

# 防火墙开放8888端口

firewall-cmd --zone=public --add-port=8888/tcp --permanent
systemctl restart firewalld.service
iptables -L -n

# 2、安装firewalld
root执行 # yum install firewalld firewall-config
 
# 3、运行、停止、禁用firewalld
启动：# systemctl start  firewalld
查看状态：# systemctl status firewalld 或者 firewall-cmd --state
停止：# systemctl disable firewalld
禁用：# systemctl stop firewalld

# 启动时自动开启
systemctl enable firewalld
```

### centos7开启防火墙

1、firewalld的基本使用
启动： systemctl start firewalld
关闭： systemctl stop firewalld
查看状态： systemctl status firewalld 
开机禁用  ： systemctl disable firewalld
开机启用  ： systemctl enable firewalld

2.systemctl是CentOS7的服务管理工具中主要的工具，它融合之前service和chkconfig的功能于一体。
启动一个服务：systemctl start firewalld.service
关闭一个服务：systemctl stop firewalld.service
重启一个服务：systemctl restart firewalld.service
显示一个服务的状态：systemctl status firewalld.service
在开机时启用一个服务：systemctl enable firewalld.service
在开机时禁用一个服务：systemctl disable firewalld.service
查看服务是否开机启动：systemctl is-enabled firewalld.service
查看已启动的服务列表：systemctl list-unit-files|grep enabled
查看启动失败的服务列表：systemctl --failed

3.配置firewalld-cmd

查看版本： firewall-cmd --version
查看帮助： firewall-cmd --help
显示状态： firewall-cmd --state
查看所有打开的端口： firewall-cmd --zone=public --list-ports
更新防火墙规则： firewall-cmd --reload
查看区域信息:  firewall-cmd --get-active-zones
查看指定接口所属区域： firewall-cmd --get-zone-of-interface=eth0
拒绝所有包：firewall-cmd --panic-on
取消拒绝状态： firewall-cmd --panic-off
查看是否拒绝： firewall-cmd --query-panic

那怎么开启一个端口呢
添加
firewall-cmd --zone=public --add-port=80/tcp --permanent    （--permanent永久生效，没有此参数重启后失效）
重新载入
firewall-cmd --reload
查看
firewall-cmd --zone= public --query-port=80/tcp
删除
firewall-cmd --zone= public --remove-port=80/tcp --permanent

### centos关机命令

systemctl poweroff -i

### ssh

```bash
rpm -qa | grep openssh-server
systemctl start sshd
# ifconfig命令不存在
sudo yum install net-tools
在安装的时候选择 网络部分，网络地址转换(NAT) 模式，安装好之后 ：
这里宿主机是win7，ip是192.168.52.238  虚拟机ip为10.0.2.15  我们用端口40001来转发虚拟机的22端口 

设置好之后就能在宿主机里用 sercurecrt 登陆虚拟机了

用这种方式，虚拟机既能访问外网，主机又能ssh上去管理虚拟机，而且最重要的是虚拟机在局域网环境内不需要再分配独立的ip（用主机的ip加指定端口）

同样的，在虚拟机里也能通过ssh 访问宿主机同网段的其他机器

在本机ssh远程
ssh root@本机ip -p 40001

### 2.1关闭ssh服务

systemctl stop sshd

### 2.2禁止自动启动

system disable sshd
```

### docker

```bash
4、安装需要的软件包， yum-util 提供yum-config-manager功能，另外两个是devicemapper驱动依赖的

$ sudo yum install -y yum-utils device-mapper-persistent-data lvm2

5、设置yum源

$ sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

6、可以查看所有仓库中所有docker版本，并选择特定版本安装

$ yum list docker-ce --showduplicates | sort -r

7、安装docker

$ sudo yum install docker-ce  #由于repo中默认只开启stable仓库，故这里安装的是最新稳定版17.12.0
$ sudo yum install <FQPN>  # 例如：sudo yum install docker-ce-17.12.0.ce

8、启动并加入开机启动

$ sudo systemctl start docker
$ sudo systemctl enable docker

9、验证安装是否成功(有client和service两部分表示docker安装启动都成功了)

$ docker version


二、问题

1、因为之前已经安装过旧版本的docker，在安装的时候报错如下：

Transaction check error:
  file /usr/bin/docker from install of docker-ce-17.12.0.ce-1.el7.centos.x86_64 conflicts with file from package docker-common-2:1.12.6-68.gitec8512b.el7.centos.x86_64
  file /usr/bin/docker-containerd from install of docker-ce-17.12.0.ce-1.el7.centos.x86_64 conflicts with file from package docker-common-2:1.12.6-68.gitec8512b.el7.centos.x86_64
  file /usr/bin/docker-containerd-shim from install of docker-ce-17.12.0.ce-1.el7.centos.x86_64 conflicts with file from package docker-common-2:1.12.6-68.gitec8512b.el7.centos.x86_64
  file /usr/bin/dockerd from install of docker-ce-17.12.0.ce-1.el7.centos.x86_64 conflicts with file from package docker-common-2:1.12.6-68.gitec8512b.el7.centos.x86_64

2、卸载旧版本的包

$ sudo yum erase docker-common-2:1.12.6-68.gitec8512b.el7.centos.x86_64

3、再次安装docker
$ sudo yum install docker-ce

```

### wifi连接

```bash
一：所用命令
dmesg | grep firmware（看看有没有来自无线网卡的固件请求）
iw：
     iw dev(查找无线网卡口)
     iw wls1 link(查看wls1网口无线网络连接情况)
     iw wls1 scan | grep SSID(查看wls1网口可连接的wifi)
ip：
     ip link set wls1 up(将无线网口wls1开启)
     ip link show wls1(显示无线网口wls1连接情况)
     ip addr  show wls1(显示分配的ip地址，特别适用于查看是否成功地通过dhcp自动获取了ip地址) 
wpa_supplican:
     wpa_supplicant -B -i wlp3s0 -c <(wpa_passphrase "ssid" "psk") (连接无线网ssid，密码psk)
dhclient:
     dhclient wls1(为wls1分配ip地址)
如需使用上述命令，只需将wls1直接更换成自己网口就行了

二：具体过程：
查看是否需要安装固件
大多无线网卡还需要固件。内核一般会自动探测并加载两者，如果您得到类似 SIOCSIFFLAGS: No such file or directory 的输出，意味着您得手动加载固件。若不确定，用 dmesg 查询内核日志，看看有没有来自无线网卡的固件请求。比如您有 Intel 芯片组，输出大概是这样：

### dmesg | grep firmware

firmware: requesting iwlwifi-5000-1.ucode
若无输出，表明系统的无线芯片不需要固件。
查看无线网口：

### iw dev(interface后面即为无线网口号)

激活无线网络接口：

### ip link set wls1 up 

为了检验接口是否激活成功，您可以查看以下命令的输出：

### ip link show wls1

3: wls1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state DOWN mode DORMANT group default qlen 1000 link/ether 00:11:22:33:44:55 brd ff:ff:ff:ff:ff:ff 
<BROADCAST,MULTICAST,UP,LOWER_UP> 中的UP 表明该接口激活成功，后面的 state DOWN 无关紧要。
查看无线网络连接情况：

### iw wls1 link

刚开始应该会显示无连接
扫描可连接的wifi

### iw wls1 scan | grep SSID

扫描可用的网络
连接指定的SSID

### wpa_supplicant -B -i wlp3s0 -c <(wpa_passphrase "ssid" "psk") 

将ssid 替换为实际的网络名称，psk 替换为无线密码，请保留引号。
用dhcp 获得 IP 分配：

### dhclient wlp3s0 

测试是否成功地从路由器获取了ip(重要)

### ip addr  show wls1

如果分配有ip，即可上网，也可以有ping直接测试

```



### Linux下 is not in the sudoers file

```bash
xxx is not in the sudoers file. This incident will be reported

解决办法

用su换为root用户，并输入以下命令进入sudo配置文件
$ su – root
输入以下命令进入sudo配置文件

# visudo

在配置文件里找到下边的位置，并加入用户权限，保存退出

# Allow root to run any commands anywhere
user ALL=(ALL) ALL

这里使用/root可以快速定位
```

### 查找centos7的openjdk

```
# which java

# cd /usr/lib/jvm

/etc/alternatives/java_sdk_1.8.0_openjdk/lib
查找jdk

查找jdk的执行命令

$ which java

/usr/bin/java

$ ls -lrt /usr/bin/java

/usr/bin/java -> /etc/alternatives/java

$ ls -lrt /etc/alternatives/java

/etc/alternatives/java -> /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.191.b12-1.el7_6.x86_64/jre/bin/java

$ cd /etc/alternatives/java-1.8.0-openjdk-1.8.0.191.b12-1.el7_6.x86_64/

```

### centos 没有pip

```bash
# 1。查看是否安装依赖包，没安装先安装：
yum install epel-release

# 2。更新文件库
yum -y update

# 3。安装pip
yum -y install python-pip
```


### centos7安装mysql5.7
```bash
https://dev.mysql.com/doc/mysql-yum-repo-quick-guide/en/

### 1.配置yum源

1.1下载mysql源安装包
在MySQL官网中下载YUM源rpm安装包：http://dev.mysql.com/downloads/repo/yum/ 

https://dev.mysql.com/doc/mysql-yum-repo-quick-guide/en/
安装mysql57

wget http://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm

安装MySQL源

yum localinstall mysql57-community-release-el7-8.noarch.rpm


检查mysql源是否安装成功

yum repolist enabled | grep "mysql.*-community.*"


或者是

yum repolist enabled | grep mysql.*


可以修改vim /etc/yum.repos.d/mysql-community.repo源，改变默认安装的mysql版本。比如要安装5.6版本，将5.7源的enabled=1改成enabled=0。然后再将5.6源的enabled=0改成enabled=1即可。改完之后的效果如下所示： 

### 2.安装mysql

yum install mysql-community-server


### 3.启动mysql服务

systemctl start mysqld
sudo systemctl start mysqld.service


查看状态
centos7中常用

sudo systemctl status mysqld.service
sudo service mysqld status
### 4.开机启动
systemctl enable mysqld
systemctl daemon-reload
### 5、修改root本地登录密码

mysql安装完成之后，在/var/log/mysqld.log文件中给root生成了一个默认密码。通过下面的方式找到root默认密码，然后登录mysql进行修改：

shell> grep 'temporary password' /var/log/mysqld.log


进入mysql并修改密码

mysql -uroot -p
ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass4!';


### 6.添加远程登录用户

默认只允许root帐户在本地登录，如果要在其它机器上连接mysql，必须修改root允许远程连接，或者添加一个允许远程连接的帐户，为了安全起见，我添加一个新的帐户：

CREATE USER 'finley'@'%' IDENTIFIED BY 'password';

GRANT ALL PRIVILEGES ON *.* TO 'finley'@'%' WITH GRANT OPTION;


下面是全部语句

mysql> GRANT ALL PRIVILEGES ON *.* TO 'yangxin'@'%' IDENTIFIED BY 'Yangxin0917!' WITH GRANT OPTION;
FLUSH PRIVILEGES;



### 7、配置默认编码为utf8

修改/etc/my.cnf配置文件，在[mysqld]下添加编码配置，如下所示：

[mysqld]
character_set_server=utf8
init_connect='SET NAMES utf8'

重新启动mysql服务，查看数据库默认编码如下所示
show variables like '%character%';

配置文件：/etc/my.cnf 
日志文件：/var/log/mysqld.log 
服务启动脚本：/usr/lib/systemd/system/mysqld.service 
socket文件：/var/run/mysqld/mysqld.pid

重启mysql
systemctl restart mysql

mysql服务命令


# service 命令和systemctl优先考虑systemctl
service mysqld stop
service mysqld start
service mysqld restart
service mysqld status


```
### 安装openjdk

```bash
安装jre：

sudo yum install java-1.8.0-openjdk

然后会有些安装提示信息，一直“y”回车就好。

安装jdk：

sudo yum install java-1.8.0-openjdk-devel

也有些安装提示信息，一直“y”回车就好。

查看jre安装情况：

java -version

显示：

openjdk version "1.8.0_181"
OpenJDK Runtime Environment (build 1.8.0_181-b13)
OpenJDK 64-Bit Server VM (build 25.181-b13, mixed mode)

查看jdk安装情况：

javac -version

显示：
javac 1.8.0_181


2.配置环境变量。

运行命令：vim  /etc/profile

会提示文件已存在，输入“e”回车。

编辑文件，在最后加上：

#Java
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.181-3.b13.el7_5.x86_64
export CALSSPATH=$JAVA_HOME/lib/*.*
export PATH=$PATH:$JAVA_HOME/bin 

然后键盘按下“Esc”，再按下“：”双引号，输入wq回车保存退出vim编辑模式。

最后需要：

source  /etc/profile

使修改生效。

其中/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.181-3.b13.el7_5.x86_64是你的jdk的默认安装路径。
```

## 安装nginx

### 一、安装编译工具及库文件

```bash
yum -y install make zlib zlib-devel gcc-c++ libtool  openssl openssl-devel

```

### 二、首先要安装 PCRE

PCRE 作用是让 Nginx 支持 Rewrite 功能。

1、下载 PCRE 安装包，下载地址： [http://downloads.sourceforge.net/project/pcre/pcre/8.35/pcre-8.35.tar.gz](http://downloads.sourceforge.net/project/pcre/pcre/8.35/pcre-8.35.tar.gz)

```bash
[root@bogon src]# cd /usr/local/src/
[root@bogon src]# wget http://downloads.sourceforge.net/project/pcre/pcre/8.35/pcre-8.35.tar.gz

```

2、解压安装包:

```
[root@bogon src]# tar zxvf pcre-8.35.tar.gz

```

3、进入安装包目录

```
[root@bogon src]# cd pcre-8.35

```

4、编译安装 

```
[root@bogon pcre-8.35]# ./configure
[root@bogon pcre-8.35]# make && make install

```

5、查看pcre版本

```
[root@bogon pcre-8.35]# pcre-config --version

```

### 安装 Nginx

1、下载 Nginx，下载地址：[http://nginx.org/download/nginx-1.6.2.tar.gz](http://nginx.org/download/nginx-1.6.2.tar.gz)

```
[root@bogon src]# cd /usr/local/src/
[root@bogon src]# wget http://nginx.org/download/nginx-1.6.2.tar.gz

```

2、解压安装包

```
[root@bogon src]# tar zxvf nginx-1.6.2.tar.gz

```

3、进入安装包目录

```
[root@bogon src]# cd nginx-1.6.2

```

4、编译安装

```
[root@bogon nginx-1.6.2]# ./configure --prefix=/usr/local/webserver/nginx --with-http_stub_status_module --with-http_ssl_module --with-pcre=/usr/local/src/pcre-8.35
[root@bogon nginx-1.6.2]# make
[root@bogon nginx-1.6.2]# make install

```

5、查看nginx版本

```
[root@bogon nginx-1.6.2]# /usr/local/webserver/nginx/sbin/nginx -v

```

到此，nginx安装完成。



### Nginx 配置

创建 Nginx 运行使用的用户 www：

```
[root@bogon conf]# /usr/sbin/groupadd www 
[root@bogon conf]# /usr/sbin/useradd -g www www

```

配置nginx.conf ，将/usr/local/webserver/nginx/conf/nginx.conf替换为以下内容



```
[root@bogon conf]#  cat /usr/local/webserver/nginx/conf/nginx.conf

user www www;
worker_processes 2; #设置值和CPU核心数一致
error_log /usr/local/webserver/nginx/logs/nginx_error.log crit; #日志位置和日志级别
pid /usr/local/webserver/nginx/nginx.pid;
#Specifies the value for maximum file descriptors that can be opened by this process.
worker_rlimit_nofile 65535;
events
{
  use epoll;
  worker_connections 65535;
}
http
{
  include mime.types;
  default_type application/octet-stream;
  log_format main  '$remote_addr - $remote_user [$time_local] "$request" '
               '$status $body_bytes_sent "$http_referer" '
               '"$http_user_agent" $http_x_forwarded_for';
  
#charset gb2312;
     
  server_names_hash_bucket_size 128;
  client_header_buffer_size 32k;
  large_client_header_buffers 4 32k;
  client_max_body_size 8m;
     
  sendfile on;
  tcp_nopush on;
  keepalive_timeout 60;
  tcp_nodelay on;
  fastcgi_connect_timeout 300;
  fastcgi_send_timeout 300;
  fastcgi_read_timeout 300;
  fastcgi_buffer_size 64k;
  fastcgi_buffers 4 64k;
  fastcgi_busy_buffers_size 128k;
  fastcgi_temp_file_write_size 128k;
  gzip on; 
  gzip_min_length 1k;
  gzip_buffers 4 16k;
  gzip_http_version 1.0;
  gzip_comp_level 2;
  gzip_types text/plain application/x-javascript text/css application/xml;
  gzip_vary on;
 
  #limit_zone crawler $binary_remote_addr 10m;
 #下面是server虚拟主机的配置
 server
  {
    listen 80;#监听端口
    server_name localhost;#域名
    index index.html index.htm index.php;
    root /usr/local/webserver/nginx/html;#站点目录
      location ~ .*\.(php|php5)?$
    {
      #fastcgi_pass unix:/tmp/php-cgi.sock;
      fastcgi_pass 127.0.0.1:9000;
      fastcgi_index index.php;
      include fastcgi.conf;
    }
    location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|ico)$
    {
      expires 30d;
  # access_log off;
    }
    location ~ .*\.(js|css)?$
    {
      expires 15d;
   # access_log off;
    }
    access_log off;
  }

}

```

检查配置文件nginx.conf的正确性命令：

```
[root@bogon conf]# /usr/local/webserver/nginx/sbin/nginx -t

```



### 启动 Nginx

Nginx 启动命令如下：

```
[root@bogon conf]# /usr/local/webserver/nginx/sbin/nginx

```

检测是否正常启动

```bash
ps -ef | grep nginx

```

### 访问站点

从浏览器访问我们配置的站点ip：

### Nginx 其他命令

以下包含了 Nginx 常用的几个命令：

```
/usr/local/webserver/nginx/sbin/nginx -s reload            # 重新载入配置文件
/usr/local/webserver/nginx/sbin/nginx -s reopen            # 重启 Nginx
/usr/local/webserver/nginx/sbin/nginx -s stop              # 停止 Nginx

```

### 其他问题

我们可以通过log文件快速定位错误，常见的错误，有用户权限问题，例如我们通过命令，可以查看启动nginx的用户
```bash
# 查看启动进程
ps -ef | grep nginx

```
如果出现用户不是root，需要我们手动在conf文件中添加
```bash
vim /usr/local/webserver/nginx/conf/nginx.conf

user root;
```
此外，我们server位置设定的命令，是有针对性的，root设置根目录，index设置主页文件，等等。



## 安装npm

```bash
wget https://npm.taobao.org/mirrors/node/v8.0.0/node-v8.0.0-linux-x64.tar.xz
tar -xvf  node-v8.0.0-linux-x64.tar.xz
mv node-v8.1.4-linux-x64 node
vim /etc/profile




```

### 文件最后添加

```
#set for nodejs  
export NODE_HOME=/usr/local/node  
export PATH=$NODE_HOME/bin:$PATH
```

### 保存退出后执行更新命令

```
source /etc/profile
```

## tuna服务

一键设置清华大学源

https://tuna.moe/oh-my-tuna/

## Scala安装

### 安装之前

首先，保证已经安装了jdk

### 安装idea

去官网下载即可

### 安装Scala-idea插件

下载Scala插件

```
http://plugins.jetbrains.com/plugin/1347-scala
```

或者在idea的插件库里选择安装

### 安装过程

#### 1.下载

```
https://www.scala-lang.org/download/
```

```bash
下载后进入安装包所在目录进行解压操作(我下载的是：scala-2.11.8.tgz)

    tar -zxvf scala-2.11.8.tgz

可以选择手动解压

然后迁移文件到指定文件夹下
如：

    解压，将文件夹移动到/usr/local/share
    
    $ mv download/your scala path /usr/local/share
```



#### 2.配置环境变量

```bash
Mac修改 .bash_profile 文件，此文件是mac 当前用户的环境配置文件。

/etc/profile 是当前系统的环境配置文件（Linux，系统可修改这个）

.bash_profile 文件的路径是在当前用户下。

    vi .bash_profile
    //添加如下信息
    export SCALA_HOME=你Scala的路径/scala
    export PATH=$PATH:$SCALA_HOME/bin

实例结果：

    export PATH=${PATH}:/usr/local/share/scala-2.11.7/bin
刷新设置：  

    source ~/.bash_profile 刷新设置
```

#### 3.检验结果

```bash
在终端输入scala 命令，进入scala解释器，然后输入1＋2，查看计算结果。
使用

    :q
退出程序

```

### 建立教程：

```
https://docs.scala-lang.org/getting-started-intellij-track/getting-started-with-scala-in-intellij.html
```

## docker

### daocloud

https://hub.daocloud.io/repos?type=featured

https://www.daocloud.io/mirror



### 常用可选参数

常用可选参数说明：

```bash
-i # 表示以“交互模式”运行容器
-t # 表示容器启动后会进入其命令行。加入这两个参数后，容器创建就能登录进去。即 分配一个伪终端。
--name # 为创建的容器命名
-v # 表示目录映射关系(前者是宿主机目录，后者是映射到宿主机上的目录，即 宿主机目录:容器中目录)，可以使 用多个-v 做多个目录或文件映射。注意:最好做目录映射，在宿主机上做修改，然后 共享到容器上。
-d # 在run后面加上-d参数,则会创建一个守护式容器在后台运行(这样创建容器后不 会自动登录容器，如果只加-i -t 两个参数，创建后就会自动进去容器)。
-p # 表示端口映射，前者是宿主机端口，后者是容器内的映射端口。可以使用多个-p 做多个端口映射
-e # 为容器设置环境变量
--network=host # 表示将主机的网络环境映射到容器中，容器的网络与主机相同
###########
docker ps -a # docker 列举容器
docker stop container id # docker 关闭容器
docker start container id # docker 启动容器
docker cp sourcepath containerId:targetpath # 复制文件

```

### 镜像加速器

Linux系统目前存在的三种系统启动方式所对应的配置文件目录分别为：

- SysVinit：/etc/init.d目录；
- UpStart： /usr/share/upstart目录；
- Systemd：/usr/lib/systemd目录；

```bash
# ubuntu 16+, debian 8+, centos7
# 使用systemd系统，在/etc/docker/daemon.json写入内容
{"registry-mirrors":[
	"https://registry.docker-cn.com"
	]
}
# 重启服务
sudo systemctl daemon-reload
sudo systemctl restart docker

# win10
# 在系统托盘选择settings，在配置窗口左侧导航栏选择daemon，在register mirros填写
# https://register.docker-cn.com

# macos
# 在任务栏中点击docker for mac daemon register mirrors，docker重启并应用配置的镜像地址

```

### docker镜像操作

```bash
# 我们对镜像操作时，可以使用短id，只需取前3个字符，只要足够区分于别的镜像就可以了。
docker search xxx # docker 搜索镜像
docker pull xxx # docker 拉取镜像
docker image ls # docker 列举镜像
docker image ls -a # docker 列举所有镜像


docker image rm xxx# docker 删除镜像

docker image rm $(docker image ls -q redis) # 删除所有仓库名为redis的镜像

```

### docker容器操作

```bash
docker run xxx # docker新建并启动
# -t 非配一个终端， -i让容器的标准输入并保持打开
# -d后台运行
docker container ls # docker查看容器，-a查看所有（包括终止的）
docker container stop # 终止运行的容器

# 在使用 -d参数后，容器启动后悔进入后台
docker exec xxx # 进入容器进行操作, -i -t分配终端
# 如下所示
docker run -itd ubuntu
docker container ls
docker exec -i xxx bash
# 或者
docker exec -it xxx bash

docker export xx > ubuntu.tar # 导出容器
cat ubuntu.tar | docker import - test/ubuntu:v1.0 # 导入容器

docker container rm xxx # 删除一个容器，添加-f参数，删除一个运行中的容器
docker container prune # 清理所有处于终止状态的容器
```

## zsh

```bash
## check you have zsh
cat /etc/shells
# if you have not, I suggest you have a try
## install the zsh
### centos install
sudo yum -y install zsh
### ubuntu install
sudo apt-get -y install zsh
# and you can check you have zsh when you have execute above
## change your default shell to zsh

chsh -s /bin/zsh

## install enhance setting - oh my zsh!

sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"


## index back

# if you have install miniconda or conda and so on (like some setting in ~/.bashrc), it can be not work, you should copy this setting to ~/.zshrc, and use the script

source ~/.zshrc
```

## jupyter生成目录

https://www.jianshu.com/p/f314e9868cae

https://blog.csdn.net/weixin_42150990/article/details/81081889

```
conda install -c conda-forge jupyter_contrib_nbextensions
```

运行Jupyter Notebook, 在打开的Notebook界面里, 你会发现多了一个Nbextensions,点击这个tab, 会有如下界面



勾选Table of Contents (有的版本是toc2). 然后创建或者打开一个Jupter Notebook

第四步, 生成目录

在Notebook上面选项中,多了一个生成目录图标, 如下图中最右边的图标.

jupyter nbconvert --to markdown "3.11-matplotlib 基础.ipynb"

## mac配置


### iterm2

command + [

command + ]

左右切屏

command + d 垂直分屏

command +shift + d 水平分屏

command + w 标签

### homebrew



## tomcat配置域名服务器

最近做了个网站，用的是web'服务器是tomcat，框架式SpringMVC，功能做好后，就准备上线使用了，手上已经有域名以及一台服务器，已经绑定好ip了，剩下的也就是配置

Tomcat了，比较简单，但是自己记录下防止遗忘了，

首先，访问服务器时默认的是80端口，这个好改，tomcat中的server.xml文件直接修改，这里要说明的是如果一个服务器上有多个tomcat的话，修改端口需要注意的是要修改

三个地方的 



第一处是 <Server port="8085" shutdown="SHUTDOWN">



第二处是  <Connector connectionTimeout="20000" port="80" protocol="HTTP/1.1" redirectPort="8443"/>



第三处是 <Connector port="8099" protocol="AJP/1.3" redirectPort="8443"/>



修改好端口

在修改两处地方



第一是  <Engine defaultHost="localhost" name="Catalina">  把defaultHost的值修改成你的域名    <Engine defaultHost="www.test.com" name="Catalina">



第二是   <Host appBase="webapps" autoDeploy="true" name="localhost" unpackWARs="true" xmlNamespaceAware="false" xmlValidation="false">



                 把name的值修改成你的域名  
    
                <Host appBase="webapps" autoDeploy="true" name="www.test.com" unpackWARs="true" xmlNamespaceAware="false" xmlValidation="false">



最后再加上具体的项目指向



       在Host下面加上 <Context docBase="testPro" path="" reloadable="true"/></Host>
    
      这个testPro就是tomcat中的项目名称



保存server.xnl文件，重新启动服务，如果你的域名和ip绑定好的话就可以直接用域名访问了  



如果你不确定域名是否绑上了正确的外网ip，可以直接在dos里面ping  域名   如果显示的是正确的外网ip，那么就没有问题了....

