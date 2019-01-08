---
layout: article
title: 数据库基础学习-MySQL(一)
tags: 数据库基础 MySQL
article_header:
  type: cover
  image:
    src: /images/mysql1.jpg
---
## 一、基础知识

**常见的数据库软件**

| 所属公司 | 数据库软件          |
| ---     | ---                |
| 甲骨文   | MySQL、Oracle      |
| IBM     | DB2                |
| 微软     | SQL Server、Access |
| 赛贝斯   | Sybase             |

**MySQL特点：** 开源免费、轻量级、适用于中小型项目

**数据库管理系统分类（按数据模型分类）：**  

* 网状模型：关系复杂、不易管理
* 层次模型：层次分明、适用的局限性较大
* 关系模型：使用二维表格来描述数据

**数据库语言：** SQL (Structured Query Language)

**SQL标准：**

1. 数据查询 DQL (Query)：查
2. 数据操作 DML (Manipulation)：增删改
3. 数据控制 DCL (Control)：权限管理
4. 数据定义 DDL (Defined)：对数据库和表结构的增删改

SQL方言：只适用于指定的数据库，如MySQL的limit

## 二、DDL

* 显示所有的数据库：show databases;
* 创建数据库：create database \[if not exits\] 数据库名 charset='utf8';
* 删除数据库：drop database 数据库名;
* 使用数据库：use 数据库名;
* 创建表：create table \[if not exits\] 表名(列名1 列类型, 列名2 列类型, ... ,列名n 列类型);
* 显示所有编码：show variables like 'char%';
* 修改数据库编码集：alter database 数据库名 charset='latin1';
* 显示所有表：show tables;
* 显示表结构：desc 表名;
* 删除表：drop table 表名;
* 修改表名：alter table 旧表名 rename to 新表名;
* 添加列：alter table 表名 add(列名1 列类型, 列名2 列类型);
* 删除列：alter table 表名 drop 列名;
* 修改列名：alter table 表名 change 旧列名 新列名 新列类型;
* 修改列类型：alter table 表名 modify 列名 新列类型;

## 三、DML

* 添加行数据：inset into 表名 values(列值11, 列值12),(列值21, 列值22); insert into 表名(列名1, 列名2) value (列值1, 列值2);
* 修改值（更改所有行）：update 表名 set 列名1 = 列值1, 列名2 = 列值2;
* 修改值（指定行）：update 表名 set 列名1 = 列值1, 列名2 = 列值2 where 条件;
* 删除表数据：delete from 表名 where 条件;
* 清空表数据：truncate table 表名;

## 四、DQL

* 查所有数据：select * from 表名;
* 查所有行指定列数据：select 列名1, 列名2 from 表名;
* 查指定行指定列数据：select 列名1, 列名2 from 表名 where 条件;
* 查询数据重复的记录只查一条：select distinct 列名 from 表名;

**条件使用关键字**

1. 比较运算符：=、>、<、>=、<=、!=、<>
2. 逻辑运算符：or、and
3. 范围：in(i1, i2, i3) 等价于 x=i1 or x=i2 or x=i3、between i1 and i2 等价于 x>=i1 and x<=i2
4. 判断：is null、is not null
5. 模糊查询：like 表达式 (表达式中 %代表0到多个字符) 、_代表一个字符)

**列运算**

* 数量类型的列可以做加减乘除运算
* null参加算术运算结果是null
* 字符串类型可以做连续运算：concat() `eg: select concat ('我叫', name) from student;`
* 转换null值 ifnull() `eg: select ifnull(name, '无名') from student;`
* 给查到的列起名 as (可省略) `select 列名 as 名字 from 表名;`

**排序**

* 升序（不写列名后面的asc或desc默认是升序）：order by 列名 asc;
* 降序：order by 列名 desc;

**聚合函数**

做某列的纵向运算。  
count（计数）、max（找最大值）、min（找最小值）、sum（求和）、avg（求平均值）

**分组查询**

group by 列名 \[having 组条件\]

## 五、约束

* 主键约束 primary key
  
  * 特点：不能为null、不能重复、可被从表引用
  * 添加主键约束：alter table 表名 add primary key(列名);
  * 删除主键约束：alter table 表名 drop primary key;
  * 联合主键：`eg: 创建老师与学生的关系表：create table teacher_student(tid int, sid int, primary key(tid, sid));`
  * 主键自增长：auto_increment

* 非空约束 not null

* 唯一约束 unique

* 外键约束 foreign key

  * constraint 外键约束名 foreign key(外键列名) references 关联表(关联表主键列名);
  * 外键特点：
    * 外键列值必须等于关联表主键的值
    * 可以重复、可以为空、一张表可以有多个外键

**表与表之间的关系**

1. 一对多：多的一方定义为从表
2. 多对多：定义关系表，使用联合主键
3. 一对一：在从表中把主键定义为外键

## 六、导入导出

* 导出sql文件：DOS命令行中输入:
  * mysqldump -u用户名 -p密码 数据库名 \> 目的文件路径;
* 导入sql文件，创建数据库：前提是先创建同名的数据库，然后在DOS命令行中输入:
  * （方式1）mysql -u用户名 -p密码 数据库名 \< 源文件路径;
  * （方式2）use 数据库名; source: 源文件路径;

## 七、多表查询

**合并查询union**

* 合并结果集要求被合并表中列相同
* 不去除重复行：union all
* 去除重复行：union

**连接查询**

* **内连接**
  * mysql方言语法：select * from 表1 a, 表2 b where a.aid = b.bid;
  * 标准语法：select * from 表1 a inner join 表2 b on a.aid = b.bid;
* **外连接**
  * 左外连接：select * from 表1 a left outer(outer可省略) join 表2 b on a.aid = b.bid;
  * 右外连接：select * from 表1 a right outer(outer可省略) join 表2 b on a.aid = b.bid;
  * 左外连接查询使左表的数据能全查出来，右表中没有与左表对应的数据全部为null；右外连接同理。
* **全连接**
  * 左外连接 union 右外连接
* **子查询**
  * where后有查询，查询的结果作为条件；或者from后有查询，查询的结果作为表的数据存在。

## 八、MySQL的数据类型

* 整数类型
  * tinyint      1个字节
  * int/integer  4个字节
  * bigint       8个字节
* 字符类型
  * char(20)     固定长度, 不够20个用空格补齐
  * varchar(20)  可变长度，最多有20个字符
  * text         2^16个字节
* 字节类型
  * binary       255B
  * blob         65k
  * mediumblob   16M
  * longblob     4G
* 浮点类型
  * float(3, 1)  4个字节，2位整数，1位小数
  * double(3, 1) 8个字节，2位整数，1位小数
  * decimal      精度高，常用来记录金钱
* 逻辑类型
  * bool/boolean 1个字节 值为true/1、false/0
* 时间类型
  * time         时分秒
  * date         年月日
  * datetime     年月日时分秒
  * timestamp    时间戳，从计算机元年到某一时间的毫秒值
  
## 九、数据库设计的三大范式

1. 字段的原子性，字段不可再被分割
2. 必须有主键，其余字段对主键必须完全依赖
3. 其余字段对主键不存在传递依赖， 可以将有传递依赖的字段分开再做一张表

## 十、java.util.Date 与 java.sql.Date 之间的转换（通过毫秒值的方式）：

```
java.util.Date d1 = new java.util.Date();

java.sql.Date d2 = new java.sql.Date(d1.getTime());
java.sql.Time d3 = new java.sql.Time(d1.getTime());
java.sql.Timestamp d4 = new java.sql.Timestamp(d1.getTime());
```

## 十二、JDBC

```java
public class ConnectionUtil{
    //1.导入mysql-connector-java-5.0.8-bin.jar
    //2.注册驱动
    static{
        try{
            Class.forName("com.mysql.jdbc.Driver");            
        }catch(ClassNotFoundException e){
            throw new NullPointerException(e.getMessage());
        }
    }
    public vodi connection() throws Exception{
        //3.获取连接
        Connection con = DriverManager.getConnection("jdbc:mysql://localhost:3306/test?characterEncoding=utf8", "root", "pwd");
        //4.通过连接获取Statement对象（sql语句发送器）
        Statement sta = con.createStatement();
        //5.通过Statement对象发送sql语句
        String sql = "select * from student";
        ResultSet set = sta.executeQuery(sql);
        //6. 解析结果集
        while(set.next()){
            //将结果存入实体对象
        }
        //7.关闭连接，释放资源
        set.close();
        sta.close();
        con.close();
    }
}
```
