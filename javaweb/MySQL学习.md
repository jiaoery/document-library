# MySQl(老杜)

视频资源：https://www.bilibili.com/video/BV1Vy4y1z7EX?p=37&spm_id_from=pageDriver

## 1.初识MySQL

Q：什么是数据库？什么是数据库管理系统？什么是SQL？

* 数据库：
  * 英文单词：database，简称DB。按照一定格式存储数据的一些文件组合。顾名思义，存储数据的仓库，实际上就是一堆文件，这些文件存储了具有特定格式的数据

* 数据库管理
  * DataBaseManagement:简称DMBMS
  * 数据库管理系统术专门用来管理数据库汇总的数据的，数据库管理系统可以对数据库中的数据进行增删改查
  * 常见的数据库管理系统：Mysql、：、Ms SqlServer、DB2等
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

 ## 5.常用SQL语句的分类

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

* =等于

查询薪资等于800的员工姓名和编号：

```MYSQL
mysql> select empno,ename from emp where sal = 800;
+-------+-------+
| empno | ename |
+-------+-------+
|  7369 | SMITH |
+-------+-------+
1 row in set (0.00 sec)
```

查询SMITH的编号和薪资

```mysql
mysql> select empno,sal from emp where ename = 'SMITH' ;
+-------+--------+
| empno | sal    |
+-------+--------+
|  7369 | 800.00 |
+-------+--------+
1 row in set (0.00 sec)
```

* <>!=不等于

薪资不是800的员工编号和姓名

```mysql
mysql> select empno,ename from emp where sal !=800;
+-------+--------+
| empno | ename  |
+-------+--------+
|  7499 | ALLEN  |
|  7521 | WARD   |
|  7566 | JONES  |
|  7654 | MARTIN |
|  7698 | BLAKE  |
|  7782 | CLARK  |
|  7788 | SCOTT  |
|  7839 | KING   |
|  7844 | TURNER |
|  7876 | ADAMS  |
|  7900 | JAMES  |
|  7902 | FORD   |
|  7934 | MILLER |
+-------+--------+
13 rows in set (0.00 sec)
# 与上面效果一样
mysql> select empno,ename from emp where sal <>800;
+-------+--------+
| empno | ename  |
+-------+--------+
|  7499 | ALLEN  |
|  7521 | WARD   |
|  7566 | JONES  |
|  7654 | MARTIN |
|  7698 | BLAKE  |
|  7782 | CLARK  |
|  7788 | SCOTT  |
|  7839 | KING   |
|  7844 | TURNER |
|  7876 | ADAMS  |
|  7900 | JAMES  |
|  7902 | FORD   |
|  7934 | MILLER |
+-------+--------+
13 rows in set (0.00 sec)
```



* between ... and ... 两个值之间，等于 >= and <=

查询薪资在2450和3000之间的员工信息，包括2450和3000

```mysql
mysql> select empno,ename,sal from emp where sal>=2450 and sal <=3000;
+-------+-------+---------+
| empno | ename | sal     |
+-------+-------+---------+
|  7566 | JONES | 2975.00 |
|  7698 | BLAKE | 2850.00 |
|  7782 | CLARK | 2450.00 |
|  7788 | SCOTT | 3000.00 |
|  7902 | FORD  | 3000.00 |
+-------+-------+---------+
5 rows in set (0.00 sec)

#下面与上面信息相同
mysql> select empno,ename,sal from emp where sal between 2450 and 3000;
+-------+-------+---------+
| empno | ename | sal     |
+-------+-------+---------+
|  7566 | JONES | 2975.00 |
|  7698 | BLAKE | 2850.00 |
|  7782 | CLARK | 2450.00 |
|  7788 | SCOTT | 3000.00 |
|  7902 | FORD  | 3000.00 |
+-------+-------+---------+
5 rows in set (0.00 sec)
```

**注意：betwwen and 必须遵循左小右大，between and 是闭区间，包括两端的值。**

* is null 为 null（is not null 不为空）

查询补助为null的员工

```mysql
mysql> select empno,ename,sal,comm from emp where comm is null;
+-------+--------+---------+------+
| empno | ename  | sal     | comm |
+-------+--------+---------+------+
|  7369 | SMITH  |  800.00 | NULL |
|  7566 | JONES  | 2975.00 | NULL |
|  7698 | BLAKE  | 2850.00 | NULL |
|  7782 | CLARK  | 2450.00 | NULL |
|  7788 | SCOTT  | 3000.00 | NULL |
|  7839 | KING   | 5000.00 | NULL |
|  7876 | ADAMS  | 1100.00 | NULL |
|  7900 | JAMES  |  950.00 | NULL |
|  7902 | FORD   | 3000.00 | NULL |
|  7934 | MILLER | 1300.00 | NULL |
+-------+--------+---------+------+
10 rows in set (0.00 sec)
```

查询补助不为null的员工

```mysql
mysql> select empno,ename,sal,comm from emp where comm is not null;
+-------+--------+---------+---------+
| empno | ename  | sal     | comm    |
+-------+--------+---------+---------+
|  7499 | ALLEN  | 1600.00 |  300.00 |
|  7521 | WARD   | 1250.00 |  500.00 |
|  7654 | MARTIN | 1250.00 | 1400.00 |
|  7844 | TURNER | 1500.00 |    0.00 |
+-------+--------+---------+---------+
4 rows in set (0.00 sec)
```

* and 并且（多条件结合）

查询工作岗位是MANAGER 并且工资大于2500的员工

```mysql
mysql> select empno,job from emp where job = 'MANAGER' and sal>2500;
+-------+---------+
| empno | job     |
+-------+---------+
|  7566 | MANAGER |
|  7698 | MANAGER |
+-------+---------+
2 rows in set (0.00 sec)
```

* or 或者（两者满足其一即可）

查询工作岗位是MANAGER或SALESMAN的员工

```mysql
mysql> select empno,job from emp where job = 'MANAGER' or job='SALESMAN';
+-------+----------+
| empno | job      |
+-------+----------+
|  7499 | SALESMAN |
|  7521 | SALESMAN |
|  7566 | MANAGER  |
|  7654 | SALESMAN |
|  7698 | MANAGER  |
|  7782 | MANAGER  |
|  7844 | SALESMAN |
+-------+----------+
7 rows in set (0.00 sec)
```

* and和or同时出现，要考虑优先级到的问题

查询工资大于2500，并且部门编号为10或20部门的员工

```mysql
mysql> select * from emp where sal > 2500 and (deptno = 10 or deptno = 20);
+-------+-------+-----------+------+------------+---------+------+--------+
| EMPNO | ENAME | JOB       | MGR  | HIREDATE   | SAL     | COMM | DEPTNO |
+-------+-------+-----------+------+------------+---------+------+--------+
|  7566 | JONES | MANAGER   | 7839 | 1981-04-02 | 2975.00 | NULL |     20 |
|  7788 | SCOTT | ANALYST   | 7566 | 1987-04-19 | 3000.00 | NULL |     20 |
|  7839 | KING  | PRESIDENT | NULL | 1981-11-17 | 5000.00 | NULL |     10 |
|  7902 | FORD  | ANALYST   | 7566 | 1981-12-03 | 3000.00 | NULL |     20 |
+-------+-------+-----------+------+------------+---------+------+--------+
4 rows in set (0.00 sec)
```

**注意：and 和or同时出现，and优先级高。如果想让or先执行，需要加括号。以后开发中，如果不确定优先级，就加括号。**

* in 包含，相当于多个 or（not in 不在这个范围中）

查询工作岗位是MANAGER和SALESMAN的员工：

> select empno,ename,job from emp where job = 'MANAGER' or job = 'SALESMAN';
>
> select empno,ename,job from emp where job in('MANAGER', 'SALESMAN');

上面两个查询语句是一样的效果

**注意：in不是一个区间，in后面跟的是具体的值。**

查询薪资是800和5000的员工信息：

```mysql
mysql> select ename,sal from emp where sal in(800,5000);
+-------+---------+
| ename | sal     |
+-------+---------+
| SMITH |  800.00 |
| KING  | 5000.00 |
+-------+---------+
2 rows in set (0.00 sec)
```

* not 可以取非，主要用在is或in中

is null、is not null、in、not in

```mysql
mysql> select ename,sal from emp where sal not in(800,5000,3000);
+--------+---------+
| ename  | sal     |
+--------+---------+
| ALLEN  | 1600.00 |
| WARD   | 1250.00 |
| JONES  | 2975.00 |
| MARTIN | 1250.00 |
| BLAKE  | 2850.00 |
| CLARK  | 2450.00 |
| TURNER | 1500.00 |
| ADAMS  | 1100.00 |
| JAMES  |  950.00 |
| MILLER | 1300.00 |
+--------+---------+
10 rows in set (0.00 sec)
```

* like 为模糊查询，支持%或下划线匹配
  * %匹配任意个字符
  * 下划线，一个下划线只匹配一个字符

找出名字里含有O字符的员工

```mysql
mysql> select ename from emp where ename like '%o%';
+-------+
| ename |
+-------+
| JONES |
| SCOTT |
| FORD  |
+-------+
3 rows in set (0.00 sec)
```

找出第二个字母是A的

```mysql
mysql> select ename from emp where ename like '_A%';
+--------+
| ename  |
+--------+
| WARD   |
| MARTIN |
| JAMES  |
+--------+
3 rows in set (0.00 sec)
```

### 7.3 排序

* 默认排序方式是升序

```mysql
mysql> select ename,sal from emp order by sal;
+--------+---------+
| ename  | sal     |
+--------+---------+
| SMITH  |  800.00 |
| JAMES  |  950.00 |
| ADAMS  | 1100.00 |
| WARD   | 1250.00 |
| MARTIN | 1250.00 |
| MILLER | 1300.00 |
| TURNER | 1500.00 |
| ALLEN  | 1600.00 |
| CLARK  | 2450.00 |
| BLAKE  | 2850.00 |
| JONES  | 2975.00 |
| SCOTT  | 3000.00 |
| FORD   | 3000.00 |
| KING   | 5000.00 |
+--------+---------+
14 rows in set (0.00 sec)
```

* 指定为降序

```mysql
mysql> select ename,sal from emp order by sal desc;
+--------+---------+
| ename  | sal     |
+--------+---------+
| KING   | 5000.00 |
| SCOTT  | 3000.00 |
| FORD   | 3000.00 |
| JONES  | 2975.00 |
| BLAKE  | 2850.00 |
| CLARK  | 2450.00 |
| ALLEN  | 1600.00 |
| TURNER | 1500.00 |
| MILLER | 1300.00 |
| WARD   | 1250.00 |
| MARTIN | 1250.00 |
| ADAMS  | 1100.00 |
| JAMES  |  950.00 |
| SMITH  |  800.00 |
+--------+---------+
14 rows in set (0.00 sec)
```



* 指定为升序

```mysql
mysql> select ename,sal from emp order by sal asc;
+--------+---------+
| ename  | sal     |
+--------+---------+
| SMITH  |  800.00 |
| JAMES  |  950.00 |
| ADAMS  | 1100.00 |
| WARD   | 1250.00 |
| MARTIN | 1250.00 |
| MILLER | 1300.00 |
| TURNER | 1500.00 |
| ALLEN  | 1600.00 |
| CLARK  | 2450.00 |
| BLAKE  | 2850.00 |
| JONES  | 2975.00 |
| SCOTT  | 3000.00 |
| FORD   | 3000.00 |
| KING   | 5000.00 |
+--------+---------+
14 rows in set (0.00 sec)
```

* 多字段排序

查询员工名字和薪资，要求按照薪资升序，如果薪资一样的话，再按照名字升序排列。

```mysql
mysql> select ename,sal from emp order by sal asc,ename asc;
+--------+---------+
| ename  | sal     |
+--------+---------+
| SMITH  |  800.00 |
| JAMES  |  950.00 |
| ADAMS  | 1100.00 |
| MARTIN | 1250.00 |
| WARD   | 1250.00 |
| MILLER | 1300.00 |
| TURNER | 1500.00 |
| ALLEN  | 1600.00 |
| CLARK  | 2450.00 |
| BLAKE  | 2850.00 |
| JONES  | 2975.00 |
| FORD   | 3000.00 |
| SCOTT  | 3000.00 |
| KING   | 5000.00 |
+--------+---------+
14 rows in set (0.00 sec)
```

* 综合一点的案例

```mysql
mysql> select ename,sal from emp where sal between 1250 and 3000 order by sal desc;
+--------+---------+
| ename  | sal     |
+--------+---------+
| SCOTT  | 3000.00 |
| FORD   | 3000.00 |
| JONES  | 2975.00 |
| BLAKE  | 2850.00 |
| CLARK  | 2450.00 |
| ALLEN  | 1600.00 |
| TURNER | 1500.00 |
| MILLER | 1300.00 |
| WARD   | 1250.00 |
| MARTIN | 1250.00 |
+--------+---------+
10 rows in set (0.00 sec)
```

**注意：关键字的顺序移动不能改变**

>select … from … where … order by …（排序总是在最后执行）

### 7.4 分组查询

在实际的应用中，可能有这样的需求，需要先进行分组，然后对每一组的数据进行操作，这个时候我们需要使用分组查询，将他们之间的关键字全部组合在一起：

>select … from … where … group by … order by…
>执行顺序：from、where、group by、select、order by

**注意：分组函数不能直接在where后使用**

* **注意：分组函数不能直接使用在where后面**

```mysql
mysql> select ename,sal from emp where sal > min(sal);
ERROR 1111 (HY000): Invalid use of group function
```

`select ename,sal from emp where sal > min(sal);` 错误写法，因为分组函数在使用的时候必须先分组之后才能使用。where执行的时候，还没有分组。所以where后面不能出现分组函数。

```mysql
mysql> select sum(sal) from emp;
+----------+
| sum(sal) |
+----------+
| 29025.00 |
+----------+
1 row in set (0.00 sec)
```



`select sum(sal) from emp;` 可以使用，因为select在 group by 之后执行，group by 默认整张表是一组。**（要充分理解SQL的执行顺序）**

* group 关键字

找出每个工作岗位的工资和

思路：按照工作岗位分组，然后对工资求和。

```mysql
mysql> select job,sum(sal) from emp group by job;
+-----------+----------+
| job       | sum(sal) |
+-----------+----------+
| CLERK     |  4150.00 |
| SALESMAN  |  5600.00 |
| MANAGER   |  8275.00 |
| ANALYST   |  6000.00 |
| PRESIDENT |  5000.00 |
+-----------+----------+
5 rows in set (0.00 sec)
```

分析一下以上语句的执行顺序：先从emp表中查询数据，根据job字段进行分组，然后对每一组的数据进行sum（sal）

* **注意：以下情况的sql语句是错误的**

`select ename,job,sum(sal) from emp group by job;`，不要这样写，没有意义。在mysql中可以执行，其他数据库就报错了。因为ename有14条记录，而job只有5条。

**重要结论：在一条select语句中，如果有group by语句的话，select后面只能跟：参加分组的字段，以及分组函数。其他的一律不能跟！！！**

* 练习：找出每个部门的最高薪资

实现思路：按照部门编号分组，求每一组的最大值。

```mysql
mysql> select deptno,max(sal) from emp group by deptno;
+--------+----------+
| deptno | max(sal) |
+--------+----------+
|     20 |  3000.00 |
|     30 |  2850.00 |
|     10 |  5000.00 |
+--------+----------+
3 rows in set (0.00 sec)
```

* 两个字段联合分组

Q:找出“每个部门，不同工作岗位”的最高薪资：

```mysql
mysql> select deptno,job,max(sal) from emp group by deptno,job;
+--------+-----------+----------+
| deptno | job       | max(sal) |
+--------+-----------+----------+
|     20 | CLERK     |  1100.00 |
|     30 | SALESMAN  |  1600.00 |
|     20 | MANAGER   |  2975.00 |
|     30 | MANAGER   |  2850.00 |
|     10 | MANAGER   |  2450.00 |
|     20 | ANALYST   |  3000.00 |
|     10 | PRESIDENT |  5000.00 |
|     30 | CLERK     |   950.00 |
|     10 | CLERK     |  1300.00 |
+--------+-----------+----------+
9 rows in set (0.00 sec)
```

* 找出每个部门最高薪资，要求显示最高薪资大于3000的

方法一：找出每个部门最高薪资，然后显示最高薪资大于3000的

```mysql
mysql> select deptno,max(sal) from emp group by deptno having max(sal)>3000;
+--------+----------+
| deptno | max(sal) |
+--------+----------+
|     10 |  5000.00 |
+--------+----------+
1 row in set (0.00 sec)
```

**注意：使用having可以对分完组之后的数据进一步过滤，注意having不能单独使用，having不能代替where，having必须和group by联合使用。**

方法二：先将大于3000的都找出来，然后进行分组。

```mysql
mysql> select deptno,max(sal) from emp where sal>3000 group by deptno;
+--------+----------+
| deptno | max(sal) |
+--------+----------+
|     10 |  5000.00 |
+--------+----------+
1 row in set (0.00 sec)
```

**优化策略where和having，优先选择where，where实在完成不了了，再选择having。**



* 找出每个部门的平均薪资，要求显示平均薪资高于2500的

```mysql
mysql> select deptno,avg(sal) from emp group by deptno having avg(sal)>2500;
+--------+-------------+
| deptno | avg(sal)    |
+--------+-------------+
|     10 | 2916.666667 |
+--------+-------------+
1 row in set (0.00 sec)
```

### 7.5 总结

> select
>
> ... 
>
> from 
>
>  ... 
>
> where 
>
> ... 
>
> group by
>
>  ... 
>
> having
>
>  ... 
>
> order by 
>
> ...				
>
> 以上关键字只能按照这个顺序来，不能颠倒。

执行顺序：1. from 2. where 3. group by 4. having 5. select 6. order by

### 7.6 查询结果去重

查询结果去重复记录：distinct

原表的数据不会被修改，但是查询结果会去重

```mysql
# 会存在重复数据
mysql> select job from emp;
+-----------+
| job       |
+-----------+
| CLERK     |
| SALESMAN  |
| SALESMAN  |
| MANAGER   |
| SALESMAN  |
| MANAGER   |
| MANAGER   |
| ANALYST   |
| PRESIDENT |
| SALESMAN  |
| CLERK     |
| CLERK     |
| ANALYST   |
| CLERK     |
+-----------+
14 rows in set (0.00 sec)
## 去重
mysql> select distinct job from emp;
+-----------+
| job       |
+-----------+
| CLERK     |
| SALESMAN  |
| MANAGER   |
| ANALYST   |
| PRESIDENT |
+-----------+
5 rows in set (0.00 sec)
```



## 8.函数

### 8.1 单行处理函数

特点：一个输入对应一个输出

#### 8.1.1字符串函数(JAVA String函数类似)

* upper、lower函数改变大小写

`select lower(ename) from emp;` 变成小写

```mysql
mysql> select lower(ename) from emp;
+--------------+
| lower(ename) |
+--------------+
| smith        |
| allen        |
| ward         |
| jones        |
| martin       |
| blake        |
| clark        |
| scott        |
| king         |
| turner       |
| adams        |
| james        |
| ford         |
| miller       |
+--------------+
14 rows in set (0.00 sec)
```

* substr,substring函数截取，索引从1开始**（没有0）**

```mysql
mysql> select substr(ename,1,1) from emp;
+-------------------+
| substr(ename,1,1) |
+-------------------+
| S                 |
| A                 |
| W                 |
| J                 |
| M                 |
| B                 |
| C                 |
| S                 |
| K                 |
| T                 |
| A                 |
| J                 |
| F                 |
| M                 |
+-------------------+
14 rows in set (0.00 sec)
```

* 练习：找到第一个字母以A开头的员工信息：

```mysql
# 方法1：模糊查询1
mysql> select ename from emp where ename like 'A%';
+-------+
| ename |
+-------+
| ALLEN |
| ADAMS |
+-------+
2 rows in set (0.00 sec)

# 方法2：substr函数
mysql> select ename from emp where substr(ename,1,1) = 'A';
+-------+
| ename |
+-------+
| ALLEN |
| ADAMS |
+-------+
2 rows in set (0.00 sec)
```

* concat函数：拼接字符串
* length函数用于获取参数值的字节个数

```mysql
# 获取对应字段的长度
mysql> select length(ename) enamelength from emp;
+-------------+
| enamelength |
+-------------+
|           5 |
|           5 |
|           4 |
|           5 |
|           6 |
|           5 |
|           5 |
|           5 |
|           4 |
|           6 |
|           5 |
|           5 |
|           4 |
|           6 |
+-------------+
14 rows in set (0.00 sec)

# 首字母大写，其他字母小写
mysql> select concat(upper(substr(ename,1,1)),lower(substr(ename,2,length(ename) - 1 ))) as result from emp;
+--------+
| result |
+--------+
| Smith  |
| Allen  |
| Ward   |
| Jones  |
| Martin |
| Blake  |
| Clark  |
| Scott  |
| King   |
| Turner |
| Adams  |
| James  |
| Ford   |
| Miller |
+--------+
14 rows in set (0.00 sec)
```

* **instr**函数返回字符串第一次出现的索引，若找不到，则返回0
* trim函数去前后空格

```mysql
mysql> select * from emp where ename = trim(' KING');
+-------+-------+-----------+------+------------+---------+------+--------+
| EMPNO | ENAME | JOB       | MGR  | HIREDATE   | SAL     | COMM | DEPTNO |
+-------+-------+-----------+------+------------+---------+------+--------+
|  7839 | KING  | PRESIDENT | NULL | 1981-11-17 | 5000.00 | NULL |     10 |
+-------+-------+-----------+------+------------+---------+------+--------+
1 row in set (0.00 sec)
```

* lpad函数用指定字符实现左填充指定长度
* rpad函数同上
* replace函数替换

#### 8.1.2 数学函数

* round()函数四舍五入（前一个参数为输入值，后一个参数为保留小数点位数，负数代表正数位数）

如：

`select round(1236.567, -1) as result from emp;` 1240，保留到十位
`select round(1236.567, -2) as result from emp;` 1200，保留到百位

* rand()生成随机数

```mysql
#生成100以内的随机数
mysql> select round(rand()*100,0) from emp;
+---------------------+
| round(rand()*100,0) |
+---------------------+
|                  31 |
|                  16 |
|                  88 |
|                  93 |
|                 100 |
|                  20 |
|                   2 |
|                  51 |
|                  46 |
|                  77 |
|                  47 |
|                   3 |
|                  77 |
|                  75 |
+---------------------+
14 rows in set (0.00 sec)
```

* ifnull标记null的使用取代值

```mysql
# 求解年度工资
mysql> select ename,(sal+comm)*12 as yearsal from emp;
+--------+----------+
| ename  | yearsal  |
+--------+----------+
| SMITH  |     NULL |
| ALLEN  | 22800.00 |
| WARD   | 21000.00 |
| JONES  |     NULL |
| MARTIN | 31800.00 |
| BLAKE  |     NULL |
| CLARK  |     NULL |
| SCOTT  |     NULL |
| KING   |     NULL |
| TURNER | 18000.00 |
| ADAMS  |     NULL |
| JAMES  |     NULL |
| FORD   |     NULL |
| MILLER |     NULL |
+--------+----------+
14 rows in set (0.00 sec)

# 防止null出现
mysql> select ename,(sal+ifnull(comm,0))*12 as yearsal from emp;
+--------+----------+
| ename  | yearsal  |
+--------+----------+
| SMITH  |  9600.00 |
| ALLEN  | 22800.00 |
| WARD   | 21000.00 |
| JONES  | 35700.00 |
| MARTIN | 31800.00 |
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



**注意：Null只要参与了运算，结果为Null。为了避免这个想象，需要使用ifnull函数。用法：ifnull(数据，被当做哪个值)**

`select ename, (sal+ifnull(comm,0)) * 12 as yearsal from emp;` comm如果为null，当做0处理。

- ceil函数向上取整
- floor函数向下取整
- truncate函数截断
- mod函数取余

#### 8.1.3 日期函数

* **now函数**

获取当前时间，获取的时间带有：时分秒信息！是datetime类型的。

* curdate函数返回当前日期，不包含时间
* curtime函数返回当前时间，不包含日期
* **str_to_date函数**

**将字符串转化成日期类型**

**语法格式**：str_to_date(‘字符串日期’, ‘日期格式’)。

**str_to_date函数**可以把字符串**varchar转换成日期date**类型数据，通常使用在插入insert方面，因为插入的时候需要一个日期类型的数据，需要通过该函数将字符串转换成date。

* **date_format函数**
  * 1.**将日期类型转换成特定格式的字符串。**
    * `date_format(birth, '%Y/%d/%m')`
      mysql默认格式：%Y-%d-%m。
  * 2. **将日期转化为字符串格式**
* **timestampdiff(interval，datetime1，datetime2)函数**

比较的单位interval可以为以下数值
FRAC_SECOND。表示间隔是毫秒![在这里插入图片描述](https://img-blog.csdnimg.cn/cac059c7abaa42b289fda5224e6f978b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATXlfaGFpcg==,size_20,color_FFFFFF,t_70,g_se,x_16)

#### 8.1.4 其他函数

* case … when … then … when … then … else … end

例如：当员工的工作岗位是MANAGER的时候，工资上调10%，当工作岗位是SALESMAN的时候，工资上调50%，其他正常。（不是修改数据库，只是查询结果显示为工资上调）

```mysql
mysql> select
    -> ename,
    -> job,
    -> sal as oldsal,
    -> (case job when 'MANAGER' then sal*1.1 when 'SALESMAN' then sal*1.5 else sal end) as newsal
    -> from
    -> emp;
+--------+-----------+---------+---------+
| ename  | job       | oldsal  | newsal  |
+--------+-----------+---------+---------+
| SMITH  | CLERK     |  800.00 |  800.00 |
| ALLEN  | SALESMAN  | 1600.00 | 2400.00 |
| WARD   | SALESMAN  | 1250.00 | 1875.00 |
| JONES  | MANAGER   | 2975.00 | 3272.50 |
| MARTIN | SALESMAN  | 1250.00 | 1875.00 |
| BLAKE  | MANAGER   | 2850.00 | 3135.00 |
| CLARK  | MANAGER   | 2450.00 | 2695.00 |
| SCOTT  | ANALYST   | 3000.00 | 3000.00 |
| KING   | PRESIDENT | 5000.00 | 5000.00 |
| TURNER | SALESMAN  | 1500.00 | 2250.00 |
| ADAMS  | CLERK     | 1100.00 | 1100.00 |
| JAMES  | CLERK     |  950.00 |  950.00 |
| FORD   | ANALYST   | 3000.00 | 3000.00 |
| MILLER | CLERK     | 1300.00 | 1300.00 |
+--------+-----------+---------+---------+
14 rows in set (0.00 sec)
```



- version()
- database()
- user()
- format()

### 8.2 分组函数

也叫多行处理函数，多个输入对应一个输出

* count计数
* sum求和
* avg平均值
* max最大值
* min最小值

**注意：分组函数在使用的时候必须先进行分组，然后才能使用。如果没有对数据进行分组，整张表默认为一组。**

```mysql
mysql> select job,sum(sal) from emp group by job;
+-----------+----------+
| job       | sum(sal) |
+-----------+----------+
| CLERK     |  4150.00 |
| SALESMAN  |  5600.00 |
| MANAGER   |  8275.00 |
| ANALYST   |  6000.00 |
| PRESIDENT |  5000.00 |
+-----------+----------+
5 rows in set (0.00 sec)
```

分组函数的注意事项：

* 1，分组函数自动忽略null
* 2.**分组函数中`count(*)`和`count(具体字段)`有什么区别？**
  * count(具体字段)：表示统计该字段下所有不为null的元素的总数
  * count(*)：统计表当中的总行数。只要有一行数据count则++。
* 3.**分组函数不能直接使用在where子句中**

`select ename,sal from emp where sal > min(sal);` 错误写法（需要使用having）

* 4.**所有的分组函数可以组合起来一起用。**

> select sum(sal),min(sal),max(sal),avg(sal),count(*) from emp;

## 9.连接查询

* Q:什么是连接查询？
  * 从一张表中单独查询，称为单表查询。
    emp表和dept表联合起来查询数据，从emp表中取员工名字，从dept中取部门名字。这种跨表查询，多张表联合起来查询数据，称为连接查询。
* 连接查询的分类
  * 1.根据语法的年代分类：
    * SQL92：1992年的时候出现的语法
    * SQL99：1999年的时候出现的语法
      **我们重点学习SQL99。**
  * 2.根据表的连接方式分类：
    * **内连接**：等值连接、非等值连接、自连接
    * **外连接**：左外连接、右外连接
    * **全连接（略不讲）**

### 9.1 笛卡尔积现象

当两张表进行连接查询，没有任何条件限制的时候，最终查询结果条数，是两张表条数的乘积，这种现象被称为笛卡尔积现象。（笛卡尔积发现的，这是一个数学现象。）

两张表连接没有任何条件限制：`select ename,dname from emp,dept;` ename的一条记录会去匹配dname的所有记录。

Q:如何避免笛卡尔积呢？

A：连接的时候添加条件，满足这个条件的记录被筛选出来！

```mysql
mysql> select ename, dname from emp,dept where emp.deptno = dept.deptno;
+--------+------------+
| ename  | dname      |
+--------+------------+
| SMITH  | RESEARCH   |
| ALLEN  | SALES      |
| WARD   | SALES      |
| JONES  | RESEARCH   |
| MARTIN | SALES      |
| BLAKE  | SALES      |
| CLARK  | ACCOUNTING |
| SCOTT  | RESEARCH   |
| KING   | ACCOUNTING |
| TURNER | SALES      |
| ADAMS  | RESEARCH   |
| JAMES  | SALES      |
| FORD   | RESEARCH   |
| MILLER | ACCOUNTING |
+--------+------------+
14 rows in set (0.00 sec)

## 也可使用别名
mysql> select e.ename, d.dname from emp e, dept d where e.deptno = d.deptno;
+--------+------------+
| ename  | dname      |
+--------+------------+
| SMITH  | RESEARCH   |
| ALLEN  | SALES      |
| WARD   | SALES      |
| JONES  | RESEARCH   |
| MARTIN | SALES      |
| BLAKE  | SALES      |
| CLARK  | ACCOUNTING |
| SCOTT  | RESEARCH   |
| KING   | ACCOUNTING |
| TURNER | SALES      |
| ADAMS  | RESEARCH   |
| JAMES  | SALES      |
| FORD   | RESEARCH   |
| MILLER | ACCOUNTING |
+--------+------------+
14 rows in set (0.00 sec)
```

### 9.2 内连接

#### 9.2.1等值查询

查询每个员工所在部门名称，显示员工名和部门名：

```mysql
SQL92语法：
select 
	e.ename,d.dname
from
	emp e, dept d
where
	e.deptno = d.deptno;

```

**SQL92缺点**：结构不清晰，表的连接条件，和后期进一步筛选的条件，都放在where后面。

```mysql
SQL99语法：
select
	e.ename, d.dname
from
	emp e
join
	dept d
on
	e.deptno = d.deptno;
```

**SQL99优点**：表连接的条件是独立的，连接之后，如果还需要进一步筛选，再往后继续添加where。

```mysql
select
	...
from
	a
inner join  //inner可以省略，带着inner可读性更好
	b
on
	a和b的连接条件
where
	筛选条件
```

因为a和b的**连接条件是等量关系**，所以被称为**等值连接**。

#### 9.2.2 非等值连接

找出每个员工的薪资等级，要求显示员工名、薪资、薪资等级：

> mysql> select * from emp;
> mysql> select * from salgrade;

格式

```mydql
select 
	e.ename, e.sal, s.grade 
from 
	emp e 
join 
	salgrade s 
on 
	e.sal between s.losal and s.hisal;
```



```mysql
mysql> select e.ename,e.sal,s.grade from emp e join salgrade s on e.sal between s.losal and hisal;
+--------+---------+-------+
| ename  | sal     | grade |
+--------+---------+-------+
| SMITH  |  800.00 |     1 |
| ALLEN  | 1600.00 |     3 |
| WARD   | 1250.00 |     2 |
| JONES  | 2975.00 |     4 |
| MARTIN | 1250.00 |     2 |
| BLAKE  | 2850.00 |     4 |
| CLARK  | 2450.00 |     4 |
| SCOTT  | 3000.00 |     4 |
| KING   | 5000.00 |     5 |
| TURNER | 1500.00 |     3 |
| ADAMS  | 1100.00 |     1 |
| JAMES  |  950.00 |     1 |
| FORD   | 3000.00 |     4 |
| MILLER | 1300.00 |     2 |
+--------+---------+-------+
14 rows in set (0.00 sec)
```

条件不是一个等量关系，所以称为非等值连接。

#### 9.2.3 自连接

查询员工的上级领导，要求显示员工名和对应的领导名：

```mysql
mysql> select empno,ename,mgr from emp;
+-------+--------+------+
| empno | ename  | mgr  |
+-------+--------+------+
|  7369 | SMITH  | 7902 |
|  7499 | ALLEN  | 7698 |
|  7521 | WARD   | 7698 |
|  7566 | JONES  | 7839 |
|  7654 | MARTIN | 7698 |
|  7698 | BLAKE  | 7839 |
|  7782 | CLARK  | 7839 |
|  7788 | SCOTT  | 7566 |
|  7839 | KING   | NULL |
|  7844 | TURNER | 7698 |
|  7876 | ADAMS  | 7788 |
|  7900 | JAMES  | 7698 |
|  7902 | FORD   | 7566 |
|  7934 | MILLER | 7782 |
+-------+--------+------+
14 rows in set (0.00 sec)

mysql> select a.ename as '员工名' ,b.ename as '领导名'
    -> from emp a
    -> join emp b
    -> on a.mgr = b.empno;
+--------+--------+
| 员工名 | 领导名 |
+--------+--------+
| SMITH  | FORD   |
| ALLEN  | BLAKE  |
| WARD   | BLAKE  |
| JONES  | KING   |
| MARTIN | BLAKE  |
| BLAKE  | KING   |
| CLARK  | KING   |
| SCOTT  | JONES  |
| TURNER | BLAKE  |
| ADAMS  | SCOTT  |
| JAMES  | BLAKE  |
| FORD   | JONES  |
| MILLER | CLARK  |
+--------+--------+
```

**自连接技巧：一张表看成两张表**，a表的mgr就是b表的empno

### 9.3 外连接

语法结构

```mysql
mysql> select
    -> e.ename,d.dname
    -> from
    -> emp e
    -> right join
    -> dept d
    -> on
    -> e.deptno = d.deptno;
+--------+------------+
| ename  | dname      |
+--------+------------+
| MILLER | ACCOUNTING |
| KING   | ACCOUNTING |
| CLARK  | ACCOUNTING |
| FORD   | RESEARCH   |
| ADAMS  | RESEARCH   |
| SCOTT  | RESEARCH   |
| JONES  | RESEARCH   |
| SMITH  | RESEARCH   |
| JAMES  | SALES      |
| TURNER | SALES      |
| BLAKE  | SALES      |
| MARTIN | SALES      |
| WARD   | SALES      |
| ALLEN  | SALES      |
| NULL   | OPERATIONS |
+--------+------------+
15 rows in set (0.02 sec)

```

**right代表**：将join关键字右边的这张表看成主表，主要是为了将这张表的数据全部查询出来，捎带着关联查询左边的表。在外连接当中，两张表连接，产生了主次关系。

带有right的是右外连接，带有left的是左外连接；写法可以互换。

**结论：外连接的查询结果条数一定是 >= 内连接的查询结果条数。**

example:查询每个员工的上级领导，要求显示所有员工和名字和领导名。

```mysql
mysql> select a.ename as '员工名',b.ename as '领导名' from emp a left join emp b on a.mgr = b.empno;
+--------+--------+
| 员工名 | 领导名 |
+--------+--------+
| SMITH  | FORD   |
| ALLEN  | BLAKE  |
| WARD   | BLAKE  |
| JONES  | KING   |
| MARTIN | BLAKE  |
| BLAKE  | KING   |
| CLARK  | KING   |
| SCOTT  | JONES  |
| KING   | NULL   |
| TURNER | BLAKE  |
| ADAMS  | SCOTT  |
| JAMES  | BLAKE  |
| FORD   | JONES  |
| MILLER | CLARK  |
+--------+--------+
14 rows in set (0.00 sec)
```

### 9.4 多表连接

语法：

```mysql
select
	...
from
	a
join 
	b
on
	a和b的连接条件
join
	c
on
	a和c的连接条件
join
	d
on
	a和d的连接条件

```

一条SQL中内连接和外连接可以混合，都可以出现。

**案例：找出每个员工的部门名称以及工资等级，还有上级领导，要求显示员工名、领导名、部门名、薪资、薪资等级。**

```mysql
mysql> select
    -> e.ename,d.dname,e.sal,s.grade
    -> from
    -> emp e
    -> join
    -> dept d
    -> on
    -> e.deptno = d.deptno
    -> join
    -> salgrade s
    -> on
    ->  e.sal between s.losal and s.hisal
    -> left join
    -> emp l
    -> on
    -> e.mgr = l.empno;
+--------+------------+---------+-------+
| ename  | dname      | sal     | grade |
+--------+------------+---------+-------+
| SMITH  | RESEARCH   |  800.00 |     1 |
| ALLEN  | SALES      | 1600.00 |     3 |
| WARD   | SALES      | 1250.00 |     2 |
| JONES  | RESEARCH   | 2975.00 |     4 |
| MARTIN | SALES      | 1250.00 |     2 |
| BLAKE  | SALES      | 2850.00 |     4 |
| CLARK  | ACCOUNTING | 2450.00 |     4 |
| SCOTT  | RESEARCH   | 3000.00 |     4 |
| KING   | ACCOUNTING | 5000.00 |     5 |
| TURNER | SALES      | 1500.00 |     3 |
| ADAMS  | RESEARCH   | 1100.00 |     1 |
| JAMES  | SALES      |  950.00 |     1 |
| FORD   | RESEARCH   | 3000.00 |     4 |
| MILLER | ACCOUNTING | 1300.00 |     2 |
+--------+------------+---------+-------+
14 rows in set (0.00 sec)
```

### 9.5 子查询

select语句中嵌套select语句，被嵌套的select语句称为子查询。

语法结构如下：

```mysql
select
	..(select).
from
	..(select).
where
	..(select).

```

案例；

* where子句中的子查询

example：**找出比最低工资高的员工姓名和工资**

实现思路：

第一步：查询最低工资是多少

> select min(sal) from emp;

第二步：找出>800的

> select ename,sal from emp where sal > 800;

第三步：合并

> select ename,sal from emp where sal > (select min(sal) from emp);

* from子句中的子查询

**注意：from后面的子查询，可以将子查询的查询结果当做一张临时表。**

案例：找出每个岗位的平均工资的薪资等级。

第一步：找出每个岗位的平均工资（按照岗位分组求平均值）

> select job,avg(sal) from emp group by job;

第二步：把以上的查询结果就当做一张真实存在的表t

> select * from salgrade; s表

第三步：t表和s表进行表连接

```mysql
mysql> select
    -> t.*, s.grade
    -> from
    -> (select job,avg(sal) as avgsal from emp group by job) t  
    -> join
    -> salgrade s
    -> on
    -> t.avgsal between s.losal and s.hisal; 
+-----------+-------------+-------+
| job       | avgsal      | grade |
+-----------+-------------+-------+
| CLERK     | 1037.500000 |     1 |
| SALESMAN  | 1400.000000 |     2 |
| MANAGER   | 2758.333333 |     4 |
| ANALYST   | 3000.000000 |     4 |
| PRESIDENT | 5000.000000 |     5 |
+-----------+-------------+-------+
5 rows in set (0.00 sec)

```

* select 子句中的子查询

### 9.6 Union合并查询结果集

案例：查询工作岗位是MANAGER和SALESMAN的员工：

>select ename,job from emp where job = 'MANAGER' or job = 'SALESMAN';
>
>select ename,job from emp where job in('MANAGER', 'SALESMAN');

以上两种查询语句都是正确的，但是这也可以使用union来实现相同的效果

>select ename, job from emp where job = 'MANAGER'
>union
>select ename, job from emp where job = 'SALESMAN';

**union的效率更高**。对于表连接来说，每连接一次新表，则匹配的次数满足笛卡尔积，成倍的翻。
但是union可以减少匹配的次数。在减少匹配次数的情况下，还可以完成两个结果集的拼接。



**举个例子：**
a连接b连接c，a、b、c各10条记录，
匹配次数是：1000
使用union：a连接b：10`*`10 -->100条记录，a连接c：10`*`10 -->100条记录。
匹配次数：100 + 100 = 200次。(做到了优化效率)



**union在使用的时候注意事项**：

* 1.进行结果集合并的时候，要求两个结果集的列数相同。
* 2.结果集合并时列和列的数据类型要相同。

### 9.7 limit 限制数量

作用：将查询结果集的一部分取出来。通常使用在分页查询当中。分页的作用是为了提高用户的体验，不用一次查询全部。

**使用**：`limit startIndex, length`
startIndex是起始下标，从0开始；length是长度

例子：

* 1.按照薪资降序，取出排名在前5名的员工：

```mysql
select
	ename, sal
from
	emp
order by
	sal desc
limit 5; //取前五

# 效果
mysql> select
    -> ename, sal
    -> from
    -> emp
    -> order by
    -> sal desc
    -> limit 5; //取前五
+-------+---------+
| ename | sal     |
+-------+---------+
| KING  | 5000.00 |
| SCOTT | 3000.00 |
| FORD  | 3000.00 |
| JONES | 2975.00 |
| BLAKE | 2850.00 |
+-------+---------+
5 rows in set (0.01 sec)
```

**注意：mysql当中limit在order by之后执行！**

* 2.取出工资排名在[3-5]名的员工：

```mysql
select
	ename, sal
from
	emp
order by
	sal desc
limit
	2, 3; //2代表起始位置从下标2开始，3就是三条记录 [3-5]

# 效果
mysql> select
    -> ename, sal
    -> from
    -> emp
    -> order by
    -> sal desc
    -> limit
    -> 2, 3;
+-------+---------+
| ename | sal     |
+-------+---------+
| FORD  | 3000.00 |
| JONES | 2975.00 |
| BLAKE | 2850.00 |
+-------+---------+
3 rows in set (0.00 sec)
```

## 10.DDL语句

### 10.1 建表

```mysql
create table 表名 (
	字段名1 数据类型,
	字段名2 数据类型,
	字段名3 数据类型
);
```

**表名：建议以 t_或者tbl_开始，可读性强。**

* mysql中常见的数据类型有：

  * varchar（最长255）：可变长度的字符串。比较智能，会根据实际的数据长度动态分配空间。

  * char（最长255）：定长字符串。不管实际的数据长度是多少，分配固定长度的空间去存储数据，使用不恰当会导致空间的浪费。

    > 优点：不需要动态分配空间，速度快。
    > 缺点：使用不当会导致空间的浪费。

  * int（最长11）
  * bigint
  * float
  * double
  * date：短日期
  * datetime：长日期
  * clob：字符大对象，最多可以存储4G的字符串。比如：存储一篇文章。超过255个字符的都要采用clob来存储。
  * blob：二进制大对象。专门用来存储图片、声音、视频等流媒体数据。往blog类型字段上插入数据的时候，需要使用IO流。

例子：

* 创建一个学生表

```mysql
create table t_student (
	no int,
	name varchar(32),
	sex char(1) default 'm', //可以指定默认值
	age int(3),
	email varchar(255)
);

# 实例
mysql> create table t_student( no int, name varchar(32), sex char(1) default 'm', age int(3), emal varchar(255) );
Query OK, 0 rows affected, 1 warning (0.01 sec)

mysql> desc t_student;
+-------+--------------+------+-----+---------+-------+
| Field | Type         | Null | Key | Default | Extra |
+-------+--------------+------+-----+---------+-------+
| no    | int          | YES  |     | NULL    |       |
| name  | varchar(32)  | YES  |     | NULL    |       |
| sex   | char(1)      | YES  |     | m       |       |
| age   | int          | YES  |     | NULL    |       |
| emal  | varchar(255) | YES  |     | NULL    |       |
+-------+--------------+------+-----+---------+-------+
5 rows in set (0.00 sec)
```

* 创建一个用户vip表；

```mysql
CREATE TABLE IF NOT EXISTS user_info_vip (
    id int(11) PRIMARY KEY AUTO_INCREMENT NOT NULL COMMENT '自增ID',
    uid int unique NOT NULL COMMENT '用户ID',
    nick_name varchar(64)  COMMENT '昵称',
    achievement int(11)  default 0 COMMENT '成就值',
    level int(11) COMMENT '用户等级',
    job varchar(32)  COMMENT '职业方向',
    register_time datetime default CURRENT_TIMESTAMP COMMENT '注册时间'
)CHARACTER SET utf8 COLLATE utf8_general_ci;

# 实例
mysql> CREATE TABLE IF NOT EXISTS user_info_vip (     id int(11) PRIMARY KEY AUTO_INCREMENT NOT NULL COMMENT '自增ID',     uid int unique NOT NULL COMMENT '用户ID',     nick_name varchar(64)  COMMENT '昵称',     achievement int(11)  default 0 COMMENT '成就值',     level int(11) COMMENT '用户等级',     job varchar(32)
COMMENT '职业方向',     register_time datetime default CURRENT_TIMESTAMP COMMENT '注册时间' )CHARACTER SET utf8 COLLATE utf8_general_ci;
Query OK, 0 rows affected, 5 warnings (0.01 sec)

mysql> desc user_info_vip;
+---------------+-------------+------+-----+-------------------+-------------------+
| Field         | Type        | Null | Key | Default           | Extra             |
+---------------+-------------+------+-----+-------------------+-------------------+
| id            | int         | NO   | PRI | NULL              | auto_increment    |
| uid           | int         | NO   | UNI | NULL              |                   |
| nick_name     | varchar(64) | YES  |     | NULL              |                   |
| achievement   | int         | YES  |     | 0                 |                   |
| level         | int         | YES  |     | NULL              |                   |
| job           | varchar(32) | YES  |     | NULL              |                   |
| register_time | datetime    | YES  |     | CURRENT_TIMESTAMP | DEFAULT_GENERATED |
+---------------+-------------+------+-----+-------------------+-------------------+
7 rows in set (0.00 sec)
```

### 10.2 删除表

> 删除表：drop table if exists t_student;

### 10.3 快速建表

> create table emp2 as select * from emp;

原理：将一个查询结果当做一张表新建，相当于复制了这张表。表创建出来，同时表中的数据也存在了。

> create table mytable as select empno,ename from emp where job = 'MANAGER'; 只复制表中一些字段

### 10.4 表结构的增删改（略）

使用 alter 操作：添加、删除、修改一个字段。

不常用：

* 第一：在实际开发中，需求一旦确定之后，表被设计好，很少会再去进行表结构的修改。因为开发进行中的时候，修改表结构成本比较高。修改表的结构对应的java代码就需要进行大量的修改。这个责任应该由设计人员来承担。
* 第二：由于修改表结构的操作很少，所以我们不需要掌握，如果有一天真的要修改表的结构，可以使用工具。
* 第三：修改表结构的操作是不需要写到java程序中的。

## 11.DML语句

### 11.1 insert语句

* 语法格式：insert into 表名(字段1, 字段2, 字段3, …) values(值1, 值2, 值3, …);

**注意：字段名和值要一一对应，数量要对应，数据类型要对应。**

**insert语句中的字段名可以省略**，省略字段名等于都写上了，所以**值也要都写上**。

例如：

> insert into t_student values(2, 'lisi', 'f', 20, 'lisi@qq.com');

* **插入日期类型**

首先创建一个目标表t_user

```mysql
create table t_user(
	id int,
	name varchar(32),
	birth date
);
```

插入对应的数据

>`insert into t_user(id,name,birth) values(1, 'zhangsan', str_to_date('01-10-1990','%d-%m-%Y'));`
>mysql的日期格式：%Y 年、%m 月、%d日、%h 时、%i 分、%s 秒。

**注意：如果提供的日期字符串是这个格式：%Y-%m-%d，str_to_date函数可以省略。**

> `insert into t_user(id,name,birth) values(1, 'zhangsan', '1990-10-01');`

* **查询日期类型**

mysql默认使用%Y-%m-%d的格式。

```mysql
mysql> select name,birth from t_user;
+-----------+------------+
| name      | birth      |
+-----------+------------+
| zhangsan  | 1990-10-01 |
| zhangsan1 | 1990-10-01 |
+-----------+------------+
2 rows in set (0.01 sec)
```

如果不想用这个格式，可以使用date_format函数：

> select id,name,date_format(birth, '%m/%d/%Y') as birth from t_user;

```mysql
mysql> select id,name,date_format(birth, '%m/%d/%Y') as birth from t_user;
+------+-----------+------------+
| id   | name      | birth      |
+------+-----------+------------+
|    1 | zhangsan  | 10/01/1990 |
|    2 | zhangsan1 | 10/01/1990 |
+------+-----------+------------+
2 rows in set (0.00 sec)
```

* date和datetime两个类型的区别

  * date是短日期：只包括年月日信息。
  * datetime是长日期：包括年月日时分秒信息。

  实例：删除原来的表，新建一个表

  ```mysql
  drop table if exists t_user;
  create table t_user(
  	id int,
  	name varchar(32),
  	birth date, //短日期
  	create_time datetime  //长日期，这条记录的创建时间
  );
  ```

  mysql短日期默认格式：`%Y-%m-%d`
  mysql长日期默认格式：`%Y-%m-%d %h:%i:%s`
  `insert into t_user(id,name,birth,create_time) values(1, 'lisi', '1990-10-01', '2020-03-18 15:49:50');`
  可以结合now()函数使用，now()函数返回当前时间并且带有时分秒：

  `insert into t_user(id,name,birth,create_time) values(1, 'lisi', '1990-10-01', now());`

* 一次插入多条数据

```mysql
insert into t_user(id, name, birth, create_time) values
(1, 'zs', '1980-10-11', now()),
(2, 'lisi', '1981-10-11', now()),
(3, 'wangwu', '1982-10-11', now());
```

* 将结果插入到表中

  > insert into dept_bak select * from dept;

* **replace into … values**

![在这里插入图片描述](https://img-blog.csdnimg.cn/0ee61fd16ec14149a90ffc7afb53013a.png)

### 11.2 update 语句

**语法格式**：update 表名 set 字段名1 = 值1, 字段名2 = 值2, 字段名3 = 值3 … where 条件;
**注意：没有条件限制会导致所有数据全部更新。**

### 11.3 delete语句

**语法格式**：delete from 表名 where 条件;
**注意：没有条件，整张表的数据会全部删除！**

> `delete from dept_bak;` 删除表中所有的数据

**删除的原理**：表中的数据被删除了，但是这个数据在硬盘上的真实存储空间不会被释放。
优点：支持回滚，后悔了可以再恢复。

缺点：删除效率比较低。

* 快速删除表中数据

使用：`truncate table dept_bak;`（属于DDL操作）
`truncate tb_name`：全部删除（表清空，包含自增计数器重置）

**原理**：物理删除，表被一次截断，效率较高。
优点：快速。
缺点：不支持回滚。

**说明**：如果一张表非常大，比如有上亿条记录，使用delete执行可能需要1个小时才能删除完，效率低。选择使用truncate删除表中的数据，只需要不到1秒钟的时间就删除完，效率较高。但是在使用truncate之间，必须仔细询问客户是否真的要删除，并警告删除之后不可恢复。

## 12.约束

**constraint**：在创建表的时候，我们可以给表中的字段加上一些约束，来保证这个表中数据的完整性、有效性！

**约束的作用就是为了保证：表中的数据有效！**

**约束包括**：

* 非空约束：not null
* 唯一性约束：primary key
* 主键约束：primary key
* 外键约束：foreign key
* 检查约束：check（mysql不支持，Oracle支持）

所以，mysql重点学习4个类型的约束。

* **非空约束：not null**

非空约束所约束的字段不能为null。

```mysql
drop table if exists t_vip;
create table t_vip(
	id int,
	name varchar(255) not null
);
```

```mysql
mysql> drop table if exists t_vip;
Query OK, 0 rows affected, 1 warning (0.07 sec)

mysql> create table t_vip(
    -> id int,
    -> name varchar(255) not null
    -> );
Query OK, 0 rows affected (0.05 sec)

mysql> insert into t_vip(id) value(1);
ERROR 1364 (HY000): Field 'name' doesn't have a default value
```

上面实例标记为not null的name字段不能为空

* **唯一性约束：unique**

唯一性约束unique约束的字段不能重复，但是可以为NULL。

```my
mysql> drop table if exists t_vip;
Query OK, 0 rows affected (0.00 sec)

mysql> create table t_vip(
    -> id int,
    -> name varchar(255) unique,
    -> email varchar(255) 
    -> );
Query OK, 0 rows affected (0.01 sec)

mysql> insert into t_vip(id,name,email) values(1,'jiaoery','jiaoery@qq.com');
Query OK, 1 row affected (0.01 sec)

mysql> insert into t_vip(id,name,email) values(2,'jiaoery','jiaoery1@qq.com');
ERROR 1062 (23000): Duplicate entry 'jiaoery' for key 't_vip.name'
```

如何让两个字段联合起来具备唯一性

**新需求：name和email两个字段联合起来具有唯一性！**

```mysql
ysql> drop table if exists t_vip;
Query OK, 0 rows affected, 1 warning (0.00 sec)

mysql> create table t_vip( id int, name varchar(255), email varchar(255), unique(name,email));
Query OK, 0 rows affected (0.01 sec)

mysql> insert into t_vip(id,name,email) values(1,'wangwu','wangwu@qq.com');
Query OK, 1 row affected (0.00 sec)

mysql> insert into t_vip(id,name,email) values(2,'wangwu','wangwu@sina.com');
Query OK, 1 row affected (0.00 sec)

mysql> insert into t_vip(id,name,email) values(3,'wangwu','wangwu@sina.com');
ERROR 1062 (23000): Duplicate entry 'wangwu-wangwu@sina.com' for key 't_vip.name'

```

两个字段同时一样，才插入失败。

**unique和not null 可以联合**

```mysql
mysql> drop table if exists t_vip;
Query OK, 0 rows affected (0.01 sec)

mysql> create table t_vip(
    -> id int,
    -> name varchar(255) not null unique
    -> );
Query OK, 0 rows affected (0.00 sec)

mysql> desc t_vip;
+-------+--------------+------+-----+---------+-------+
| Field | Type         | Null | Key | Default | Extra |
+-------+--------------+------+-----+---------+-------+
| id    | int          | YES  |     | NULL    |       |
| name  | varchar(255) | NO   | PRI | NULL    |       |
+-------+--------------+------+-----+---------+-------+
2 rows in set (0.02 sec)
```

可以看到name的类型为PRI（主键）。
在mysql中，如果一个字段同时被 not null 和 unique 约束的话，该字段自动变成主键字段。**（但Oracle不一样）**

* **主键约束（primary key，简称PK）**

主键约束的相关术语：主键约束、主键字段、主键值。
**主键值是每一行记录的唯一标识**

**注意：任何一张表都应该有主键，没有主键，表无效！**

主键的特征：not null +unique（不为空，同时不能重复）

添加主键约束：

```mysql
create table t_vip(
	id int primary key,
	name varchar(255)
);

create table t_vip(
	id int,
	name varchar(255),
	primary key(id)
);

```

复合主键**（几个字段联合做主键）**

```mysql
create table t_vip(
	id int,
	name varchar(255),
	email varchar(255),
	primary key(id, name) //表级约束是联合的意思
);
insert into t_vip(id,name,email) values(1,'zhangsan','zs@123.com');
insert into t_vip(id,name,email) values(1,'lisi','ls@123.com');
```



上面的是正确的写法，下面是错误的写法（老版本的sql可以）

```mysql
create table t_vip(
	id int primary key,
	name varchar(255) primary key,
	email varchar(255)
);
```

错误写法：这不是联合。一张表不能添加两个主键。



**!!!**

**主键值建议使用的类型**：
int、bigint、char等类型，不建议使用varchar做主键。
主键值一般都是数字，一般都是定长的！

主键也可以这样分类：
**自然主键**：主键值是一个自然数，和业务没关系。
**业务主键**：主键值和业务紧密关联，例如拿银行卡账号做主键值。



在mysql中，有一种机制，可以帮助我们自动维护一个主键值：

```mysql
ysql> drop table if exists t_vip;
Query OK, 0 rows affected (0.02 sec)

mysql> create table t_vip( id int primary key auto_increment, name varchar(255)
);
Query OK, 0 rows affected (0.01 sec)

mysql> insert into t_vip(name) values('lisi');
Query OK, 1 row affected (0.01 sec)

mysql> insert into t_vip(name) values('zhangsan');
Query OK, 1 row affected (0.00 sec)

mysql> select * from t_vip;
+----+----------+
| id | name     |
+----+----------+
|  1 | lisi     |
|  2 | zhangsan |
+----+----------+
2 rows in set (0.01 sec)
```



* **外键约束（foreign key，简称FK）**

设计数据库表，描述“班级和学生”信息：

班级一张表、学生一张表



**注意：添加了外键约束，表与表之间产生了父子关系。**
t_class是父表、t_student是子表。
删除表的顺序：先删子，再删父。
创建表的顺序：先创建父，再创建子。
删除数据的顺序：先删子，再删父。
插入数据的顺序：先插入父，再插入子。

```mysql
drop table if exists t_student; //先删子
drop table if exists t_class;

create table t_class(
	classno int primary key,
	classname varchar(255)
);
create table t_student(
	no int primary key auto_increment,
	name varchar(255),
	cno int,
	foreign key(cno) references t_class(classno)
);
```

```mysql
mysql> drop table if exists t_student;
Query OK, 0 rows affected, 1 warning (0.00 sec)

mysql> drop table if exists t_class;
Query OK, 0 rows affected, 1 warning (0.00 sec)

mysql> create table t_class(
    -> classno int primary key,
    -> classname varchar(255)
    -> );
Query OK, 0 rows affected (0.01 sec)

mysql> create table t_student(
    -> no int primary key auto_increment,
    -> name varchar(255),
    -> cno int,
    -> foreign key(cno) references t_class(classno)
    -> );
Query OK, 0 rows affected (0.01 sec)
```

注意：子表中的外键引用的附表中的某个字段，被引用的这个字段**不一定是主键**。但**至少具有unique唯一性**。
**外键可以为null。**



## 13.存储引擎

### 13.1 存储引擎介绍

存储引擎是MySQL中特有的一个术语，其他数据库中没有。
实际上存储引擎是一个表存储/组织数据的方式，不同的存储引擎，表存储数据的方式不同。

查看表：`show create table t_vip;`

实例效果

```mysql
mysql> show create table t_vip;
+-------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Table | Create Table                                                                                                                                                                                            |
+-------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| t_vip | CREATE TABLE `t_vip` (
  `id` int NOT NULL AUTO_INCREMENT,
  `name` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci |
+-------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
```

mysql默认的存储引擎是：InnoDB
mysql默认的字符编码方式是：utf8

那么我们可以直接制定创建表的引擎和文字格式

```mysql
create table t_product(
	id int primary key,
	name varchar(255)
) engine=InnoDB default charset=gbk;

```

* 查看mysql支持哪些存储引擎

命令：`show engines \G`

```mysql
mysql> show engines \G
*************************** 1. row ***************************
      Engine: ARCHIVE
     Support: YES
     Comment: Archive storage engine
Transactions: NO
          XA: NO
  Savepoints: NO
*************************** 2. row ***************************
      Engine: BLACKHOLE
     Support: YES
     Comment: /dev/null storage engine (anything you write to it disappears)
Transactions: NO
          XA: NO
  Savepoints: NO
*************************** 3. row ***************************
      Engine: MRG_MYISAM
     Support: YES
     Comment: Collection of identical MyISAM tables
Transactions: NO
          XA: NO
  Savepoints: NO
*************************** 4. row ***************************
      Engine: FEDERATED
     Support: NO
     Comment: Federated MySQL storage engine
Transactions: NULL
          XA: NULL
  Savepoints: NULL
*************************** 5. row ***************************
      Engine: MyISAM
     Support: YES
     Comment: MyISAM storage engine
Transactions: NO
          XA: NO
  Savepoints: NO
*************************** 6. row ***************************
      Engine: PERFORMANCE_SCHEMA
     Support: YES
     Comment: Performance Schema
Transactions: NO
          XA: NO
  Savepoints: NO
*************************** 7. row ***************************
      Engine: InnoDB
     Support: DEFAULT
     Comment: Supports transactions, row-level locking, and foreign keys
Transactions: YES
          XA: YES
  Savepoints: YES
*************************** 8. row ***************************
      Engine: MEMORY
     Support: YES
     Comment: Hash based, stored in memory, useful for temporary tables
Transactions: NO
          XA: NO
  Savepoints: NO
*************************** 9. row ***************************
      Engine: CSV
     Support: YES
     Comment: CSV storage engine
Transactions: NO
          XA: NO
  Savepoints: NO
9 rows in set (0.00 sec)
```

### 13.2 mysql常用引擎

#### 13.2.1 **MyISAM存储引擎**

**用这种引擎创建出来的表，具有以下特征**：
使用三个文件表示每个表：
格式文件 – 存储表结构的定义（mytable.frm）
数据文件 – 存储表行的内容（mytable.MYD）
索引文件 – 存储表上索引（mytable.MYI）
可被转换为压缩、只读表来节省空间

#### 13.2.2 **InnoDB存储引擎**

mysql默认的存储引擎，同时也是一个重量级的存储引擎。
InnoDB支持事务，支持数据库崩溃后自动恢复机制。
InnoDB存储引擎最重要的特点是：非常安全

它管理的表主要具有以下特征：

* 1.每个InnoDB表在数据库目录中以 .frm格式文件表示。
* 2.InnoDB表空间tablespace被用于存储表的内容
* 3.提供一组用来记录事务性活动的日志文件
* 4.用commit（提交）、SAVEPOINT及ROLLBACK（回滚）支持事务处理。
* 5.提供全ACID兼容
* 6.在MySQL服务器崩溃后提供自动恢复。
* 7.多版本（MVCC）和行级锁定
* 8.支持外键及引用的完整性，包括级联删除和更新。InnoDB最大的特点就是支持事务：保证数据的安全，效率不高，并且也不能压缩，不能转换为只读，不能很好的节省存储空间

#### 13.2.3 **MEMORY存储引擎**

使用MEMORY存储引擎的表，其数据存储在内存中，且行的长度固定，这两个特点使得MEMORY存储引擎非常快。

MEMORY存储引擎管理的表具有下列特征：
在数据库目录内，每个表均以 .frm格式的文件表示。
表数据及索引被存储在内存中。（目的就是快，查询快！）
表级锁机制。
不能包含TEXT或BLOB字段。

MEMORY存储引擎以前被称为HEAP引擎。

优点：查询效率最高，不需要和硬盘交互。
缺点：不安全，关机后数据消失。因为数据和索引都是在内存当中。

## 14.事务（必须掌握和理解）

### 14.1 认识事务

一个事务其实就是一个完整的业务逻辑，是一个最小的工作单元。要么同时成功，要么同时失败，不可再分。

什么是一个完整的事物逻辑？

>假设转账，从A账户向B账户转账10000
>A账户的钱减去10000（update语句）
>B账户的钱加上10000（update语句）
>这就是一个完整的业务逻辑

这两个update语句要求必须同时成功或者同时失败，才能保证钱是正确的。

**只有DML语句才会有事务这一说，其他语句和事务无关。**
insert、delete、update。
一旦你的操作涉及到增、删、改，那么就一定要考虑安全问题。

### 14.2 事务的实现

因为做某件事情的时候，需要多条DML语句共同联合起来才能完成，所以需要事务的存在。如果任何一件复杂的事情都能一条DML语句搞定，那么事务就没有存在的价值了。
所以，本质上，一个事务就是多条DML语句同时成功，或者同时失败！

事务：就是批量的DML语句同时成功，或者同时失败。

**Q：事务是怎么做到多条DML语句同时成功和同时失败的？**

nnoDB存储引擎：提供一组用来**记录事务性活动的日志文件**。

结构如下：

>事务开启：
>insert
>delete
>update
>…
>事务结束

提交事务：清空事务性活动的日志文件，将数据全部彻底持久化到数据库表中。提交事务标志着，事务的结束，并且是一种全部成功的结束。

回滚事务：将之前所有的DML操作全部撤销，并且清空事务性活动的日志文件。回滚事务标志着，事物的结束，并且是一种全部失败的结束。

**提交和回滚实现**
提交事务：commit;
回滚事务：rollback;（回滚是回滚到上一次的提交点）
事务对应的英文单词是：transaction



mysql中默认事务是自动提交的，就是每执行一条DML语句，则提交一次。

开启事务，则会关闭自动提交机制

```mysql
start transaction;
insert into dept_bak values(10, 'abc', 'tj');
rollback; //回滚，则上面的insert语句被撤销
```

### 14.3 事务的4个特性

**ACID**

**原子性（A）**：说明事务是最小的工作单元，不可再分。

**一致性（C）**：所有事务要求，在同一个事务当中，所有操作必须同时成功，或者同时失败，以保证数据的一致性。

**隔离性（I）**：A事务和B事务之间具有一定的隔离。相当于多线程并发访问的每个线程。

**持久性（D）**：事务最终结束的一个保障。事务提交，就相当于将没有保存到硬盘上的数据保存到硬盘上！



**重点研究事务的隔离性**

形象的说，A教室和B教室中间有一道墙，这道墙可以很厚，也可以很薄。越厚，隔离级别越高。

事务与事务之间有4个隔离级别：

* **读未提交**：read uncommitted（最低隔离级别）
  事务A可以读取到事务B未提交的数据。（没有提交就读取到了）
  这种隔离级别存在的问题就是：脏读现象（Dirty Read），我们称堵到了脏数据。
  这种隔离级别一般都是理论上的，大多数的数据库隔离级别都是二挡起步。
* **读已提交**：read commited
  事务A只能读取到事务B提交之后的数据。解决了脏读现象。
  这种隔离级别存在的问题是：不可重复读取数据。
  这种隔离级别是比较真实的数据，每次读到的数据是绝对真实的。
  Oracle数据库默认的隔离级别是：read commited
* **可重复读**：repeatable read
  提交之后也读不到，永远读取的都是刚开启事务时的数据。
  事务A开启之后，不管多久，每一次在事务A中读取到的数据，都是一致的。即使事务B将数据已经修改，并且提交了，事务A读取到的数据还是没有发生改变，这就是可重复读。
  存在的问题：可能会出现幻影读。每一次读取到的数据都是幻影，不真实。
  mysql中默认的事务隔离级别就是这个。
* **序列化/串行化**：serializable（最高的隔离级别）
  这是最高隔离级别，效率最低。解决了所有的问题。
  这种隔离级别表示事务排队，不能并发！
  synchronized，线程同步（事务同步）
  每次读取到的数据都是最真实的，但是效率是最低的。



**Example:验证事务隔离级别**(设置后一定要退出一次)

查看隔离级别：`select @@tx_isolation`(8.0版本以下) `SELECT @@global.transaction_isolation;`(8.0及以上版本)

```mysql
mysql> SELECT @@global.transaction_isolation;
+--------------------------------+
| @@global.transaction_isolation |
+--------------------------------+
| REPEATABLE-READ                |
+--------------------------------+
1 row in set (0.00 sec)
```

* **验证：read uncommitted**（验证脏读现象）

设置全局隔离级别为“读未提交”：`set global transaction isolation level read uncommitted;`（设置之后一定要退出一次才可以）

窗口A

```mysql
ysql> start transaction;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from t_user;
+------+----------+------------+---------------------+
| id   | name     | birth      | create_time         |
+------+----------+------------+---------------------+
|    1 | lisi     | 1990-10-01 | 2020-03-18 15:49:50 |
|    2 | lisi2    | 1990-10-01 | 2022-05-17 16:42:49 |
|    3 | jiaoery  | 1992-09-08 | 2022-05-24 18:20:00 |
|    4 | jiaoery4 | 1992-09-08 | 2022-05-24 18:31:32 |
|    5 | jiaoery5 | 1992-09-08 | 2022-05-24 18:39:33 |
|    6 | jiaoery6 | 1992-09-08 | 2022-05-24 18:55:08 |
+------+----------+------------+---------------------+
6 rows in set (0.00 sec)

# 这里暂停去窗口B操作
mysql> select * from t_user;
+------+----------+------------+---------------------+
| id   | name     | birth      | create_time         |
+------+----------+------------+---------------------+
|    1 | lisi     | 1990-10-01 | 2020-03-18 15:49:50 |
|    2 | lisi2    | 1990-10-01 | 2022-05-17 16:42:49 |
|    3 | jiaoery  | 1992-09-08 | 2022-05-24 18:20:00 |
|    4 | jiaoery4 | 1992-09-08 | 2022-05-24 18:31:32 |
|    5 | jiaoery5 | 1992-09-08 | 2022-05-24 18:39:33 |
|    6 | jiaoery6 | 1992-09-08 | 2022-05-24 18:55:08 |
|    7 | jiaoery7 | 1992-09-08 | 2022-05-25 11:15:45 |
+------+----------+------------+---------------------+
7 rows in set (0.00 sec)

#B rollbac后进行操作
mysql> select * from t_user;
+------+----------+------------+---------------------+
| id   | name     | birth      | create_time         |
+------+----------+------------+---------------------+
|    1 | lisi     | 1990-10-01 | 2020-03-18 15:49:50 |
|    2 | lisi2    | 1990-10-01 | 2022-05-17 16:42:49 |
|    3 | jiaoery  | 1992-09-08 | 2022-05-24 18:20:00 |
|    4 | jiaoery4 | 1992-09-08 | 2022-05-24 18:31:32 |
|    5 | jiaoery5 | 1992-09-08 | 2022-05-24 18:39:33 |
|    6 | jiaoery6 | 1992-09-08 | 2022-05-24 18:55:08 |
+------+----------+------------+---------------------+
6 rows in set (0.00 sec)
```



窗口B

```mysql
mysql> start transaction;
Query OK, 0 rows affected (0.00 sec)

mysql> insert into t_user values(7,'jiaoery7','1992-09-08',now());
Query OK, 1 row affected (0.00 sec)

mysql> select * from t_user;
+------+----------+------------+---------------------+
| id   | name     | birth      | create_time         |
+------+----------+------------+---------------------+
|    1 | lisi     | 1990-10-01 | 2020-03-18 15:49:50 |
|    2 | lisi2    | 1990-10-01 | 2022-05-17 16:42:49 |
|    3 | jiaoery  | 1992-09-08 | 2022-05-24 18:20:00 |
|    4 | jiaoery4 | 1992-09-08 | 2022-05-24 18:31:32 |
|    5 | jiaoery5 | 1992-09-08 | 2022-05-24 18:39:33 |
|    6 | jiaoery6 | 1992-09-08 | 2022-05-24 18:55:08 |
|    7 | jiaoery7 | 1992-09-08 | 2022-05-25 11:15:45 |
+------+----------+------------+---------------------+
7 rows in set (0.00 sec)

#A查询之后进行的操作
mysql> rollback;
Query OK, 0 rows affected (0.00 sec)
```

解释：打开两个DOS窗口，分别进入数据库，一个DOS窗口开启一个事务。开启事务后，事务B中进行的操作还未提交，在事务A中就能查询到。(无法复现了)



* **验证：read commited**

设置全局隔离级别为“读已提交”：`set global transaction isolation level read committed;`（记得退出）

事务A

```mysql
mysql> start transaction;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from t_user;
+------+----------+------------+---------------------+
| id   | name     | birth      | create_time         |
+------+----------+------------+---------------------+
|    1 | lisi     | 1990-10-01 | 2020-03-18 15:49:50 |
|    2 | lisi2    | 1990-10-01 | 2022-05-17 16:42:49 |
|    3 | jiaoery  | 1992-09-08 | 2022-05-24 18:20:00 |
|    4 | jiaoery4 | 1992-09-08 | 2022-05-24 18:31:32 |
|    5 | jiaoery5 | 1992-09-08 | 2022-05-24 18:39:33 |
|    6 | jiaoery6 | 1992-09-08 | 2022-05-24 18:55:08 |
+------+----------+------------+---------------------+
6 rows in set (0.00 sec)
# B执行插入命令，未提交
mysql> select * from t_user;
+------+----------+------------+---------------------+
| id   | name     | birth      | create_time         |
+------+----------+------------+---------------------+
|    1 | lisi     | 1990-10-01 | 2020-03-18 15:49:50 |
|    2 | lisi2    | 1990-10-01 | 2022-05-17 16:42:49 |
|    3 | jiaoery  | 1992-09-08 | 2022-05-24 18:20:00 |
|    4 | jiaoery4 | 1992-09-08 | 2022-05-24 18:31:32 |
|    5 | jiaoery5 | 1992-09-08 | 2022-05-24 18:39:33 |
|    6 | jiaoery6 | 1992-09-08 | 2022-05-24 18:55:08 |
+------+----------+------------+---------------------+
6 rows in set (0.00 sec)

# B提交插入命令
mysql> select * from t_user;
+------+----------+------------+---------------------+
| id   | name     | birth      | create_time         |
+------+----------+------------+---------------------+
|    1 | lisi     | 1990-10-01 | 2020-03-18 15:49:50 |
|    2 | lisi2    | 1990-10-01 | 2022-05-17 16:42:49 |
|    3 | jiaoery  | 1992-09-08 | 2022-05-24 18:20:00 |
|    4 | jiaoery4 | 1992-09-08 | 2022-05-24 18:31:32 |
|    5 | jiaoery5 | 1992-09-08 | 2022-05-24 18:39:33 |
|    6 | jiaoery6 | 1992-09-08 | 2022-05-24 18:55:08 |
|    7 | jiaoery7 | 1992-09-08 | 2022-05-25 11:27:14 |
+------+----------+------------+---------------------+
7 rows in set (0.00 sec)
```



事务B

```mysql
mysql> start transaction;
Query OK, 0 rows affected (0.00 sec)

mysql> insert into t_user values(7,'jiaoery7','1992-09-08',now());
Query OK, 1 row affected (0.00 sec)

# A查询一次后再提交
mysql> commit;
Query OK, 0 rows affected (0.00 sec)
```



解释：事务B进行操作未提交，在事务A查询不到修改后的记录。只有事务B提交了，才能在事务A查询到。（事务B提交，事务A才能查到）

* **验证：repeatable read**

设置全局隔离级别为“可重复读”：`set global transaction isolation level repeatable read;`（记得要退出）

事务A

```mysql
mysql> start transaction;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from t_user;
+------+----------+------------+---------------------+
| id   | name     | birth      | create_time         |
+------+----------+------------+---------------------+
|    1 | lisi     | 1990-10-01 | 2020-03-18 15:49:50 |
|    2 | lisi2    | 1990-10-01 | 2022-05-17 16:42:49 |
|    3 | jiaoery  | 1992-09-08 | 2022-05-24 18:20:00 |
|    4 | jiaoery4 | 1992-09-08 | 2022-05-24 18:31:32 |
|    5 | jiaoery5 | 1992-09-08 | 2022-05-24 18:39:33 |
|    6 | jiaoery6 | 1992-09-08 | 2022-05-24 18:55:08 |
|    7 | jiaoery7 | 1992-09-08 | 2022-05-25 11:27:14 |
+------+----------+------------+---------------------+
7 rows in set (0.00 sec)

mysql> select * from t_user;
+------+----------+------------+---------------------+
| id   | name     | birth      | create_time         |
+------+----------+------------+---------------------+
|    1 | lisi     | 1990-10-01 | 2020-03-18 15:49:50 |
|    2 | lisi2    | 1990-10-01 | 2022-05-17 16:42:49 |
|    3 | jiaoery  | 1992-09-08 | 2022-05-24 18:20:00 |
|    4 | jiaoery4 | 1992-09-08 | 2022-05-24 18:31:32 |
|    5 | jiaoery5 | 1992-09-08 | 2022-05-24 18:39:33 |
|    6 | jiaoery6 | 1992-09-08 | 2022-05-24 18:55:08 |
|    7 | jiaoery7 | 1992-09-08 | 2022-05-25 11:27:14 |
+------+----------+------------+---------------------+
7 rows in set (0.00 sec)

mysql> commit;
Query OK, 0 rows affected (0.00 sec)

#只有commit后才可以查阅得到
mysql> select * from t_user;
+------+----------+------------+---------------------+
| id   | name     | birth      | create_time         |
+------+----------+------------+---------------------+
|    1 | lisi     | 1990-10-01 | 2020-03-18 15:49:50 |
|    2 | lisi2    | 1990-10-01 | 2022-05-17 16:42:49 |
|    3 | jiaoery  | 1992-09-08 | 2022-05-24 18:20:00 |
|    4 | jiaoery4 | 1992-09-08 | 2022-05-24 18:31:32 |
|    5 | jiaoery5 | 1992-09-08 | 2022-05-24 18:39:33 |
|    6 | jiaoery6 | 1992-09-08 | 2022-05-24 18:55:08 |
|    7 | jiaoery7 | 1992-09-08 | 2022-05-25 11:27:14 |
|    8 | jiaoery8 | 1992-09-08 | 2022-05-25 11:34:44 |
+------+----------+------------+---------------------+
8 rows in set (0.00 sec)
```



事务B

```mysql
mysql> start transaction;
Query OK, 0 rows affected (0.00 sec)

mysql> insert into t_user values(8,'jiaoery8','1992-09-08',now());
Query OK, 1 row affected (0.00 sec)

mysql> commit;
Query OK, 0 rows affected (0.00 sec)
```



解释：事务B操作完提交后，在事务A中查询到的数据，依然是开启事务之前的数据。不论事务B做什么操作，提交几次，只要事务A没提交，查询到的就是以前的。（事务B提交，事务A提交了才能查到）



* **验证：serializable**

设置全局隔离级别为“序列化”：`set global transaction isolation level serializable;`

事务A

```mysql
mysql> start transaction;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from t_user;
+------+----------+------------+---------------------+
| id   | name     | birth      | create_time         |
+------+----------+------------+---------------------+
|    1 | lisi     | 1990-10-01 | 2020-03-18 15:49:50 |
|    2 | lisi2    | 1990-10-01 | 2022-05-17 16:42:49 |
|    3 | jiaoery  | 1992-09-08 | 2022-05-24 18:20:00 |
|    4 | jiaoery4 | 1992-09-08 | 2022-05-24 18:31:32 |
|    5 | jiaoery5 | 1992-09-08 | 2022-05-24 18:39:33 |
|    6 | jiaoery6 | 1992-09-08 | 2022-05-24 18:55:08 |
|    7 | jiaoery7 | 1992-09-08 | 2022-05-25 11:27:14 |
|    8 | jiaoery8 | 1992-09-08 | 2022-05-25 11:34:44 |
+------+----------+------------+---------------------+
8 rows in set (0.00 sec)

mysql> select * from t_user;
+------+----------+------------+---------------------+
| id   | name     | birth      | create_time         |
+------+----------+------------+---------------------+
|    1 | lisi     | 1990-10-01 | 2020-03-18 15:49:50 |
|    2 | lisi2    | 1990-10-01 | 2022-05-17 16:42:49 |
|    3 | jiaoery  | 1992-09-08 | 2022-05-24 18:20:00 |
|    4 | jiaoery4 | 1992-09-08 | 2022-05-24 18:31:32 |
|    5 | jiaoery5 | 1992-09-08 | 2022-05-24 18:39:33 |
|    6 | jiaoery6 | 1992-09-08 | 2022-05-24 18:55:08 |
|    7 | jiaoery7 | 1992-09-08 | 2022-05-25 11:27:14 |
|    8 | jiaoery8 | 1992-09-08 | 2022-05-25 11:34:44 |
+------+----------+------------+---------------------+
8 rows in set (0.00 sec)

mysql> commit;
Query OK, 0 rows affected (0.00 sec)
```

事务B

```mysql
mysql> start transaction;
Query OK, 0 rows affected (0.00 sec)

mysql> start transaction;
Query OK, 0 rows affected (0.00 sec)

mysql> insert into t_user values(9,'jiaoery9','1992-09-08',now());
ERROR 1205 (HY000): Lock wait timeout exceeded; try restarting transaction
mysql> insert into t_user values(9,'jiaoery9','1992-09-08',now());
ERROR 1205 (HY000): Lock wait timeout exceeded; try restarting transaction

# A未提交，B会卡住；但是A commit后 B会立即执行
mysql> insert into t_user values(9,'jiaoery9','1992-09-08',now());
Query OK, 1 row affected (2.60 sec)
```



解释：在事务A中进行一些操作，没提交，事务B就去查询结果，会卡在那里。当事务A提交，事务B会立马出现结果。相当于同步了。

## 15.索引（index）

索引是在数据库表的字段上添加的，是为了提高查询效率存在的一种机制。
一张表的一个字段可以添加一个索引，当然，多个字段联合起来也可以添加索引。
索引相当于一本书的目录，是为了缩小扫描范围而存在的一种机制。

根据SQL语句扫描：`select * from t_user where name = 'jack'`，扫描name查找。
如果name字段上没有添加索引（目录），或者说没有给name字段创建索引，MySQL会进行全扫描，会将name字段上的每一个值都比对一遍。效率比较低。

MySQL在查询方面主要就是两种方式：
第一种方式：全表扫描。
第二种方式：根据索引检索。

在mysql数据库当中，索引也是需要排序的，并且这个索引的排序和TreeSet数据结构相同。TreeSet底层是一个自平衡的二叉树！在mysql当中索引是一个B-Tree数据结构。
遵循左小右大原则存放，采用中序遍历方式遍历取数据。

### 15.1 索引的实现原理

假设有一张用户表：t_user

![在这里插入图片描述](https://img-blog.csdnimg.cn/4237c2ce018b4395b908c565aabd2575.png)

说明
1.在任何数据库当中主键上都会自动添加索引对象，id字段上自动有索引。另外在mysql当中，一个字段上如果有unique约束的话，也会自动创建索引对象。
2.在任何数据库当中，任何一张表的任何一条记录在硬盘存储上都有一个硬盘的物理存储编号。
3.在mysql当中，索引是一个单独的对象，不同的存储引擎以不同的形式存在。在MyISAM存储引擎中，索引存储在一个 .MYI文件中；在InnoDB存储引擎中，索引存储在一个逻辑名称叫做tablespace当中；在MEMORY存储引擎当中，索引被存储在内存当中。不管索引存储在哪里，索引在mysql当中都是一个树的形式存在。（自平衡二叉树：B-Tree）

**什么情况下，我们会考虑给字段添加索引呢？**

1.数据量庞大（到底有多么庞大算庞大，这个需要测试，因为每一个硬件环境不同）。
2.该字段经常出现在where的后面，以条件的形式存在，也就是说这个字段总是被扫描。
3.该字段很少的DML操作，因为DML之后，索引需要重新排序。

建议不要随意添加索引，因为索引也是需要维护的，太多的话反而会降低系统的性能。建议通过主键查询，建议通过unique约束的字段进行查询，效率会比较高。

### 15.2 创建和删除索引

* 创建索引：

> create index emp_ename_index on emp(ename);

给emp表的ename字段添加索引，起别名：emp_ename_index

* 删除索引

> drop index emp_ename_index on emp;

将emp表上的emp_ename_index索引对象删除。



在mysql当中，查看一个SQL语句是否使用了索引进行检索：
`explain select * from emp where ename = 'KING';`

### 15.3 索引失效

下面几种情况下，索引将会失效

#### 15.3.1 "%"开头的模糊查询

> select * from emp where ename like '%T';

尽量避免模糊查询的时候以"%"开头，这是一种优化的策略。
可以用SQL语句查看：

> explain select * from emp where ename like '%T';

#### 15.3.2 使用or

如果使用or，那么要求or两边的条件字段都要有索引，索引才生效。如果其中一个字段没有索引，那么另一个字段上的索引也会失效。所以不建议使用or，可以使用union来代替。

#### 15.3.3 使用复合索引

使用复合索引的时候，**要使用左侧的列查找**，索引才生效。使用右侧，则失效。
复合索引：两个字段，或者更多的字段联合起来添加一个索引。

`create index emp_job_sal_index on emp(job,sal);`

使用左侧：`explain select * from emp where job = 'MANAGER';` 生效

使用右侧：`explain select * from emp where sal= 800;` 失效

#### 15.3.4 索引列参与运算

在where当中索引列参加了运算，索引失效。

`explain select * from emp where sal+1 = 800;`

#### 15.3.5 索引列使用了函数

在where当中索引列使用了函数。
`explain select * from emp where lower(ename) = 'smith';`

**索引是各种数据库进行优化的重要手段，优化的时候优先考虑的因素就是索引。**

**索引在数据库当中分了很多类**：

单一索引：一个字段上添加索引。
复合索引：两个字段或者更多的字段上添加索引。
主键索引：主键上添加索引。
唯一性索引：具有unique约束的字段上添加索引。
（注意：唯一性比较弱的字段上添加索引的用处不大。）



例：需要在examination_info表创建以下索引，规则如下：
在duration列创建**普通索引** idx_duration、在exam_id列创建**唯一性索引** uniq_idx_exam_id、在tag列创建**全文索引** full_idx_tag。

```mysql
create index idx_duration on examination_info(duration);
create unique index uniq_idx_exam_id on examination_info(exam_id);
create fulltext index full_idx_tag on examination_info(tag);
```

## 16.视图

View：站在不同的角度去看待同一份数据

创建视图对象：`create view dept2_view as select * from dept2;`

删除视图对象：`drop view dept2_view;`

**注意：只有DQL语句(Data Query Language)才能以view的形式创建。**

**create view view_name as 这里的语句必须是DQL语句**;

我们可以面向视图对象进行增删改查，对视图对象的增删改查，会导致原表被操作！（视图的特点：通过对视图的操作，会影响到原表数据

```mysql
create view
	emp_dept_view
as
select
   e.ename, e.sal, d.dname
from
   emp e
join
   dept d 
on
  e.deptno = d.deptno;
  
  //修改视图，原表的数据也会修改
  update emp_dept_view set sal = 1000 where dname = 'ACCOUNTING'; 
```

**视图对象在实际开发中到底有什么用？**

假设有一条非常复杂的SQL语句，而这条SQL语句需要在不同的位置上反复使用。
每一次使用这个SQL语句的时候都需要重新编写，很长，很麻烦。
可以把这条复杂的SQL语句以视图对象的形式新建。
在需要编写这条SQL语句的位置直接使用视图对象，可以大大简化开发，并且利于后期的维护，因为修改的时候也只需要修改一个位置就行，只需要修改视图对象映射的SQL语句。

我们以后面向视图开发的时候，使用视图的时候可以像使用table一样，可以对视图进行增删改查等操作。
视图不是在内存当中，视图对象也是存储在硬盘上，不会消失。

**再次提醒**：
视图对应的语句只能是DQL语句。
但是视图对象创建完成之后，可以对视图进行增删改查等操作。

## 17.DBA常用命令

* 新建用户
* 授权
* 回收权限
* **数据的备份（数据的导入和导出）**

**数据的导出**：

注意：在windows的dos命令窗口中
`mysqldump 数据库>D:\数据库.sql -uroot -p密码`
也可以导出指定的表：
`mysqldump bjpowernode emp>D:\bjpowernode .sql -uroot -p密码`



- **数据导入**：
  注意：需要先登录到mysql数据库服务器上。
  然后创建数据库：create database bjpowernode;
  使用数据库：use bjpowernode;
  然后初始化数据库：`source D:\bjpowernode.sql`

## 18.数据库三范式

数据库设计范式：数据库表的设计依据，教你怎么进行数据库表的设计。

* 第一范式：要求任何一张表必须有主键，每一个字段原子性不可再分。
* 第二范式：建立在第一范式的基础上，要求所有非主键字段完全依赖主键，不要产生部分依赖。
* 第三范式：建立在第二范式的基础上，要求所有非主键字段直接依赖主键，不要产生传递依赖。

三范式是面试官经常问的，所以一定要熟记在心！

设计数据库表的时候，按照以上的范式进行，可以避免表中数据的冗余，避免空间的浪费

### 18.1 第一范式

最核心，最重要的范式，所有表的设计都需要满足。

1.有主键；2.每个字段原子性不可再分

![在这里插入图片描述](https://img-blog.csdnimg.cn/de737cf1a8bf45599db858a99e710063.png)

不满足，没有主键，联系方式可以再分

![在这里插入图片描述](https://img-blog.csdnimg.cn/987ca7ae3981499fb63e8ab164297b34.png)

### 18.2 第二范式

建立在第一范式的基础上

1.所以有的非主键字段必须完全依赖主键;2.不要产生部分依赖

![在这里插入图片描述](https://img-blog.csdnimg.cn/a6bf686d545749489b893a9f49fe0221.png)

产生部分依赖，数据冗余了。

为了上一张表满足第二范式，需要三张表来表示多对对的关系

![在这里插入图片描述](https://img-blog.csdnimg.cn/e7932c2ed8af440eb408a651de2ce1c3.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATXlfaGFpcg==,size_19,color_FFFFFF,t_70,g_se,x_16)

多对多的设计：

三张表，关系表两个外键！

### 18.3 第三范式

建立在第二范式的基础上

所有非主键字段直接依赖主键，不要产生传递依赖

![在这里插入图片描述](https://img-blog.csdnimg.cn/a0d749355f4a4e5d85496997bfdad7e7.png)

以上表是一对多的关系，满足第二范式，因为主键不是复合主键，没有产生部分依赖。
但不满足第三范式，班级名称依赖班级编号，班级编号依赖学生编号，产生传递依赖。造成数据冗余。

![在这里插入图片描述](https://img-blog.csdnimg.cn/8cced6ae165f403aa3eef8fe945ad423.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATXlfaGFpcg==,size_15,color_FFFFFF,t_70,g_se,x_16)

**一对多的设计**：
两张表，多的表加外键！



**总结数据库表的设计**：

1. 一对一，第二张表外键唯一（fk+unique）
2. 一对多，两张表，多的表加外键！
3. 多对多，三张表，关系表两个外键！

**嘱咐：**
数据库设计三范式是理论上的，实践和理论有的时候有偏差。
最终的目的都是为了满足客户的需求，有的时候会拿冗余换执行速度。
因为在SQL中，表和表之间连接次数越多，效率越低。（笛卡尔积）
有的时候可能会存在冗余，但是为了减少表的连接次数，这样做也是合理的，并且对于开发人员来说，SQL语句的编写难度也会降低。

## 参考资料

MySQL查看和修改事务隔离级别：http://c.biancheng.net/view/7266.html
