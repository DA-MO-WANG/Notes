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
		