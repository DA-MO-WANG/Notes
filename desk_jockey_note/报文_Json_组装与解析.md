

#### Json语法-看懂json数据

> Json语法是JavaScript对象表示法的子集

* 两种结构要素
  * 无序排布的name/value 对，可以用object实现
  * 有序的值的分布，可以用array实现
* object要素
  * 以{ 开始，以 } 结束
  * name 只能是字符串形式，value可以是字符串/object/array/number/bool/null
* array要素
  * 以[ 开始，以 ] 结束
  * value可以是字符串/object/array/number/bool/null



#### Json解析

* 常用工具

  * 方式1: 原生JDK
  * 方式2: fastJson
  * 方式3: Gson

* 使用模版

  * 准备好model—json数据结构映射对应的实体类

    * 节点名与成员变量名一一对应
    * set/get方法不能缺少

  * ```java
    //调用工具类，完成json到实体类的数据映射
    //fastjson
    jsonmodel root = JSON.parseObject(json,jsonmodel.class);
    //Gson
    jsonmodel root = gson.fromJson(json,jsonmodel.class);
    ```

  * 

* 

  