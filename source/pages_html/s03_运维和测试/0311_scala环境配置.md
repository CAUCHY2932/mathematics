## Scala安装
### 安装之前

首先，保证已经安装了jdk

### 安装idea

去官网下载即可

### 安装Scala-idea插件
下载Scala插件

    http://plugins.jetbrains.com/plugin/1347-scala
或者在idea的插件库里选择安装
### 安装过程
#### 1.下载

    https://www.scala-lang.org/download/
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


    https://docs.scala-lang.org/getting-started-intellij-track/getting-started-with-scala-in-intellij.html


