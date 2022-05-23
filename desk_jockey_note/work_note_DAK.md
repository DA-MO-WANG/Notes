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





