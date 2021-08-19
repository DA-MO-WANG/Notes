###### 基础概念

​		docker技术的基础是容器与虚拟化技术。</br>
​		虚拟化技术是指，对实体物理资源进行抽象，然后转换为虚拟资源来面向应用方</br>
​		容器技术是一种具体的虚拟化技术，指隔离的运行环境</br>
​		垫脚石：虚拟机产品——》减少了虚拟操作系统和虚拟监视器两个层次的感念</br>
​		

​		docker的实现底层：</br>
​				三大技术基础：</br>
​						命名空间namespace :将资源切割成虚拟独立空间，彼此隔离</br>
​						控制组 control groups：硬件资源分配</br>
​						联合文件系统 UFS</br>
​				四大组成对象：</br>
​						镜像images：只读的虚拟文件包，成层次的结构，每次更新时只会增加，不会更改</br>
​						容器container：一种活的空间</br>
​						网络network：通过restapi来网络通讯进而信息交互</br>
​						数据卷volume：文件目录挂载</br>
​				产品组成：</br>
​						dock er engine = docker daemon + CLI. ,c/s架构</br>

###### 镜像images

​		dock er镜像只允许自己打包，自己导出，自己下载。</br>
​		dock er每一个镜像信息，有全球唯一的64位hash码来标识</br>
​		镜像的命名逻辑：username （谁发布的）+ repository（软件名） + tag（版本号）</br>
​		镜像的生命周期：</br>
​				核心状态：created-- running--- passed--- stopped---deleted</br>![image-20210819132456539](../README.assets/image-20210819132456539.png)

​				主进程与容器的关系：</br>
​						容器的兴衰和容器内pid编号为1的进程绑在一起的。所以推荐一个容器对应一个程序。这样容器关闭指令也会传到主进程停止。</br>
​				写时复制机制</br>
​						写时复制，比如两个数组拷贝，并非立即在内存中执行复制操作，而是先让新饮用指向旧内存空间上。等到旧引用发生修改时，再执行真正的复制操作。docker也是类似，docker启动初始化时，并非立即把镜像复制到沙盒环境，而是先把镜像挂到沙盒中。等容器涉及对文件修改时，才把修改体现到沙盒环境。这样就加快了启动速度</br>

###### 镜像仓库

​		仓库的价值：除了存储镜像，还有分发镜像，也就是可以供不同环境上传和拉取（事情变得简单，是因为把不简单的部分变得不可见就行）</br>
​		docker hub ：中央镜像仓库，默认仓库地址</br>

###### 配置镜像源

​		配置docker-engine的json文件：增加一行 registry mirrors

###### 容器网络配置

​		容器网络从大面讲就是，从真实的网络环境中，独立出一套自有的网络设备</br>
​		容器网络模型：</br>
​				沙盒 sandbox：包含了端口套接字、IP路由表、防火墙这些</br>
​				网络 network：虚拟子网</br>
​				端点 endpoint：出入口</br>
​		五种网络驱动：</br>
​				bridge driver(多)：网桥实现网络通讯</br>
​				host driver</br>
​				overlay driver(多)：借助集群模块docker. swarm来搭建</br>
​				maclan driver</br>
​				none driver</br>
​		容器互联：两个容器连接互动起来</br>
​				容器间的网络互通：</br>
​				暴露端口：在容器创建时 --expose  端口号</br>
​				别名来连接	</br>
​				

###### 端口映射

​		容器外通过网络访问容器中的应用？</br>
​				一条命令，在创建容器时：-p 宿主机端口：容器端口</br>
​				非Linux平台：还要再搭一层映射，这个由desktop自动完成</br>

###### 数据存储、管理

​			背景：容器是隔离的，docker中文件如何做到与外界数据进行交互？uni o nFS文件系统，支持不同文件系统挂载在同一目录结构</br>
​			挂载方式：
​					bind mount: 把宿主的目录和文件挂到容器系统中</br>
​					volume：同样，由docker管理</br>
​					t mpfs mount : 挂载内存到文件系统</br>

###### 镜像生成、迁移

​		容器修改后，把沙盒环境持久化成一个镜像文件：</br>
​				指令：docker commit  原镜像名（类比git提交代码）</br>
​		为镜像命名：～  （-m 提交注释） 原镜像名 新镜像名</br>
​		把镜像输出到外部：</br>
​				docker save   -o  ./输出文件名.tar. </br>
​				或者：docker export -o  ./ 输出文件名.tar.  新镜像命名</br>
​		从外部导入镜像到容器：</br>
​				docker load  -i 外部文件名</br>
​				或者：docker import ./ 输出文件名.tar.  新镜像命名</br>

###### 基于镜像构建定义文件dockerfile 的镜像迁移

​		docker file 简单体积小，所以在网络传输很快，所以方便容器迁移
​		dockerfile 本质就是一个普通文本文件，但有一套自己独有的指令语法：注释行+指令行（指令+参数）
​		docker file 实现了自动化构建环境体系，记录了完整的环境搭建流程逻辑
​		docker file 相当于一个配置文件，docker会根据这个配置文件来构建镜像						







###### docker常用指令

docker images   ——》查看本地所有镜像</br>
docker pull  镜像名 ——〉拉取指定镜像（不完全，就拉取最新版本）</br>
docker search  镜像名——》查看该镜像的所有版本</br>
Docker  inspect 镜像名——〉 查看镜像的详细信息</br>
docker  rmi  镜像名——》 删除指定镜像</br>
Docker info. ——〉查看docker信息</br>
Docker ps ——》查看dock er暴露的端口</br>
Docker network create  (-d 指定类型)——〉创建网络</br>
Docker network ls ——》查看已经存在的网络</br>
docker save  -o.  ./文件名.tar. ——〉镜像输出到外部
docker  load  -i  外部文件名.tar  ——》文件导入容器内部