###### kubernetes是什么

​		kubernetes是一个平台。管理容器相关操作的平台。他把过去手动的容器操作转变为自动化处理。
​		传统部署：物理机器成本高、资源分配不好管理
​		虚拟化部署：提高了物理资源使用率。
​		容器化部署：跨平台移植、提高响应速度、各种运维服务
​		kubernetes：负载均衡功能、存储编排、自我修复响应容器状态
​								容器自动化服务平台，手动配置变自动化配置转变（docker管理一些安装环境配置，k8s管理一个个docker）

###### K8s 集群搭建实战

​		方式一：minikube 
​		方式二：kind
​		方式三：kubeadm
​		方式四：二进制包
​		方式五：yum
​		方式六：第三方工具
​		方式七：花钱买服务

​		kubeadm:
​				1.准备物理机环境
​					min 2 core 2g、禁止swap分区、保证能访问外网、机器间通信
​				2.放弃，出问题了，暂时感觉自己先修准备不足，退一步转为面试题掌握