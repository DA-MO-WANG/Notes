###### 搭建环境

1. 从0开始创建一个项目

```shell
#安装一个类似maven的项目管理工具：
npm install -g @angular/cli
#创建workspace, 默认的一套框架文件放在哪个文件夹
ng new my-app
#运行项目
cd my-app
ng serve --open #启动项目，自动打开浏览器访问
```

2. 更改别人的项目

```shell
#考虑国内网络环境，换成淘宝镜像
npm install -g cnpm --registry=https://registry.npm.taobao.org
npm install
ng serve
```



