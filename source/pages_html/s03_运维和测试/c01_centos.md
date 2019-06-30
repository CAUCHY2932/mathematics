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



### 1.2设置pip镜像源

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

```
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

启动jupyter notebook：

nohup jupyter lab --allow-root &

登录jupyter lab

http://服务器ip地址:8888/lab

10.10.10.100:8888/lab


2、安装firewalld
root执行 # yum install firewalld firewall-config
 
3、运行、停止、禁用firewalld
启动：# systemctl start  firewalld
查看状态：# systemctl status firewalld 或者 firewall-cmd --state
停止：# systemctl disable firewalld
禁用：# systemctl stop firewalld

# 启动时自动开启
systemctl enable firewalld
```
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
pip在centos也没有，所以网上找来资料，3条语句就搞定啦！

1。查看是否安装依赖包，没安装先安装：

yum install epel-release

2。更新文件库

yum -y update

3。安装pip

yum -y install python-pip

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
--------------------- 
作者：yzh_1346983557 
来源：CSDN 
原文：https://blog.csdn.net/yzh_1346983557/article/details/81509329 
版权声明：本文为博主原创文章，转载请附上博文链接！
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



## 在centos7上安装mongodb

### 添加源

> vim /etc/yum.repos.d/MongoDB.repo

```
[mongodb-org-3.6]

name=MongoDB Repository

baseurl=https://repo.mongodb.org/yum/redhat/\$releasever/mongodb-org/3.6/x86_64/

gpgcheck=1

enabled=1

gpgkey=https://www.mongodb.org/static/pgp/server-3.6.asc
```

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

```
[mongodb-org]
name=MongoDB Repository
baseurl=https://mirrors.tuna.tsinghua.edu.cn/mongodb/yum/el$releasever/


```

## repo

/etc/yum.repos.d/
目录下是常用的repo文件
如sublime-text.repo





## centos7 设置samba



> https://www.linuxidc.com/Linux/2017-11/148354.htm

### 服务器端下载samba和客户端包

yum -y install samba samba-client

### 客户端需要下载samba-client cifs-utils包

### 服务器端编辑配置文件

配置文件 /etc/samba/smb.conf
启动和关闭文件 /etc/init.d/smb

### 查看服务启动情况，设置服务开机自动启动

service smb status
service smb start
chkconfig  --level 35 smb on
chkconfig --list smb

### 修改smb服务配置文件

vim /etc/samba/smb.conf

[global]　　　　　　　　　　　　　　　　　　　　//设置samba服务整体环境

workgroup = WORKGROUP　　　　　　　　　　  //设置工作组名称
server string = Samba Server Version %v                 //服务器说明

[laiGei]　　　　　　　　　　　　　　　　　　   //共享目录的名称

comment = Public stuff　　　　　　　　　　　　   //注释说明　
path = /usr/local/laige　　　　　　　　   　　　　  //共享目录的路径
public = yes　　　　　　　　　　　　　　　　　  //是yes/否no公开共享，若为否则进行身份验证(只有当security = share 时此项才起作用)
writeable = yes　　　　　　　　　　　　　　　　//是yes/否no不以只读方式共享当与read only发生冲突时，无视read only
browseable = yes　　　　　　　　　　　　　　   //是yes/否no在浏览资源中显示共享目录，若为否则必须指定共享路径才能存取
guest ok = yes　　　　　　　　　　　　　　　　//是yes/否no公开共享，若为否则进行身份验证(只有当security = share 时此项才起作用)





 [global]
     workgroup = SAMBA
     security = user

```
 passdb backend = tdbsam

 printing = cups
 printcap name = cups
 load printers = yes
 cups options = raw
```







security = user
说明：设置用户访问Samba Server的验证方式，一共有四种验证方式。

```
share：用户访问Samba Server不需要提供用户名和口令, 安全性能较低。
user：Samba Server共享目录只能被授权的用户访问,由Samba Server负责检查账号和密码的正确性。账号和密码要在本Samba Server中建立。
server：依靠其他Windows NT/2000或Samba Server来验证用户的账号和密码,是一种代理验证。此种安全模式下,系统管理员可以把所有的Windows用户和口令集中到一个NT系统上,使用Windows NT进行Samba认证, 远程服务器可以自动认证全部用户和口令,如果认证失败,Samba将使用用户级安全模式作为替代的方式。
domain：域安全级别,使用主域控制器(PDC)来完成认证。
```

passdb backend = tdbsam
说明：passdb backend就是用户后台的意思。目前有三种后台：smbpasswd、tdbsam和ldapsam。sam应该是security account manager（安全账户管理）的简写。

```
smbpasswd：该方式是使用smb自己的工具smbpasswd来给系统用户（真实
用户或者虚拟用户）设置一个Samba密码，客户端就用这个密码来访问Samba的资源。smbpasswd文件默认在/etc/samba目录下，不过有时候要手工建立该文件。

tdbsam：该方式则是使用一个数据库文件来建立用户数据库。数据库文件叫passdb.tdb，默认在/etc/samba目录下。passdb.tdb用户数据库可以使用smbpasswd –a来建立Samba用户，不过要建立的Samba用户必须先是系统用户。我们也可以使用pdbedit命令来建立Samba账户。pdbedit命令的参数很多，我们列出几个主要的。

        pdbedit –a username：新建Samba账户。
        pdbedit –x username：删除Samba账户。
        pdbedit –L：列出Samba用户列表，读取passdb.tdb数据库文件。
        pdbedit –Lv：列出Samba用户列表的详细信息。
        pdbedit –c “[D]” –u username：暂停该Samba用户的账号。
        pdbedit –c “[]” –u username：恢复该Samba用户的账号。
```



















service smb restart

### 新建samba用户，必须是系统中存在的用户

useradd samba
smbpasswd -a samba

### 注意事项

　　1，防火墙要关闭， # service iptables  stop

　　2，selinux要设置成disabled，路径是/etc/sysconfig/selinux

　　3，注意共享目录的权限设置

　　4，要设置成不需要用户名密码直接访问，需要修改配置文件，将security设置成security = share。

 

 使用smbpasswd添加共享用户的常用方法：

　　　　smbpasswd -a 添加用户（被添加用户必须是系统用户）

　　　　smbpasswd -d 冻结用户 （这个用户不能用了）

　　　　smbpasswd -e 恢复用户 （将冻结的用户解冻）

　　　　smbpasswd -n 将用户密码设置为空 

　　　　smbpasswd -x 删除用户







### 其他
```bash
[logger]
    comment = Logs Directories
    path = /storage/logger/
    public = no
    admin users = logadmin
    valid users = @logadmin
    browseable = yes
    writable = yes
    create mask = 0777
    directory mask = 0777
    force directory mode = 0777
    force create mode = 0777

[shared]

    # 共享文件目录描述
​    comment = Shared Directories
    # 共享文件目录
​    path = /storage/shared/
    # 是否允许guest访问
​    public = no
    # 指定管理用户
​    admin users = admin
    # 可访问的用户组、用户
​    valid users = @admin
    # 是否浏览权限
​    browseable = yes
    # 是否可写权限
​    writable = yes
    # 文件权限设置
​    create mask = 0777
​    directory mask = 0777
​    force directory mode = 0777
​    force create mode = 0777

```



## centos7学习资料

centos7学习资料
https://wiki.centos.org/zh/FrontPage?action=show&redirect=zh

https://wiki.centos.org/FrontPage

## centos7安装mysql5.7

https://dev.mysql.com/doc/mysql-yum-repo-quick-guide/en/

### 1.配置yum源

1.1下载mysql源安装包
在MySQL官网中下载YUM源rpm安装包：http://dev.mysql.com/downloads/repo/yum/ 

https://dev.mysql.com/doc/mysql-yum-repo-quick-guide/en/
安装mysql57
```bash
wget http://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm
```
安装MySQL源
```bash
yum localinstall mysql57-community-release-el7-8.noarch.rpm
```
检查mysql源是否安装成功
```bash
yum repolist enabled | grep "mysql.*-community.*"
```
或者是
```bash
yum repolist enabled | grep mysql.*
```
可以修改vim /etc/yum.repos.d/mysql-community.repo源，改变默认安装的mysql版本。比如要安装5.6版本，将5.7源的enabled=1改成enabled=0。然后再将5.6源的enabled=0改成enabled=1即可。改完之后的效果如下所示： 

### 2.安装mysql

```bash
yum install mysql-community-server
```
### 3.启动mysql服务

```bash
systemctl start mysqld
sudo systemctl start mysqld.service
```
查看状态
centos7中常用

```bash
sudo systemctl status mysqld.service
sudo service mysqld status
```
### 4.开机启动

```bash
systemctl enable mysqld
systemctl daemon-reload
```
### 5、修改root本地登录密码
mysql安装完成之后，在/var/log/mysqld.log文件中给root生成了一个默认密码。通过下面的方式找到root默认密码，然后登录mysql进行修改：

```bash
shell> grep 'temporary password' /var/log/mysqld.log
```
进入mysql并修改密码
```bash
mysql -uroot -p
ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass4!';
```
### 6.添加远程登录用户
默认只允许root帐户在本地登录，如果要在其它机器上连接mysql，必须修改root允许远程连接，或者添加一个允许远程连接的帐户，为了安全起见，我添加一个新的帐户：

```bash
CREATE USER 'finley'@'%' IDENTIFIED BY 'password';

GRANT ALL PRIVILEGES ON *.* TO 'finley'@'%' WITH GRANT OPTION;
```
下面是全部语句
```bash
mysql> GRANT ALL PRIVILEGES ON *.* TO 'yangxin'@'%' IDENTIFIED BY 'Yangxin0917!' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```
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
```bash
# service 命令和systemctl优先考虑systemctl
service mysqld stop
service mysqld start
service mysqld restart
service mysqld status
```
## centos7开启防火墙

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

## centos关机命令
systemctl poweroff -i

## centos安装oracle12c

参考链接
https://wiki.centos.org/zh/HowTos/Oracle12onCentos7
https://docs.centos.org/en-US/docs/

### 准备工作

提前下载oracle 12c
r1 版本有两个文件，r2有两个文件

1. 引言
   本指南介绍如何在 CentOS 7.1（64 位元）上利用快速安装的功能部署 Oracle 数据库 12c（12.1.0.2.0）。

参考系统：

[root@centos7 ~]# hostnamectl
   Static hostname: centos7.example.com
         Icon name: computer
           Chassis: n/a
        Machine ID: 583b4d69eaea465ea4bb96ac3b891e15
           Boot ID: 931ed1af622046ebbde071a87844a7d5
    Virtualization: kvm
  Operating System: CentOS Linux 7 (Core)
       CPE OS Name: cpe:/o:centos:centos:7
            Kernel: Linux 3.10.0-229.11.1.el7.x86_64
      Architecture: x86_64

2. 先决条件
   成功安装操作系统后，请确认主机名称并在你的 DNS 上登记它。你也可选择在 /etc/hosts 内加入你的主机名称／IP。

[root@centos7 ~]# cat /etc/hostname
centos7.example.com
将 SELinux 维持在 enforcing 模式，并启用防火墙

[root@centos7 ~]# sestatus
SELinux status:                 enabled
SELinuxfs mount:                /sys/fs/selinux
SELinux root directory:         /etc/selinux
Loaded policy name:             targeted
Current mode:                   enforcing
Mode from config file:          enforcing
Policy MLS status:              enabled
Policy deny_unknown status:     allowed
Max kernel policy version:      28

[root@centos7 ~]# firewall-cmd --state
running
把 CentOS 系统更新至最新组件

[root@centos7 ~]# yum update -y
下载 Oracle 数据库 12c 的 Linux x86-64 版本：

http://www.oracle.com/technetwork/database/enterprise-edition/downloads/index.html

3. 安装步骤
   为 Oracle 数据库创建所须的操作系统用户及群组。

[root@centos7 ~]# groupadd oinstall
[root@centos7 ~]# groupadd dba
[root@centos7 ~]# useradd -g oinstall -G dba oracle
[root@centos7 ~]# passwd oracle
在 /etc/sysctl.conf 加入下列内核参数

fs.aio-max-nr = 1048576
fs.file-max = 6815744
kernel.shmall = 2097152
kernel.shmmax = 1987162112
kernel.shmmni = 4096
kernel.sem = 250 32000 100 128
net.ipv4.ip_local_port_range = 9000 65500
net.core.rmem_default = 262144
net.core.rmem_max = 4194304
net.core.wmem_default = 262144
net.core.wmem_max = 1048586
检查并运用新的数值。

[root@centos7 ~]# sysctl -p
[root@centos7 ~]# sysctl -a
在 /etc/security/limits.conf 为 oracle 用户设置上限

oracle soft nproc 2047
oracle hard nproc 16384
oracle soft nofile 1024
oracle hard nofile 65536
将 Oracle 数据库软件的 zip 文件（linuxamd64_12102_database_1of2.zip, linuxamd64_12102_database_2of2.zip）解压至 /stage 目录。

[root@centos7 ~]# yum install -y zip unzip
[root@centos7 ~]# unzip linuxamd64_12102_database_1of2.zip -d /stage/
[root@centos7 ~]# unzip linuxamd64_12102_database_2of2.zip -d /stage/
修改　/stage 的权限

[root@centos7 ~]# chown -R oracle:oinstall /stage/
为 Oracle 软件创建 /u01 目录，及为数据库文件创建 /u02 目录。

[root@centos7 ~]# mkdir /u01
[root@centos7 ~]# mkdir /u02
[root@centos7 ~]# chown -R oracle:oinstall /u01
[root@centos7 ~]# chown -R oracle:oinstall /u02
[root@centos7 ~]# chmod -R 775 /u01
[root@centos7 ~]# chmod -R 775 /u02
[root@centos7 ~]# chmod g+s /u01
[root@centos7 ~]# chmod g+s /u02
安装所须组件：

[root@centos7 ~]# yum install -y binutils.x86_64 compat-libcap1.x86_64 gcc.x86_64 gcc-c++.x86_64 glibc.i686 glibc.x86_64 \
glibc-devel.i686 glibc-devel.x86_64 ksh compat-libstdc++-33 libaio.i686 libaio.x86_64 libaio-devel.i686 libaio-devel.x86_64 \
libgcc.i686 libgcc.x86_64 libstdc++.i686 libstdc++.x86_64 libstdc++-devel.i686 libstdc++-devel.x86_64 libXi.i686 libXi.x86_64 \
libXtst.i686 libXtst.x86_64 make.x86_64 sysstat.x86_64
还有安装 X Window System 组件群组。

[root@centos7 ~]# yum groupinstall -y "X Window System"
由于 Oracle 的安装采用图像界面，你可通过以下两个简单的方法进行。

方案 1

通过 SSH 从一台图像化 Linux 计算机远程登录。

ssh -X oracle@centos7.example.com
export LANG="en_US" 

方案 2

利用一台拥有 SSH 客户端（PuTTY）及 X-Windows 终端機仿真器（Xming）的微软 Windows 桌面。

以下文档描述如何在 Windows 系统上安装 Xming。

- Xming —— 微软 Windows 计算机下的 X-Windows 终端機仿真器

请采用上述的方案登录为 oracle 用户，然后执行 Oracle 安装程序：

[oracle@centos7 ~]$ /stage/database/runInstaller
Starting Oracle Universal Installer...

1. Oracle 安装程序画面
   第一步 —— 安全性更新

假若你不想接收来自 Oracle 支持部的电邮，请取消勾选该项目并按 Next。

在新打开的窗口按 YES。

第二步 —— 安装选项

选择 Create and configure a database 并按 Next

第三步 —— 系统级别

选择 Desktop Class 进行缺省的简便 Oracle 数据库安装。

第四步 —— 典型安装

在 Typical Install Configuration 画面，设置以下功能。

Oracle base

/u01/app/oracle

Software location

/u01/app/oracle/product/12.1.0/dbhome_1

Database file location

/u02

Global database name

orcl.example.com

另外请设置合适的 Database edition（数据库版本）及 Character set（符集）。请为数据库的管理订立一个安全的口令，最后请取消勾选 Create as Container database 项目。

第五步 —— 创建库存

接纳缺省的 /u01/app/oraInventory 并按 Next。

第六步 —— 检查先决条件

安装程序会自动检查所有必须的操作系统组件及内核设置。

第七步 —— 摘要

这是编辑安装特点的最后机会。请按 Install。

第八步 —— 执行设置脚本

当询问窗口出现时，请登录成为 root 并执行两个脚本：
/u01/app/oraInventory/orainstRoot.sh
/u01/app/oracle/product/12.2.0/dbhome_1/root.sh

[root@centos7 ~]# /u01/app/oraInventory/orainstRoot.sh
Changing permissions of /u01/app/oraInventory.
Adding read,write permissions for group.
Removing read,write,execute permissions for world.
Changing groupname of /u01/app/oraInventory to oinstall.
The execution of the script is complete.

[root@centos7 ~]# /u01/app/oracle/product/12.1.0/dbhome_1/root.sh
Performing root user operation.
The following environment variables are set as:
    ORACLE_OWNER= oracle
    ORACLE_HOME=  /u01/app/oracle/product/12.1.0/dbhome_1
Enter the full pathname of the local bin directory: [/usr/local/bin]: <PRESS ENTER>
   Copying dbhome to /usr/local/bin ...
   Copying oraenv to /usr/local/bin ...
   Copying coraenv to /usr/local/bin ...
Creating /etc/oratab file...
Entries will be added to the /etc/oratab file as needed by
Database Configuration Assistant when a database is created
Finished running generic part of root script.
Now product-specific root actions will be performed.
You can follow the installation in a separated window.
这两个脚本都必须以 root 的身份来执行。

第九步 —— 安装进度

一个显示安装进度的窗口将会出现。请勿关闭这个窗口。

第十步 —— 顺利完成安装

最后一个画面将会通知你安装已经完成并显示 Oracle 企业级管理员的 URL。

https://localhost:5500/em

请按 OK 来关闭安装程序。

5. 安装后的任务
   5.1. 防火墙
   请登录成为 root 并检查已引导的本地

[root@centos7 ~]# firewall-cmd --get-active-zones
public
  interfaces: eth0
打开相关的端口

[root@centos7 ~]# firewall-cmd --zone=public --add-port=1521/tcp --add-port=5500/tcp --add-port=5520/tcp --add-port=3938/tcp \ 
--permanent
success

[root@centos7 ~]# firewall-cmd --reload
success

[root@centos7 ~]# firewall-cmd --list-ports
1521/tcp 3938/tcp 5500/tcp 5520/tcp
5.2. Oracle 工作环境
请登录为 oracle 用户并在 /home/oracle/.bash_profile 内加入下列数值

TMPDIR=$TMP; export TMPDIR
ORACLE_BASE=/u01/app/oracle; export ORACLE_BASE
ORACLE_HOME=$ORACLE_BASE/product/12.1.0/dbhome_1; export ORACLE_HOME
ORACLE_SID=orcl; export ORACLE_SID
PATH=$ORACLE_HOME/bin:$PATH; export PATH
LD_LIBRARY_PATH=$ORACLE_HOME/lib:/lib:/usr/lib:/usr/lib64; export LD_LIBRARY_PATH
CLASSPATH=$ORACLE_HOME/jlib:$ORACLE_HOME/rdbms/jlib; export CLASSPATH
重新装入 bash_profile 来运用新设置值：

[oracle@centos7 ~]$ . .bash_profile
5.3. 登录数据库
最后请登录数据库：

[oracle@centos7 ~]$ sqlplus system@orcl
... output omitted ...
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With the Partitioning, OLAP, Advanced Analytics and Real Application Testing options
SQL>
请利用 Oracle 企业级管理员来管理数据库：

https://<主机名称>:5500/em



### Xming —— 从 Windows 系统管理图像化的 Linux 应用程序

Contents

关于 Xming
先决条件
安装
如何应用 Xming

1. 关于 Xming
   Xming 是一个在 Microsoft Windows 计算机上运行的开源 X-Windows 终端機仿真器（X 服务器）。Xming 容让 Windows 机器显示在远程 Linux 服务器上执行的图像化 Linux 程序。除了基本安装程序外，本文章亦示范如何利用 PuTTY SSH 客端程序保障 Xming 下的 X-Window 工作阶段。
2. 先决条件
   远程的 CentOS 服务器上必须安装了 X Window System 组件群组。

[root@centos7 ~]# yum groupinstall "X Window System" -y 
X Window 系统采用主从结构。X 服务器通过网络或本地回送界面听候来自 X 客端应用程序的连接。服务器与图像卡、显示屏、键盘、鼠标等硬件沟通。X 客端应用程序在用户空间内运行，它为用户创建一个图像化界面，并将用户的要求递交 X 服务器。

3. 安装
   Xming 安装程序适用于 Windows 10／8／7／Vista／XP 桌面及 Windows Server 2012／2008／2003。

步骤：

利用下列连结从 Sourceforge 下载最新版的 Xming 安装程序。
http://sourceforge.net/projects/xming/files/latest/download

待安装程序完全下载至桌面后，连按 Xming 的图示两次来开始安装
在 Welcome to the Xming Setup Wizard 画面按 Next

接纳 C:\Program Files\Xming 或浏览另一个目录作为安装的目的地。请按 Next 继续* 被问及需要安装的组件时，接纳缺省值并按 Next

在 Select Additional Tasks 画面，选择桌面图示等额外任务并按 Next

下一个画面显示所有安装设置。如果一切正碓无误，请按 Install

在 Completing the Xming Setup Wizard 窗口内按 Finish

你的 Windows 桌面现在已准备好显示远程的 X11 图像化应用程序。

4. 如何应用 Xming
   顺利完成安装后，你的 Windows 计算机将会出现一个 Xming 图示在桌面。连按它两次便会引导 X11 服务器并让你的计算机成为一台 X 服务器。当 X11 服务器在运作时，一个 X 图示会出现在你的工作列里。

步骤：

连按 Xming 的图示两次来引导 Xming
打开 PuTTY 的连接设置窗口（引导 Putty）
在 PuTTY 的设置窗口，选择 Connection --> SSH --> X11

请勾选 Enable X11 forwarding 选项

返回 Session 类型，指定你要连接的主机名称或 IP 地址
在 Saved Sessions 下指定一个合适的名称并存储连接 —— 或 —— 直接按 Open 来连接至所指定的 CentOS 计算机。

利用 xeyes、xterm 或其它图像化应用程序（xorg-x11-apps）来测试 X11 转接。

[root@centos7 ~]# yum install xeyes -y 

[root@centos7 ~]# xeyes 



启动
1.#su - oracle 切换到 oracle 用户且切换到它的环境
2.$lsnrctl status 查看监听及数据库状态
3.$lsnrctl start 启动监听
4.$sqlplus / as sysdba 以 DBA 身份进入 sqlplus
5.SQL>startup 启动 db

 停止
1.#su - oracle 切换到 oracle 用户且切换到它的环境
2.$lsnrctl stop 停止监听
3.$sqlplus / as sysdba 以 DBA 身份进入 sqlplus

4.SQL>SHUTDOWN IMMEDIATE 关闭 db





### 注意点

1.在检查先决条件时，选择忽略全部
2.在安装时需要一台有图形界面的linux
3.默认登录到root用户更好使用
4.注意执行脚本
5.创建密码注意，大写字母，小写字母，和数字



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