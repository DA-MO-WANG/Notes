1. 命令行环境常用的用法

   * ```sql
     -- 选择具体的数据库
     use 数据库名;
     -- 显示表的情况
     show tables;
     -- 展示表的表结构
     show columns from 具体表;
     ```

2. 很简单的查询

   * 检索单个列、多个列、所有列
   * 对返回情况有限制
     * 返回不重复的行：distinct 适用于所有列，而不是离它最近的列，指的是相同的只显示一次
     * 返回的行数有要求：limit  行下标（从0开始），返回的行数量
   * 完全限定名
   * 排序效果
     * order by  任意列（与这个列是否要检索无关）
       * 多级排序情况：第一个排序条件无法做出排序选择，才去执行第二个条件
       * 排序方向：desc 和 asc 两种策略，放在相应条件后面
       * order by 与 limit 组合会产生top函数的效果
   * 筛选语句
     * 等值查询
       * NULL的特殊性：不等查询时，不包括null
     * 范围查询
       * 字段名  between 值1  and 值2
       * 离散范围： 字段名  in  (值1，值2)
     * 组合条件
       * and 比 or有更高优先级
       * not 来否定其他条件
     * 

3. 特殊的匹配

   * 模糊匹配:like
     * % 不能匹配空值
   * 正则匹配？？？？

4. 加工从数据库中检索出的数据：因为用户想要的格式，不一定与数据库中存储的对等

   * concat() ：拼接，用逗号把字段和其他内容粘在一起
   * trim() : 去掉左右两边的空格
   * as 起别名
   * 算术运算
   * 文本处理函数
     * Upper()
     * soundex()：发音比较，只要相似就认为匹配
     * Date()、Time()、Year()
     * 数值函数
   * 汇总函数
     * avg()：忽略null
     * count()：如果是*，包含null; count(具体字段) 不包含null
     * max()、min()
     * sum()
   * 组合用途
     * avg(distinct columin)

5. 分组：把数据分成逻辑组，对每个组内聚集运算

   * group by 被检索的列
   * 过滤分组：HAVING 条件![image-20220421140638865](book_Mysql必知必会_使用语法.assets/image-20220421140638865.png)

   

6. 子查询

   * 嵌套子查询：子句作为where条件In 操作符

   * 子查询作为被显示的字段

7. 联结：在运行中产生

   * 内联结：笛卡尔积：最原始的情况，没有联结条件时，第一个表的每行去第二个表的每一行匹配

     ```sql
     -- ANSI SQL规范，推荐inner join语法
     关键1:哪些表要联结
     关键2:联结的条件是什么
     ```

   * 外部联结：outer join 指定联结的方向

     * left join  从左边的表的每一行开始，去匹配
     * right join

   * 与其他技巧复合运用

8. 组合查询：执行多个查询，把结果合并作为单个查询结果集展示

   * 规则：每个查询被检索列必须相同

9. 全文本搜索？？？？