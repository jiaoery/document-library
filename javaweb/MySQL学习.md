# MySQl(老杜)

## 1.初识MySQL

### 1.1什么是数据库？什么是数据库管理系统？什么是SQL？

* 数据库：
  * 英文单词：database，简称DB。按照一定格式存储数据的一些文件组合。顾名思义，存储数据的仓库，实际上就是一堆文件，这些文件存储了具有特定格式的数据

* 数据库管理
  * DataBaseManagement:简称DMBMS
  * 数据库管理系统术专门用来管理数据库汇总的数据的，数据库管理系统可以对数据库中的数据进行增删改查
  * 常见的数据库管理系统：Mysql、Orcle、Ms SqlServer、DB2等
* SQL:结构化查询语言
  * 程序员需要学习SQL语句，实现增删改查

三者关系：DMBMS->执行->SQL->操作->DB

## 2.安装Mysql

网路教程即可，傻瓜式安装

### 2.1 配置环境变量

Windows配置方式与Java环境配置相同；

![img](https://pic4.zhimg.com/80/v2-9de6c0cd248733c5e84afb7d97140adf_1440w.jpg)

mac 配置环境变量

未配置时的状态

![image-20220421114004720](img/image-20220421114004720.png)

切换到zsh命令（新的mac系统大部分是这个命令行）

```bash
chsh -s /bin/zsh
```

首先配置环境变量文件

```bash
vim ~/.zshrc
```

在打开的文件里追加以下内容

```text
export PATH=$PATH:/usr/local/mysql/bin
alias mysqlstart='sudo /usr/local/mysql/support-files/mysql.server start'
alias mysqlstop='sudo /usr/local/mysql/support-files/mysql.server stop'
```

之后，按`esc`退出插入模式，并输入`:wq` ( 有`:`哦 ) 

使用vim命令保存文件内容

```bash
source ~/.zshrc
```

查看是否成功

```bash
echo $PATH
```

如果添加成功，会出现`/usr/local/mysql/bin`这一句，就说明配置成功了

需要注意端口不可以冲突



## 3.数据库常用命令行

* 登录数据库

```bash
mysql -uroot -p [密码]
```



![image-20220421145233320](img/image-20220421145233320.png)

* 退出数据库

  ```bash
  exit
  ```

* 查看mysql中有哪些数据库

```bash
show databases;
```

![image-20220421154107581](img/image-20220421154107581.png)

这四个是默认数据库

* 打开指定的数据库

```bash
use [数据库名字];
```

![image-20220421154343041](img/image-20220421154343041.png)

* 创建数据库

```bash
create database [数据库名];
```

## 4.表常用命令行

**表**：数据中最基本的单位table，数据库中是以表格的形式来表示数据的，任何一张表都有**行**和**列**

行：row，被称为数据或记录

列：column，被称为字段

`ps：每一个字段都有：字段名、数据类型、约束等属性`

字段名：见名知意

数据类型：字符串，数字，日期等

约束：被约束的字段具备唯一性

![请添加图片描述](https://img-blog.csdnimg.cn/23050b48ad644347934c9a4ae4440d50.png)



* 查看表

```bash
show tables;
```

* 查看mysql数据库的版本号

```bash
select verison();
```

* 查看当前使用的是哪个数据库

```bash
select database();
```

**注意：\c用来终止一条命令的输入。**

 ## 5. 常用SQL语句的分类

* DQL：数据查询语言 (DQL-Data Query Language)
  * 凡是带有select关键字的都是查询语句

* DML：数据操作语言 (DML-Data Manipulation Language)

  * 凡是对表当中的数据进行增删改的都是DML

     insert 增

     delete 删

     update 改

 **注意：这个主要是操作表中的数据data。**

* DDL：数据定义语言 (DDL-Data Definition Language)

  * DDL主要操作的是表的结构，不是表中的数据。

    create：新建，等同于增
    drop：删除
    alter：修改

**注意：这个增删改和DML不同，这个主要是对表结构进行操作。**

* TCL：事务控制语言 (TCL-Transactional Control Language)

  ​	事务提交：commit;

  ​    事务回滚：rollback,

* DCL：是数据控制语言 (DCL-Data Control Language)

   授权：grant

   撤销权限：revoke

## 6.导入准备的表

* 导入sql文件

```mysql
resource [sql路径名]
```

**注意：路径中不要有中文！！！！**这里使用到的资源https://pan.baidu.com/share/init?surl=kW7V3yLaxDJxCaZ2yp019g 密码：y346

最终表如下

![image-20220422163757239](img/image-20220422163757239.png)

其中：DEPT 部门表 EMP员工表 SALGRADE 工资等级表

* 删除表（这里不建议测试）

```mysql
drop database [表名]
```

* 查看表中所有数据

```mysql
select * from 表名;
```

![image-20220422164210104](img/image-20220422164210104.png)

![image-20220422164318205](img/image-20220422164318205.png)

![image-20220422164452415](img/image-20220422164452415.png)

* 不看表中的数据，只看表的结构

```mysql
desc 表名;
```

效果如下：

![image-20220427192353729](img/image-20220427192353729.png)

## 7.查询操作

### 7.1 普通查询方式

* 查询表中某几个字段

```mysql
select [字段，中间用“,”隔开] from 【表名】

mysql> select deptno,dname from dept;
+--------+------------+
| deptno | dname      |
+--------+------------+
|     10 | ACCOUNTING |
|     20 | RESEARCH   |
|     30 | SALES      |
|     40 | OPERATIONS |
+--------+------------+
4 rows in set (0.00 sec)
```

* 查询全部字段

```mysql
#第一种方式：可以把每个字段都写上
select a,b,c,d,e,f… from tablename;
#第二种方式：可以使用*
select * from tablename;
```

**注意：**这种方式的缺点：
 1、效率低
 2、可读性差。
 在实际开发中不建议，可以自己玩没问题。
 你可以在DOS命令窗口中想快速的看一看全表数据可以采用这种方式。

* 给查询的类起别名

```mysql
select deptno,dname as deptname from dept;

+--------+------------+
| deptno | deptname   |
+--------+------------+
|     10 | ACCOUNTING |
|     20 | RESEARCH   |
|     30 | SALES      |
|     40 | OPERATIONS |
+--------+------------+
4 rows in set (0.00 sec)
```

其中as 为起别名的关键字

**注意：只是将显示的查询结果列名显示为deptname，原表列名还是叫：dname**
**记住：select语句是永远都不会进行修改操作的。（因为只负责查询）**

并且，as关键字是可以被省略的

```mysql
mysql> select deptno,dname deptname from dept;
+--------+------------+
| deptno | deptname   |
+--------+------------+
|     10 | ACCOUNTING |
|     20 | RESEARCH   |
|     30 | SALES      |
|     40 | OPERATIONS |
+--------+------------+
4 rows in set (0.00 sec)
```

**Q:如果别名里面有空格该如何处理？**

```mysql
select deptno,dname 'dept name' from dept; //加单引号
select deptno,dname "dept name" from dept; //加双引号
+--------+------------+
| deptno | dept name  |
+--------+------------+
|     10 | ACCOUNTING |
|     20 | RESEARCH   |
|     30 | SALES      |
|     40 | OPERATIONS |
+--------+------------+

```

**注意：**

**在所有的数据库当中，字符串统一使用单引号括起来，**
**单引号是标准，双引号在oracle数据库中用不了。但是在mysql中可以使用。**

* 查询中带公式

```mysql
mysql> select ename,sal*12 as yearsal from emp;
+--------+----------+
| ename  | yearsal  |
+--------+----------+
| SMITH  |  9600.00 |
| ALLEN  | 19200.00 |
| WARD   | 15000.00 |
| JONES  | 35700.00 |
| MARTIN | 15000.00 |
| BLAKE  | 34200.00 |
| CLARK  | 29400.00 |
| SCOTT  | 36000.00 |
| KING   | 60000.00 |
| TURNER | 18000.00 |
| ADAMS  | 13200.00 |
| JAMES  | 11400.00 |
| FORD   | 36000.00 |
| MILLER | 15600.00 |
+--------+----------+
14 rows in set (0.00 sec)
## 别名为中文，需要带 ‘’
+--------+----------+
| ename  | 年薪     |
+--------+----------+
| SMITH  |  9600.00 |
| ALLEN  | 19200.00 |
| WARD   | 15000.00 |
| JONES  | 35700.00 |
| MARTIN | 15000.00 |
| BLAKE  | 34200.00 |
| CLARK  | 29400.00 |
| SCOTT  | 36000.00 |
| KING   | 60000.00 |
| TURNER | 18000.00 |
| ADAMS  | 13200.00 |
| JAMES  | 11400.00 |
| FORD   | 36000.00 |
| MILLER | 15600.00 |
+--------+----------+
14 rows in set (0.00 sec)
```

### 7.2条件查询

|       运算符        |                            说明                             |
| :-----------------: | :---------------------------------------------------------: |
|          =          |                            等于                             |
|     <>或者!=bu      |                           不等于                            |
|          <          |                            小于                             |
|         <=          |                          小于等于                           |
|          >          |                            大于                             |
|         >=          |                          大于等于                           |
| between ... and ... |                        在两个值之间                         |
|       is null       |                            为空                             |
|         and         |                            并且                             |
|         or          |                            或者                             |
|         in          |                            包含                             |
|         not         |                             非                              |
|        like         | 模糊查询支持%或\_,其中%匹配任意长的字符，一个\_匹配一个字符 |

关键字where具体的语法格式如下

> select
> 字段1,字段2,字段3…
> from
> 表名
> where
> 条件;
