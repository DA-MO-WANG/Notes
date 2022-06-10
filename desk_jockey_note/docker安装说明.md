# docker安装说明

1.要安装的是docker-ce

2.离线情况-手动下载rpm; 有网情况，利用yum下载

3.如何利用yum下载？

- ​	下载依赖包				

```shell
sudo yum install -y yum-utils
```

- 添加docker版本仓库

  ```shell
  sudo yum-config-manager \
  	--add-repo \
   	https://download.docker.com/linux/centos/docker-ce.repo
  ```

- 下载最新版本

```shell
yum install docker-ce docker-ce-cli containers.io
```

- 安装指定版本

```shell
yum list docker-ce —showduplicates | sort -r
sudo yum install docker-ce-(版本号：冒号和-之间的数字) docker-ce-cli-版本号 containers.io -y
```

- 改变挂载目录

  ​		默认目录是 /var/lib/docker; 创建目标目录; 创建json文件, 注册阿里云账号，配置镜像加速器

```shell
mkdir -p /etc/docker
vi /etc/docker/daemon.json
{
	"registry-mirrors":[
		"https://ot2k4d59.mirrors.aliyuncs.com/"
	],
	"graph":"/data/docker"
}
```

- 设置开机启动，并启动

```shell
systemctl enable docker
systemctl daemon-reload
systemctl start docker
```

- 查看docker相关信息

  ```shell
  docker info
  ```

  - 拉取镜像

    ```shell
    docker pull 镜像名
    ```

    ​		镜像名的构造：3部分
    ​				域名 ：端口号（不写默认是官方仓库）

    ​				/仓库名 ：用户名/软件名（用户名/不写默认是library）
    ​				:标签  ：			

- 运行容器

  ```shell
  docker run 镜像
  ```

  整理镜像时，想删除不需要的镜像：

  ```shell
  docker rmi -f 镜像名
  ```

  给自己的镜像打标签：
  
  ```shell
  docker tag 原镜像名  新镜像名
  ```
  
  没网给自己的镜像打包
  
  ```shell
  docker save 镜像名 > 路径/镜像.tar.gz
  ```
  
  没网导入镜像
  
  ```shell
  docker load < 路径/镜像.tar.gz
  ```



//容器的增删查  暂时的启动与停止

从镜像到容器

```shell
docker run 镜像名
```

docker运行的流程：
		先去本地看一下有没有现成的镜像，没有的话再找公有仓库
		基于镜像去创建并启动一个容器
		分配文件系统，在只读的镜像层外挂一层可读写层
		分配网络虚拟接口
		分配IP地址
		

看一下容器运行列表

```shell
docker ps 
```

​			-a 已经退出的也显示
删除容器

```shell
docker rm -f 容器ID
```

让容器停下来

```shell
docker stop 容器唯一标识
```

让停下来的容器再跑起来

```
docker start 容器id（容器id可在ps -a 中看到）
```

#### 容器网络相关

###### 端口映射

如果是容器要与外网互动：端口映射 ==》 iptable dnat
		流量走向：brower ->  物理端（端口） -> 容器端口 ->  容器应用程序
	

```
docker run --name 别名 -d （-p 物理机端口:容器端口）  应用程序
```

##### 容器网络的几种模式

###### Bridge模式

​		docker在主机启动时，会产生一个虚拟网桥docker0
​		这个虚拟网桥的作用就类似机房中网络结构中的物理交换机--》机房的网络机构照搬到内部当中
​		每个容器--〉相当于机房中的机器
​		容器启动，就会产生一对网卡，一端etho插到容器这边；另一端veth0插到宿主机上

###### 自定义模式

​		区分 --link下的网络连接，--lin k单向连接，a 加入到 b，b对a可见通，a对b还是通不了
​		自定义网络，是先创建一个网络，然后其他容器加入到这个网络中

​		容器互联互通的情况：
​				单向的互通：--link

```shell
docket run -tid (--link 被连接方容器名1) --name 连接主动方容器名2 镜像  top
#进入容器内部
docker exec -it 容器名 /bin/sh
```

​				一个网络

```shell
#创建网络
docker network create -d bridge 网络别名
#按照定义的网络来启动容器,进入交互终端
docker run -it --rm --name 容器名 --network 网络名 镜像名 sh
```

###### 		Host模式

​				新创建容器的网络与宿主机共享，ip与端口 --net=host

###### 		Container模式		

​		新创建的容器也是和别人共享网络配置，但不再是宿主机，而是已经存在的容器

```
--net=container:旧容器名
```

##### 容器内的数据管理

​		数据共享和持久化

```shell
在容器启动时，考虑到这个数据是否重要，进而考虑是否持久化,因为一旦容器重启，没有持久化，数据就会消失（比如私有仓库）
```



###### 		数据卷

​				数据卷提供一个挂载点目录，这个目录与容器内部的文件目录映射起来
​				外界通过这个目录可以改变容器内部的数据

```shell
#创建一个数据卷
docker volume create 数据卷名
#查看数据卷
docker volume ls
#查看指定数据卷内部信息
docker volume inspect 数据卷名
#在启动容器时应用数据卷
docker run -d -p 8080:80 --name web -v 数据卷名：容器内部目录  镜像名
#删除不再使用的数据卷
docker volume prune
```

数据卷独立于容器，容器删除后并不会涉及数据卷



###### 挂载主机目录

​		可以做到容器内部访问容器外部的文件

​		

```shell
#启动容器时挂载主机目录 或者 --mount
docker run -it -v 宿主机绝对目录路径：容器绝对目录 镜像名 /bin/sh
#对挂载权限有要求：默认读写改成只读——》在容器目录后加 :ro

#出现操作权限问题-permission denied
#关闭selinux 
临时关闭-》setenforce 0
永久关闭-》修改/etc/sysconfig/selinux 文件，改值selinux 为disabled

特权方式来启动容器: --priviledged=true -》docker run -it --priviledged=true -v /test:/soft centos /bin/sh

```

##### 自己打包镜像

​		dockerfile文件和docker commit 命令的区别：前者每一次改动以命令的形势保存到文件里，方便管理追溯版本记录，后者则丢失不可回溯·

```shell
#创建一个目录，存放新镜像dockerfile
mkdir -p 文件夹名 && cd 文件夹 && touch dockerfile文件名
#dockerfile中常用指令

#构建的第一步，在一个基础镜像上开始
From 基础镜像名
#空白镜像 scratch
#执行命令
RUN shell脚本指令
#为了减少不必要的层数：使用&&合并指令， 使用\来换行
#因为每一层镜像都会跟随下一层，所以保证在这个镜像上只添加必要的东西，及时删除不必要的部分，所以添上rm 相关的指令
```

​		

​		

```shell
#指定工作目录，不存在会自动创建
workdir  针对容器中的路径
#主机资源复制到容器中
#还可以拿到远程服务器中的资源；当拿到tar文件时，会自动解压，操作解压后的资源
ADD  src路径资源 dest路径资源
COPY 
#关于路径的有一点注意事项
#这里的src路径，指docker build命令构建镜像执行的那个目录--上下文目录
#这里的dest路径，指的是相对于workdir设置的相对路径或者绝对路径
#src路径如果是一个目录，只会把目录下的东西操作过去，不涉及目录本身

```

```shell
#写完dockerfile后，构建镜像
#-t指定镜像名
docker build （-t 镜像名（软件名：版本号）） .
# 末尾点，用来指定上下文目录 build context
#实际build执行时，是在远程的docker daemon引擎中执行，而docker引擎是通过事先上传的上下文目录（这里就要求dockerfile最后放在一个空目录下，不要涉及很多不必要的东西，避免被无意上传），来操作镜像设计的文件的，严格的说，这里都是发生在远程服务器的事

#当dockerfile不放在上下文目录中时
-f ../Dockerfile.Dev参数来指定特定的dockerfile文件
```

##### 推送

```shell
#推送公有仓库
先去注册docker-hub账号，本地登陆
#按照docker-hub的规则标注镜像
docker tag 镜像名  hub账号名/镜像名
#推送
docker push hub账号名/镜像名
```

```shell
#推送私有仓库
#依赖docker的一个registry镜像来实现
docker run -d -p 5000:5000 --name 仓库别名 registry：2
#上传镜像到私有仓库
#先去把镜像打包成完整的名字： 两部分（域名:端口号)+仓库名(用户名/软件名（用户名/不写默认是library）:标签)	
#查看私有仓库的镜像
curl 域名:端口号/v2/_catalog

#本网段其他主机也能推送到这个仓库，修改docker配置来改变这个限制
编辑/etc/docker/daemon.json文件
修改insecure-registries字段

```

###### Dockerfile最佳实践

​		考虑缓存的存在，减少重复创建镜像的性能

```shell
#考虑缓存的存在，减少重复创建镜像的性能
#关闭缓存功能
docker build 时添加参数 --no-cache=true
```

```shell
#多阶段构建理论，按照阶段的变化频率来安排指令顺序
构建依赖工具>依赖>应用
```

```
#对多行参数排序
```

```shell
#Dockerfile指令
From Run WORKDIR COPY ADD
LABEL
EXPOSE 指定端口
ENV  指定环境变量
USER
CMD与ENTRYPOINT
#没有ENTRYPOINT，按CMD
#这两者都存在，cmd作为entrypoint的参数
```

```sql
#如何在docker中模拟window中两个用户登陆同一个数据库实例
#在同一个容器id内，开启两个终端
mysql -h localhost -u 用户名2 -p
```

```sql
#如何在docker内开启一个mysql
docker pull mysql:指定版本号
docker run 
```





