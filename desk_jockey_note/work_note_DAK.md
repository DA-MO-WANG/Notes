```shell
#spring注解
@RestController
从游戏角度来看，这个按钮作用是标识控制器类---给spring框架来识别的
@RequestMapping("url")
提供的作用：标识url映射路径---也是前端需要的：ip:端口:项目路径:<p color ="red">控制器类url映射路径</p>:方法路径
不同的是方法上的mapping映射是带方法指示的映射：比如@GetMapping("url")
@PathVariable("url路径中占位符参数名")----》对应{参数名}


```

```shell
#angular项目
#项目目录结构
		》e2e  #自动化集成测试目录
		》node_modules  #npm依赖库
		》src #源代码目录
		
				》app #工程源码
				
						》自定义的项目目录
								》页面、调接口的部分：service-quality-assurance-info.service.ts														
				》assets #资源目录
				》environments #环境目录
																								

先找前后端交互的部分：src/app/core/routing/xxx.ts

```



```sql
-- sql
-- case when 函数有两种用法
-- 用法1:
case 字段
	when 值1  then express1
	when 值2  then express2
else express3 end
-- 用法2:
case when 条件1 then express1
		 when 条件2 then express2
else express3 

-- join操作
inner join 只展示两个表的交集部分，也就是匹配条件成立的部分
left join 以左边每行记录为准，去找右边匹配的部分，找不到的用null代替
right join 以右边每行记录为准，同理
```



```java
//java日志处理
//错误范例：
logger.info(内部用+拼接)//原因是可能产生多个字符串对象，耗性能
//正确范例——日志带有关键词，比如流水号，这样后期看日志可以根据关键词查找
logger.info(用占位符{},逗号，参数来构建)
  
```

```java
//HttpClient 出了问题，束手无策，这方面有机会补补
//抓包工具，在linux环境，这些也不太懂
```



```java
//git开发流程多场景
//1.一个新需求
//先确定是不是本地分支
	//本地分支列表
	git branch 
  //切换到指定的本地开发分支
  git checkout XXX
	//拉取远程最新分支代码
  git fetch origin XXX
  //手动处理冲突
  //推送到远程上(默认推到同名分支上)
  git push origin 
//2.如何回滚到上次提交
   //拿到想要回滚到的目标状态 commit id
   //本地切换到指定的本地分支
   //把本地分支回滚到指定的commit状态
    git reset --hard commit_id
   //推送到远程
    git push origin 
  
```

```sql
-- angular 
-- 关于样式的处理
ngClass 与 ngStyle 
-- 都可以处理样式的选择，具体用法参考文档（很多逻辑其实与后台语言大同小异）(前端开发，会看文档很关键)
-- 菜鸟教程对查询前端的一点指令有作用，而且也可以用来写demo





```

```sql
-- order by的新用法
order by 指定值,次级排序指标,再次级 -- 指定值放在最前还是最后






```

