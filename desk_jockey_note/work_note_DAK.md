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

