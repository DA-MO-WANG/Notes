###### 基础概念

​		docker技术的基础是容器与虚拟化技术。
​		虚拟化技术是指，对实体物理资源进行抽象，然后转换为虚拟资源来面向应用方
​		容器技术是一种具体的虚拟化技术，指隔离的运行环境
​		垫脚石：虚拟机产品——》减少了虚拟操作系统和虚拟监视器两个层次的感念
​		

​		docker的实现底层：
​				三大技术基础：
​						命名空间namespace :将资源切割成虚拟独立空间，彼此隔离
​						控制组 control groups：硬件资源分配
​						联合文件系统 UFS
​				四大组成对象：
​						镜像images：只读的虚拟文件包，成层次的结构，每次更新时只会增加，不会更改
​						容器container：一种活的空间
​						网络network：通过restapi来网络通讯进而信息交互
​						数据卷volume：文件目录挂载
​				产品组成：
​						dock er engine = docker daemon + CLI. ,c/s架构

###### 镜像images

​		dock er镜像只允许自己打包，自己导出，自己下载。
​		dock er每一个镜像信息，有全球唯一的64位hash码来标识
​		镜像的命名逻辑：username （谁发布的）+ repository（软件名） + tag（版本号）
​		镜像的生命周期：
​				核心状态：created-- running--- passed--- stopped---deleted![image-20210819132456539](../README.assets/image-20210819132456539.png)

​				主进程与容器的关系：
​						容器的兴衰和容器内pid编号为1的进程绑在一起的。所以推荐一个容器对应一个程序。这样容器关闭指令也会传到主进程停止。
​				写时复制机制
​						写时复制，比如两个数组拷贝，并非立即在内存中执行复制操作，而是先让新饮用指向旧内存空间上。等到旧引用发生修改时，再执行真正的复制操作。docker也是类似，docker启动初始化时，并非立即把镜像复制到沙盒环境，而是先把镜像挂到沙盒中。等容器涉及对文件修改时，才把修改体现到沙盒环境。这样就加快了启动速度

###### 镜像仓库

​		仓库的价值：除了存储镜像，还有分发镜像，也就是可以供不同环境上传和拉取（事情变得简单，是因为把不简单的部分变得不可见就行）
​		docker hub ：中央镜像仓库，默认仓库地址

###### 配置镜像源

​		配置docker-engine的json文件：增加一行 registry mirrors

###### 容器网络配置

​		容器网络从大面讲就是，从真实的网络环境中，独立出一套自有的网络设备
​		容器网络模型：
​				沙盒 sandbox：包含了端口套接字、IP路由表、防火墙这些
​				网络 network：虚拟子网
​				端点 endpoint：出入口
​		五种网络驱动：
​				bridge driver(多)：网桥实现网络通讯
​				host driver
​				overlay driver(多)：借助集群模块docker. swarm来搭建
​				maclan driver
​				none driver
​		容器互联：两个容器连接互动起来
​				容器间的网络互通：
​				暴露端口：在容器创建时 --expose  端口号
​				别名来连接	
​				

###### 端口映射

​		容器外通过网络访问容器中的应用？
​				

​	

###### docker常用指令

docker images   ——》查看本地所有镜像
docker pull  镜像名 ——〉拉取指定镜像（不完全，就拉取最新版本）
docker search  镜像名——》查看该镜像的所有版本
Docker  inspect 镜像名——〉 查看镜像的详细信息
docker  rmi  镜像名——》 删除指定镜像
Docker info. ——〉查看docker信息
Docker ps ——》查看dock er暴露的端口
Docker network create  (-d 指定类型)——〉创建网络
Docker network ls ——》查看已经存在的网络