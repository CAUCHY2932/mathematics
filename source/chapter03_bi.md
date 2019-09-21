# bi

## superset


### superset执行命令

This project is unofficial and not related to Superset or Apache.
```bash
docker search superset # docker 查找镜像
docker pull amancevice/superset # 拉取镜像
docker images # 查看镜像
docker run -d -p 8087:8088 -v /Users/young/super:/home/superset amancevice/superset # -v指定本地目录，冒号后面为容器内自动创建的目录
docker exec -it bi superset-init # bi替换为id或是容器名，初始化superset
docker exec -it bi superset load_examples # 载入示例数据（可选）
```

## github docker介绍

### Issues

Please **only** file issues in this project that are related to Docker and **do** include the Docker commands or compose configuration of your setup when filing issues (be sure to hide any secrets/passwords before submitting).

File issues/bugs with Superset at the [source](https://github.com/apache/incubator-superset/issues).

Please **do not** files issues like "Please include `<some-python-pip>` in the Dockerfile," open a [pull request](https://github.com/amancevice/superset/pulls) for updates/enhancements.


### Examples

Navigate to the `examples` directory to view examples of how to configure Superset with MySQL, PostgreSQL, or SQLite.

### Versions

This repo is tagged in parallel with superset. Pulling `amancevice/superset:0.18.5` will fetch the image of this repository running superset version `0.18.5`. It is possible that the `latest` tag includes new features/support libraries but will usually be in sync with the latest semantic version.


### Configuration

Follow the [instructions](https://superset.incubator.apache.org/installation.html#configuration) provided by Apache Superset for writing your own `superset_config.py`. Place this file in a local directory and mount this directory to `/etc/superset` inside the container. This location is included in the image's `PYTHONPATH`. Mounting this file to a different location is possible, but it will need to be in the `PYTHONPATH`.

View the contents of the `examples` directory to see some simple `superset_config.py` samples.


### Volumes

The image defines two data volumes: one for mounting configuration into the container, and one for data (logs, SQLite DBs, &c).

The configuration volume is located alternatively at `/etc/superset` or `/home/superset`; either is acceptable. Both of these directories are included in the `PYTHONPATH` of the image. Mount any configuration (specifically the `superset_config.py` file) here to have it read by the app on startup.

The data volume is located at `/var/lib/superset` and it is where you would mount your SQLite file (if you are using that as your backend), or a volume to collect any logs that are routed there. This location is used as the value of the `SUPERSET_HOME` environmental variable.

### Database Initialization

After starting the Superset server, initialize the database with an admin user and Superset tables using the `superset-init` helper script:

```bash
docker run --detach --name superset [options] amancevice/superset
docker exec -it superset superset-init
```


### Upgrading

Upgrading to a newer version of superset can be accomplished by re-pulling `amancevice/superset`at a specified superset version or `latest` (see above for more on this). Remove the old container and re-deploy, making sure to use the correct environmental configuration. Finally, ensure the superset database is migrated up to the head:

```bash
# Pull desired version
docker pull amancevice/superset

# Remove the current container
docker rm -f superset-old

# Deploy a new container ...
docker run --detach --name superset-new [options] amancevice/superset

# Upgrade the DB
docker exec superset-new superset db upgrade

# Sync the base permissions
docker exec superset-new superset init
```

##  linux上利用docker安装superset

>https://blog.csdn.net/m0_37805167/article/details/81069644

### 拉取

1.docker search superset

2.docker pull amancevice/superset

docker images  查看镜像

### superset的使用

**创建本地目录**（让容器中的superset挂载本地机的配置文件）

**开启docker容器内的superset应用，开启的同时进行端口映射，并挂载宿主机的数据**

docker ps  查**看开启的应用** 


**设定superset的用户名和密码** 
docker exec -it 容器ID  fabmanager create-admin –app superset 

**初始化数据库** 
docker exec -it 容器ID superset db upgrade



**创建默认角色和许可** 
 docker exec -it 容器ID superset init

**启动服务** 
 docker exec -it  容器ID superset runserver

## Docker容器中搭建superset

> https://blog.51cto.com/1
>
> 4033037/2329635

### 1.创建本地映射文件夹。

    mkdir -p opt\docker\superset

### 2.创建superset容器

    docker run -d -p 8087:8088 -v /opt/docker/superset:/home/superset amancevice/superset
    将主机的8087端口映射到容器的8088端口，同时将主机的/opt/docker/superset目录映射到容器的/home/superset目录。

### 3.配置用户名和密码。

    docker exec -it 420 fabmanager create-admin --app superset


备注：420为容器ID前三位。

### 4.初始化数据库

    docker exec -it 420 superset db upgrade

### 5.初始化superset

    docker exec -it 420 superset init

### 6.开启superset服务

    docker exec -it 420 superset runserver

### 7.访问superset

在本地浏览器地址栏输入下面的地址即可访问superset。8087为创建容器时映射的主机端口。

http://localhost:8087





## superset docker部署

> https://mp.weixin.qq.com/s?__biz=MzUyNzk4MjA5NQ==&mid=2247483743&idx=1&sn=9ccec34faf307aab2868faca6a3fdabc&chksm=fa760a3fcd018329f618320f7e4f51e01c82e739ed02817df377c7f76c39c6a07bf8f8049be8&token=1986779450&lang=zh_CN#rd



> https://www.cnblogs.com/vickey-wu/p/10205031.html

### 一、使用自己的数据库

#### 1. 拉取项目

```
// 创建目录用于存放项目
mkdir -p /mnt/superset
cd /mnt/superset
git clone https://github.com/amancevice/superset.git
```

#### 2. 配置数据库等

> 这里默认你已创建了你自己的空数据库和具有读写该数据库权限的用户，到下面初始化时会自动在你的数据库创建表结构用于导入你的数据。如果没有可以使用项目自带的demo数据库

```
进入项目目录
cd /mnt/superset/superset
按照官网文档填写配置信息
```

- `superset_config.py`[link](https://superset.incubator.apache.org/installation.html#configuration)

```
ROW_LIMIT = 5000

SUPERSET_WEBSERVER_PORT = 8088

SECRET_KEY = 'set_your_own_key'

SQLALCHEMY_DATABASE_URI = 'mysql://user:pass@host:port/db'


# Flask-WTF flag for CSRF
WTF_CSRF_ENABLED = True
# Add endpoints that need to be exempt from CSRF protection
WTF_CSRF_EXEMPT_LIST = []
# A CSRF token that expires in 1 year
WTF_CSRF_TIME_LIMIT = 60 * 60 * 24 * 365

# Set this API key to enable Mapbox visualizations
MAPBOX_API_KEY = ''
```

#### 3. 启动容器

> 注意：
>
> 1. -v 挂载配置文件必须挂载到容器的/etc/superset/superset_config.py或者/home/superset/superset_config.py，因为容器里面的环境变量是这两个，挂载到其他路径初始化数据库会不生效。
> 2. SECRET_KEY必须与superset_config.py的设置一致
> 3. 填写你自己数据库连接信息

```
docker run -d --name superset_name \
    --env SECRET_KEY="set_your_own_key" \
    --env SQLALCHEMY_DATABASE_URI="mysql://user:pass@host:port/db" \
    -p 8089:8088 \
    amancevice/superset
```

#### 4. 初始化容器

```
进入superset-init文件目录
cd /mnt/superset/superset/superset
初始化
docker exec -it superset_name superset-init
输入你设置登录superset前端的admin相关信息
Username [admin]: admin
User first name [admin]: vickey
User last name [user]: vickey
password: mypassword
repeat passwd: mypassword
输入完毕开始初始化，等待完成即可
```

#### 5.前端访问

```
http://ip:8088/
```

### 二、使用项目demo数据库

```
启动容器（假设我们创建了/mnt/superset）
cd /mnt/superset/
git clone https://github.com/amancevice/superset.git
cd superset
docker-compose up -d
docker-compose exec superset demo
前端访问
http://ip:8088/
```

### 三、参考链接

- [项目教程链接](https://github.com/amancevice/superset/blob/master/README.md)
- [配置文件链接](https://superset.incubator.apache.org/installation.html#configuration)
- [他人教程链接](https://devhub.io/repos/amancevice-superset)





## superset python环境配置

pip install -i https://pypi.doubanio.com./simple superset

容易失败

https://www.jianshu.com/p/5bbff5a83dd1





## 安装docker



sudo apt-get install docker.io

## 拉取镜像

sudo docker search superset

sudo docker pull amancevice/superset

## 创建启动文件夹

mkdir /opt/docker/superset

## 查看docker启动状态

sudo service docker start| stauts

## 进入命令行

然后我们使用docker ps查看到该容器信息，接下来就使用docker attach进入该容器

1. $ sudo docker attach 44fc0f0582d9 

> https://www.jianshu.com/p/e1d3e25abcba

您可以使用attach命令在docker容器中获取bash shell访问。但是您的docker容器必须以/ bin / bash启动。 使用以下语法来获取docker容器的shell访问。$ sudo docker attach <CONTAINER ID/NAME>

## docker exec

- docker exec -it {docker_id/name} /bin/bash：进入后台运行的容器的交互模式

## docker root用户进入命令行界面

exec -it --user root <container id> /bin/bash

apt-get update

apt-get isntall vim

```bash
docker exec -u 0 -it superset /bin/bash
```

0号用户就是root用户, 剩下来的就简单了



## 初始化载入样例数据

sudo docker exec -it bi superset-init

sudo docker exec -it bi superset load_examples



## 文件目录

which python

查找python的执行路径

python

查找版本

确定执行路径的包的路径



/usr/local/lib/python3.6/site-packages/superset

```
➜  ~ docker search superset
NAME                               DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
amancevice/superset                [0.26.3] Superset on Debian+Python3             129                                     [OK]
tylerfowler/superset               An extendable Docker image for Airbnb's Supe…   3

➜  ~ docker pull amancevice/superset
Using default tag: latest
latest: Pulling from amancevice/superset
0bd44ff9c2cf: Pull complete
2576b77b8a87: Pull complete
4d2fed87d171: Pull complete
Digest: sha256:d85229d3bae54cc5281e52d615a0c4752bfefa0ae62c754c598411b4f8e529cd
Status: Downloaded newer image for amancevice/superset:latest

➜  ~ mkdir /opt/docker/superset/conf & mkdir /opt/docker/superset/data

➜  ~ docker run --name superset -u 0 -d -p 8088:8088 -v /opt/docker/superset/conf:/etc/superset -v /opt/docker/superset/data:/var/lib/superset amancevice/superset
aa3fc2b4aadeecc949771844384110e7813ee028cb38bf21d8bbd73fc54c71a5

➜  ~ docker exec -it superset superset-init
Username [admin]: admin
User first name [admin]: admin
User last name [user]: admin
Email [admin@fab.org]: admin@qq.com
Password:
Repeat for confirmation:
Recognized Database Authentications.
Admin User admin created.
```

作者：adeng2016

链接：https://www.jianshu.com/p/e1d3e25abcba

来源：简书

简书著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。



```
#---------------------------------------------------------
# Superset specific config
#---------------------------------------------------------
ROW_LIMIT = 5000

SUPERSET_WEBSERVER_PORT = 8088
#---------------------------------------------------------

#---------------------------------------------------------
# Flask App Builder configuration
#---------------------------------------------------------
# Your App secret key
SECRET_KEY = '\2\1thisismyscretkey\1\2\e\y\y\h'

# The SQLAlchemy connection string to your database backend
# This connection defines the path to the database that stores your
# superset metadata (slices, connections, tables, dashboards, ...).
# Note that the connection information to connect to the datasources
# you want to explore are managed directly in the web UI
SQLALCHEMY_DATABASE_URI = 'sqlite:////var/lib/superset/superset.db'

# Flask-WTF flag for CSRF
WTF_CSRF_ENABLED = True
# Add endpoints that need to be exempt from CSRF protection
WTF_CSRF_EXEMPT_LIST = []
# A CSRF token that expires in 1 year
WTF_CSRF_TIME_LIMIT = 60 * 60 * 24 * 365

# Set this API key to enable Mapbox visualizations
MAPBOX_API_KEY = ''
```



`SECRET_KEY`注意在容器启动前就设置好，值好像跟某些元数据有关系，笔者在第一次启动后修改重启容器，遇到数据源无法查看的问题。

`MAPBOX_API_KEY`是支持地图类报表的服务提供商[mapbox](https://www.mapbox.com/)的密钥，注册一个account后可得，如下图：

样例数据

```
 docker exec -it superset superset load_examples
```

## 启动命令

sudo docker run -d -p 8087:8088 -v /opt/docker/superset:/home/superset amancevice/superset

sudo docker run --detach --name bi -d -p 8087:8088 -v /opt/docker/superset:/home/superset amancevice/superset

## 查看启动的镜像

sudo docker images

## 查看启动的容器



sudo docker ps -a

查看镜像前三位id



## 创建用户和密码

sudo docker exec -it id fabmanager create-admin --app superset

## 初始化数据库

sudo docker exec -it 容器ID superset db upgrade

## 浏览器访问

http://ip:8088/

## 停止容器

sudo docker stop xxx（id）

## 文件配置

${home}/superset_config.py

## 查看docker日志

docker logs xx

```bash
python superset db upgrade
python superset load_examples
python superset init
python superset runserver
```

**创建默认角色和许可** 
docker exec -it 容器ID superset init

## 创建用户时出错



```
docker exec -it superset superset-init
```

## 移除应用容器

```
sudo docker ps -a
sudo docker rm 3b9
```

## 重启应用容器

## 重启WEB应用容器

已经停止的容器，我们可以使用命令 docker start 来启动。

```bash
runoob@runoob:~$ docker start wizardly_chandrasekhar
```

## 官方docker superset命令

## docker安装

## 搜索镜像

## docker拉取镜像

## docker运行镜像指定目录

docker初始化配置

账号密码

初始样例



我使用docker进行安装, 本以为很简单, 中间还是遇到一些坑.

首先安装docker
创建相关目录

mkdir /dockerfs/superset/conf -p
mkdir /dockerfs/superset/data -p


创建容器

docker run -p 8088:8088 -v /dockerfs/superset/conf:/etc/superset -v mkdir /dockerfs/superset/data:/data  --name superset -d amancevice/superset:0.18.5


使用配置文件

vi /dockerfs/superset/conf/superset_config.py

输入内容
```bash
#---------------------------------------------------------
## Superset specific config

#---------------------------------------------------------
ROW_LIMIT = 5000
SUPERSET_WORKERS = 4

SUPERSET_WEBSERVER_PORT = 8088

### Flask App Builder configuration

# Your App secret key
SECRET_KEY = '\2\1thisismyscretkey\1\2\e\y\y\h'

# The SQLAlchemy connection string to your database backend
# This connection defines the path to the database that stores your
# superset metadata (slices, connections, tables, dashboards, ...).
# Note that the connection information to connect to the datasources
# you want to explore are managed directly in the web UI
SQLALCHEMY_DATABASE_URI = 'sqlite:////data/superset.db'

# Flask-WTF flag for CSRF
WTF_CSRF_ENABLED = True

# Set this API key to enable Mapbox visualizations
MAPBOX_API_KEY = ''
```
问题就出现在sqlite的路径上, sqlite默认存储在sqlite:////home/superset/.superset/superset.db, 我这里为了以后升级, 所以切换了存储路径, 这里有两种做法

直接将/home/superset/.superset/路径映射出来
将/home/superset/.superset/superset.db文件拷贝到/data目录

我这里选择的是第二种, 坑也在这, 使用
docker exec -it superset /bin/bash
cp /home/superset/.superset/superset.db /data

失败, 发现没有权限, ls了一下才发现当前用户是非root用户, 而/data目录是root权限.
经过一番查找, 发现可以使用以下命令用root账号登陆容器
docker exec -u 0 -it superset /bin/bash

0号用户就是root用户, 剩下来的就简单了
mv /home/superset/.superset/superset.db /data


退出容器, 重启容器, 然后进行用户初始化

docker restart superset 
docker exec -it superset superset-init

作者：一根弦的风筝
链接：https://www.jianshu.com/p/a6fe79d0b1b3
来源：简书
简书著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。





## **Ubuntu 16.04 安装 Docker**

1.选择国内的云服务商，这里选择阿里云为例

```
curl -sSL http://acs-public-mirror.oss-cn-hangzhou.aliyuncs.com/docker-engine/internet | sh -
```

2.安装所需要的包

```
sudo apt-get install linux-image-extra-$(uname -r) linux-image-extra-virtual
```

3.添加使用 HTTPS 传输的软件包以及 CA 证书

```
sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates
```

4.添加GPG密钥

```
sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
```

5.添加软件源

```
echo "deb https://apt.dockerproject.org/repo ubuntu-xenial main" | sudo tee /etc/apt/sources.list.d/docker.list
```

6.添加成功后更新软件包缓存

```
sudo apt-get update
```

7.安装docker

```
sudo apt-get install docker-engine
```

8.启动 docker

```
sudo systemctl enable docker
sudo systemctl start docker
```





## **Ubuntu 18.04 安装 Docker-ce**

1.更换国内软件源，推荐中国科技大学的源，稳定速度快（可选）

```
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
sudo sed -i 's/archive.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
sudo apt update
```

2.安装需要的包

```
sudo apt install apt-transport-https ca-certificates software-properties-common curl
```

3.添加 GPG 密钥，并添加 Docker-ce 软件源，这里还是以中国科技大学的 Docker-ce 源为例

```
curl -fsSL https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu \
$(lsb_release -cs) stable"
```

4.添加成功后更新软件包缓存

```
sudo apt update
```

5.安装 Docker-ce

```
sudo apt install docker-ce
```

6.设置开机自启动并启动 Docker-ce（安装成功后默认已设置并启动，可忽略）

```
sudo systemctl enable docker
sudo systemctl start docker
```

7.测试运行

```
sudo docker run hello-world
```

8.添加当前用户到 docker 用户组，可以不用 sudo 运行 docker（可选）

```
sudo groupadd docker
sudo usermod -aG docker $USER
```

9.测试添加用户组（可选）

```
docker run hello-world
```





https://blog.csdn.net/dzhmjl3/article/details/87877662

## 27版本

https://github.com/apache/incubator-superset/commit/7a7c12be4cede48dcc8349ad90f0209a9a201ef3

```bash
git reset --hard 7a7c12be4cede48dcc8349ad90f0209a9a201ef3
```

## 二次开发

superset\assets> npm install echarts

pscp.ext -r xx:xx  ./

## redash


docker pull redash/redash

https://redash.io/help/open-source/dev-guide/docker

https://hub.docker.com/r/redash/redash

https://www.cnblogs.com/rongfengliang/p/9901464.html

```yaml
version: '2'
x-redash-service: &redash-service
  image: redash/redash:7.0.0.b18042
  depends_on:
    - postgres
    - redis
  env_file: /opt/redash/env
  restart: always
services:
  server:
    <<: *redash-service
    command: server
    ports:
      - "5000:5000"
    environment:
      REDASH_WEB_WORKERS: 4
  scheduler:
    <<: *redash-service
    command: scheduler
    environment:
      QUEUES: "celery"
      WORKERS_COUNT: 1
  scheduled_worker:
    <<: *redash-service
    command: worker
    environment:
      QUEUES: "scheduled_queries,schemas"
      WORKERS_COUNT: 1
  adhoc_worker:
    <<: *redash-service
    command: worker
    environment:
      QUEUES: "queries"
      WORKERS_COUNT: 2
  redis:
    image: redis:5.0-alpine
    restart: always
  postgres:
    image: postgres:9.5-alpine
    env_file: /opt/redash/env
    volumes:
      - /opt/redash/postgres-data:/var/lib/postgresql/data
    restart: always
  nginx:
    image: redash/nginx:latest
    ports:
      - "80:80"
    depends_on:
      - server
    links:
      - server:redash
restart: always
```

## 二、docker-compose安装

#### 1，下载docker-compose

```
$ sudo curl -L https://github.com/docker/compose/releases/download/1.17.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
```

#### 2，授权

```
$ sudo chmod +x /usr/local/bin/docker-compose
```

#### 3，查看版本信息

```
$ docker-compose --version
显示出版本信息，即安装成功。
```

https://ywnz.com/linuxysjk/4254.html

```yaml
# This is an example configuration for Docker Compose. Make sure to atleast update
# the cookie secret & postgres database password.
#
# Some other recommendations:
# 1. To persist Postgres data, assign it a volume host location.
# 2. Split the worker service to adhoc workers and scheduled queries workers.
version: '2'
services:
 server:
   image: redash/redash:latest
   command: server
   depends_on:
     - postgres
     - redis
   ports:
     - "5000:5000"
   environment:
     PYTHONUNBUFFERED: 0
     REDASH_LOG_LEVEL: "INFO"
     REDASH_REDIS_URL: "redis://redis:6379/0"
     REDASH_DATABASE_URL: "postgresql://postgres@postgres/postgres"
     REDASH_COOKIE_SECRET: veryverysecret
     REDASH_WEB_WORKERS: 4
   restart: always
 worker:
   image: redash/redash:latest
   command: scheduler
   environment:
     PYTHONUNBUFFERED: 0
     REDASH_LOG_LEVEL: "INFO"
     REDASH_REDIS_URL: "redis://redis:6379/0"
     REDASH_DATABASE_URL: "postgresql://postgres@postgres/postgres"
     QUEUES: "queries,scheduled_queries,celery"
     WORKERS_COUNT: 2
   restart: always
 redis:
   image: redis:3.0-alpine
   restart: always
 postgres:
   image: postgres:9.5.6-alpine
   # volumes:
   #   - /opt/postgres-data:/var/lib/postgresql/data
   restart: always
 nginx:
   image: redash/nginx:latest
   ports:
     - "80:80"
   depends_on:
     - server
   links:
     - server:redash
   restart: always
```

## docker 安装metabase

```bash
docker run -d -p 3000:3000 --name metabase metabase/metabase
```

