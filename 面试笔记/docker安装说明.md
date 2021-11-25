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
  	—add-repo \
   	https:// [download.docker.com/linux/centos/docker-ce.repo](http://download.docker.com/linux/centos/docker-ce.repo)
  ```

  

sudo yum-config-manager \
 	—add-repo \
 	https:// [download.docker.com/linux/centos/docker-ce.repo](http://download.docker.com/linux/centos/docker-ce.repo)

​	3) 下载最新版本：yum install docker-ce docker-ce-cli [containers.io](http://containers.io)
 \4) 安装指定版本：
 	yum list docker-ce —showduplicates | sort -r：看一下版本列表

​		sudo yum install docker-ce-(版本号：冒号和-之间的数字) docker-ce-cli-版本号 [containers.io](http://containers.io) -y

​	5) 改变挂载目录
 	默认目录是 /var/lib/docker

​		创建目标目录：mkdir -p /etc/docker
 	创建json文件：vi /etc/docker/daemon.json