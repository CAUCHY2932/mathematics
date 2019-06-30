## docker
## daocloud

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