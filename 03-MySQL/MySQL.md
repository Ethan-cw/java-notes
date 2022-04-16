# 1、初识MySQL

javaEE：企业级java开发 Web

前端（页面：展示，数据！） 

后台：连接点

* 连接数据库：JDBC MyBatis
* 链接前端（控制视图的跳转，给前端传递数据）

数据库：存数据，txt, Excel, word

>只会写代码，学好数据库，混饭吃；
>
>操作系统，数据结构与算法！不错的程序员！
>
>离散数学，数字电路，体系结构，编译原理，实战经验， 高级程序员

## 1.1 为什么学习数据库

1. 岗位需求
2. 大数据时代
3. 被迫需求，存数据
4. 数据库是所有软件体系最核心的存在 DBA 

## 1.2 什么是数据库

DB ：DataBase

概念：数据仓库，软件，安装在操作系统上的

作用：存储数据，管理数据

## 1.3 数据库分类

**关系型数据库**：表和表，行和列的关系进行数据的存储

* MySQL
* Oracle

**非关系型数据库**

* Redis，MongDB
* 对象存储，通过对象的自身的属性来决定。

****

**DBMS**：数据库管理系统

* 数据库的管理软件，科学有效的管理我们的数据，维护和获取数据。
* MySQL 数据库管理系统

## 1.4 MySQL

SQL语言，体积小，速度快，成本低，开放源码。

版本：5.7 稳定 企业多

最新：8.0

按照建议：

1. 尽可能用压缩包安装，尽量不要用exe安装，不好卸载。

##  1.5 安装MySQL

1. 解压压缩包
2. 把这个包，放到自己电脑环境下 F:\Environment\mysql-5.7.36
3. 添加环境变量 添加F:\Environment\mysql-5.7.36\bin 到PATH
4. 配置my.ini

```ini
# 设置为自己MYSQL的安装目录
basedir=F:\Environment\mysql-5.7.36\
# 设置为MYSQL的数据目录
datadir=F:\Environment\mysql-5.7.36\data\
port=3306
skip-grant-tables
```

5. 启动CMD管理员模式，运行所有的命令

   ```bash
   mysqld install
   mysqld --initialize-insecure --user=mysql
   ```

6. 启动mysql 

   ```bash
   net start mysql
   ```

7. 修改密码(密码可为空，注意-p后面没有任何字符)

   ```bash
   mysql -u root -p
   update mysql.user set authentication_string=password('123456') where user='root' and Host ='localhost';
   flush privileges;
   ```

8. 删除ini文件里的skip-grant-tables

9. exit推出mysql，重启mysql

   ```bash
   net stop mysql
   net start mysql
   
   mysql -u root -p
   # 输入密码123455成功
   ```

## 1.6 安装SQLyog

1. 安装完毕

2. 打开连接数据库

3. 新建一个数据库 school

   ```sql
   CREATE DATABASE `school`CHARACTER SET utf8 COLLATE utf8_general_ci; 
   ```

4. 新建一张表 student

   ```
   字段：id，name，age
   ```

5. 查看表 尝试添加记录

## 1.7 连接数据库

命令行连接

```sql
mysql -uroot -p123456 -- 连接数据库
-----------------------------------
show databases; -- 查看所有数据库
use school -- 切换数据库 show+名字
show tables; -- 查看数据库所有表
describe student; -- 显示数据库所有表的信息
creat database westos; 
exit -- 退出链接
-- 单行注释
/*
d
*/
```

# 2. 操作数据库

操作数据库 > 操作表 > 操作表的数据

关键字不区分大小写

## 2.1 操作数据库(了解)

1. 创建数据库

2. 删除数据库

3. 使用数据接

   ```sql
   CREATE DATABASE IF NOT EXISTS westos
   
   DROP DATABASE IF EXISTS westos
   
   USE school -- 如果表名和字段名是特殊字符 `user` tab上面那个键
   ```

   

4. 查看数据库

   ```sql
   SHOW DATABASES -- 查看所有数据库
   ```

## 2.2 数据库的列类型

> 数值：
>
> * tinyint    1个字节
> * smallint  2个字节
> * mediumint  3个字节
> * **int    4个字节  常用**
> * bigint 8个字节
> * float 4个字节
> * double 8个字节（精度问题！）
> * decimal 字符串形式的浮点数，金融计算一般使用这个
>
> 字符串
>
> * char 字符串 固定大小 0~255
> * **varchar 可变字符串 0~65535 常用的**
> * tingtext 微型文本  2^8-1
> * **text 文本串 2^16-1 保存大文本**
>
> 时间日期
>
> * date YYYY-MM-DD 日期
> * time HH:mm:ss 时间格式
> * **datetime YYYY-MM-DD HH:mm:ss 最常用的格式**
> * **timestamp 时间戳 1970.1.1到现在的毫秒数**
> * year 年份表示
>
> null
>
> * 没有值，未知
> * 不要使用NULL进行运算，若使用结果一定为NULL

## 2.3 数据库的字段属性（重点）

`Unsigned`：无符号的整数，声明该列不能为负数

`zerofill`：0填充的，不足的位数使用0填充: 5—>005

`自增`：自动在上一条记录+1 默认，通常设定为主键，可以设置初始值和步长

`非空`：NULL not NULL，设置为非空，则该列必须赋值

`默认`：设置默认值 例如：sex默认为男，不指定则为男

-------------------

每一个表都必须存在以下5个字段: 未来做项目的，表示字段存在的意义

`id` 主键

`version`乐观锁

`is_delete` 伪删除

`gmt_create` 创建时间

`gmt_update` 修改时间

## 2.4 创建数据库表

```sql
CREATE TABLE `student` (
  `id` INT(4) NOT NULL AUTO_INCREMENT COMMENT '学号',
  `name` VARCHAR(30) NOT NULL DEFAULT '匿名' COMMENT '姓名',
  `pwd` VARCHAR(20) NOT NULL DEFAULT '123456' COMMENT '密码',
  `sex` VARCHAR(2) NOT NULL DEFAULT '女' COMMENT '性别',
  `birthday` DATETIME DEFAULT NULL COMMENT '出生日期',
  `address` VARCHAR(100) DEFAULT NULL COMMENT '家庭住址',
  `email` VARCHAR(50) DEFAULT NULL COMMENT '邮箱',
  PRIMARY KEY (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8
```

<img src="C:\Users\Ethan\AppData\Roaming\Typora\typora-user-images\image-20211128164648648.png" alt="image-20211128164648648"  />

```sql
-- 常用命令
SHOW CREATE DATABASE school -- 查看创建数据库的语句
SHOW CREATE TABLE student -- 查看创建数据库表的语句
DESC student --查看表的结构
```

## 2.5 数据库表的类型

`INNODB` 默认使用

* 安全性高，事务的处理，多表多用户操作

`MYISAM`： 早些年使用

* 节约空间，速度快

| 区别       | MYISAM      | INNODB                            |
| ---------- | ----------- | --------------------------------- |
| 事务支持   | 不支持      | 支持                              |
| 数据行锁定 | 不支持 表锁 | 支持 行锁                         |
| 外键约束   | 不支持      | 支持                              |
| 全文索引   | 支持        | 不支持                            |
| 表空间大小 | 较小        | 较大，约2倍                       |
| 物理存储   | 三个文件    | *.frm 文件 已经上级目录ibdata文件 |

所有的数据库文件都在data目录下：

* 一个文件夹对应一个数据库

* 本质还是文件的存储
* *.frm 表结构的定义文件
* *.MYD 数据文件（data）
* *.MYI 索引文件（index）

**数据库编码默认不支持中文，utf8支持中文**

## 2.6 修改删除表

> 修改

```sql
-- 修改表名
ALTER TABLE student RENAME AS student1
-- 增加表的字段
ALTER TABLE student1 ADD phone INT(12)
--  修改表的字段
ALTER TABLE student1 MODIFY phone VARCHAR(12) -- 修改约束
ALTER TABLE student1 CHANGE phone phone1 VARCHAR(11) -- 字段重命名

-- 删除表的字段
ALTER TABLE student1 DROP phone1
```

> 删除

```sql
-- 删除表
DROP TABLE IF EXISTS student1
```

注意：

* 所有的创建删除+上判断语句
* ``字段名，使用这个包裹
* sql大小写不敏感，建议都小写
* 所有符号用英文的

# 3 MySQL数据管理

## 3.1 外键（了解即可）

> 方式一 创建表的时候，定义外键key，给这个外键添加约束

> 方式二 创建时候没有外键关系

```sql
ALTER TABLE `student`
ADD CONSTRAINT `FK_gradeid` FOREIGN KEY(`gradeid`) 
REFERENCES  `grade`(`gradeid`);
```

删除时候，存在外键引用的时候，被引用的表无法先直接删除

> 以上外键是数据库级别的外键，不建议使用，避免数据库过多造成困扰

**最佳实践**

>* 数据库就是单纯的表，只用来存数据，只有行和列
>* 想使用外键，用程序去实现

## 3.2 DML语言（全部记住）

数据库的意义：数据存储，数据管理

DML语言：数据操作语言

* insert
* update
* delete

## 3.3 添加

```sql
-- insert into 表名([字段1，字段2，字段3]) 
-- values('值1'),('值2'),('值3'),(...)
-- 一个括号代表一个完整的对象
-- 由于主键自增 省略主键(不写表的字段，默认一一匹配)
-- 一般写插入语句一一对应
-- 插入多个字段
INSERT INTO `student`(`studentno`,`studentname`) VALUES('12300','小米'),('12301','xiaohong')
```

## 3.4 修改

>update 修改谁（条件）set 原来的值=新值

```sql
-- 修改学员的名字
-- 不指定的条件的情况，会改动所有表
-- UPDATE 表名 set 列名 = 值,多个列名=值 where 条件;
UPDATE 'student' SET `name`='cw' WHERE id=1;

-- 设置多个属性，逗号隔开
UPDATE 'student' SET `name`='cw',`email`='515761009@qq.com' 
WHERE id=1;
```

条件：where子句 运算符

操作值返回bool值

| 操作符          | 含义       | 范围           | 结果  |
| --------------- | ---------- | -------------- | ----- |
| =               | 等于       | 5=6            | false |
| <>或者!=        | 不等于     | 5<>6           | true  |
| >,<,>=,<=       |            |                |       |
| BETWEEN 2 AND 5 | 在某个区间 | [2,5] 闭合区间 |       |
| AND             | 并且       |                |       |
| OR              | 或         |                |       |

注意：

* 数据库的列名一定要存在，带上``
* 条件，筛选的条件，如果没有指定，则会修改所有的列
* value，具体的值，也可以是一个变量 例如 CURRENT_TIME
* 多个设置的属性，使用，隔开

## 3.5 删除 

> delete 命令

```sql
-- 删除指定数据
DELETE FROM 'student' WHERE id =1 ;
```

> TRUNCATE 命令

清空一个数据库表，表的结构和索引约束不变

```sql
-- 清空某一张表
TRUNCATE `student`
```

相同点：

* 都可以删数据，不会删表结构

不同点

* TRUNCATE 自增列清0，不会影响事务。
* DELETE 删除，重启数据库
  * INNODB 自增列从1开始（存在内存当中，断电即失）
  * MYISAM 继续从上一个增量开始（存在文件当中，不会丢失）

# 4 DQL查询（最重点）

## 4.1 DQL及完整语句

数据查询语言

* 所有的查询操作都用他 Select
* 简单查询和复杂查询
* **数据库最核心的东西，使用频率最高**

```sql
SELECT [ALL | DISTINCT]
{* | table.* | [table.field1[as alias1][,table.field2[as alias2]][,...]]}
FROM table_name [as table_alias]
    [left | right | inner join table_name2]  -- 联合查询
    [WHERE ...]  -- 指定结果需满足的条件
    [GROUP BY ...]  -- 指定结果按照哪几个字段来分组
    [HAVING]  -- 过滤分组的记录必须满足的次要条件
    [ORDER BY ...]  -- 指定查询记录按一个或多个条件排序
    [LIMIT {[offset,]row_count | row_countOFFSET offset}];
    --  指定查询的记录从哪条至哪条
```

## 4.2 指定查询字段

```sql
-- 查询全部的学生 SELECT 字段 FROM 表
SELECT * FROM student;

-- 查询指定字段
-- 给结果使用别名 AS
-- 给表起别名 AS
SELECT `studentno` AS 学号,`studentname` AS 姓名 FROM student AS s;

-- 函数 Concat(a,b)
SELECT CONCAT('姓名：', `studentname`) AS 新名字 FROM student;
```

> 去重 distinct

```sql
-- 查询以下有哪些同学参加了考试
SELECT * FROM result; -- 查询全部的考试成绩

-- DISTINCT 去重
SELECT DISTINCT `studentno` FROM result
```

> 数据库的列

```sql
SELECT VERSION(); -- 查询系统版本 （函数）
SELECT 100*3-1 AS 结果; -- 用来计算（表达式）
SELECT @@auto_increment_increment; -- 查询自增的步长（变量）

-- 学员考试成绩+1分
SELECT `studentno`,`studentresult`+1 AS 提分后 FROM `result`
```

## 4.3 where 条件子句

作用：检索数据中`符合条件`的值

返回结果是bool值

> 逻辑运算符

| 运算符  | 语法    | 描述   |
| ------- | ------- | ------ |
| and &&  | a and b | 逻辑与 |
| or \|\| | a or b  | 逻辑或 |
| not !   | not a   | 逻辑非 |

```sql
-- ====================== where ===================
SELECT `studentno`,`studentresult` FROM `result`
WHERE `studentresult`>=80 AND `studentresult`<=100;

-- 模糊查询(区间)
SELECT `studentno`,`studentresult` FROM `result`
WHERE `studentresult` BETWEEN 60 AND 90;

-- 除了1000号学生以外的成绩
SELECT `studentno`,`studentresult` FROM `result`
WHERE NOT `studentresult` = 70 AND NOT `studentresult`=68;
```

> 模糊查询: 比较运算符

| 运算符      | 语法               | 描述                          |
| ----------- | ------------------ | ----------------------------- |
| IS NULL     | a IS NULL          | 是否为空                      |
| IS NOT NULL | a IS NOT NULL      | 是否不为空                    |
| BETWEEN     | A BETWEEN B AND C  | 是否在BC之间                  |
| LIKE        | A LIKE B           | SQL匹配，若A能匹配到B，则为真 |
| IN          | a IN (A1,A2,A3...) | 若存在里面则返回真            |

```sql
-- 模糊查询(区间)
SELECT `studentno`,`studentresult` FROM `result`
WHERE `studentresult` BETWEEN 60 AND 90;

-- 除了1000号学生以外的成绩
SELECT `studentno`,`studentresult` FROM `result`
WHERE NOT `studentresult` = 70 AND NOT `studentresult`=68;

-- ====================== 模糊查询 ===================

-- 查询姓赵的同学
-- like结合 %（0到任意个字符） _（代表一个字符）
SELECT `studentno`,`studentname` FROM `student`
WHERE `studentname` LIKE '赵%';

-- 查询名字中有强的同学
SELECT `studentno`,`studentname` FROM `student`
WHERE `studentname` LIKE '%强%';

-- ====== in ========= 具体的一个或者多个值
-- 查询指定的学号同学
SELECT `studentno`,`studentname` FROM `student`
WHERE  `studentno` IN (1001,1002);

-- 查询在北京朝阳的学生
SELECT * FROM `student`
WHERE  `address` IN ('北京朝阳');

-- 查询地址为空的学生 null
SELECT * FROM `student`
WHERE  `address` IS NULL;

-- 查询有出生日期的同学
SELECT * FROM `student`
WHERE `borndate` IS NOT NULL;
```

## 4.4 链表查询

> JOIN 对比

![img](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimg-blog.csdnimg.cn%2Fimg_convert%2F778c36ab164d512798aa9a9ce7877e10.png&refer=http%3A%2F%2Fimg-blog.csdnimg.cn&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1640694236&t=0795db09abf7e7bff477b1c445211008)

```sql
-- 参加了考试的同学（学号，姓名，科目编号，分数）
/*
1. 分析，查询的字段来自哪些表
2. 确定哪种连接方式 7中
3. 确定交叉点（哪个数据相同）
*/
-- join（连接的表）on 连接查询
-- where 等值查询
SELECT s.`studentno`,`studentname`,`studentresult`
FROM `student` AS s
INNER JOIN `result` AS r
WHERE s.`studentno`= r.`studentno`

SELECT s.`studentno`,`studentname`,r.`studentno`,`studentresult`
FROM `student` s
RIGHT JOIN `result` r
ON s.`studentno`= r.`studentno`
```

| 操作       | 描述                                     |
| ---------- | ---------------------------------------- |
| inner join | 如果表中至少有一个匹配就能返回行         |
| left join  | 会从左表中返回所有的行，即使右表没有匹配 |
| right join | 会从右表中返回所有的行，即使左表没有匹配 |

注意：

* from a left join b ----->left join以左表为基准，就是a

* from a rightjoin b ----->rightjoin以右表为基准，就是b

> 自连结

**自己的表和自己的表连接，核心：一张表拆成两张用即可**

``` sql
-- 查询父子信息
SELECT A.`categoryname` '父栏目', B.`categoryname` '子栏目'
FROM`category` A, `category` B
WHERE A.`categoryid` = B.`pid`;
```

SELECT完整语法

![image-20211128205655847](C:\Users\Ethan\AppData\Roaming\Typora\typora-user-images\image-20211128205655847.png)

## 4.5 分页和排序

````sql
-- =============== 分页limit和排序order by
-- 排序 ASC升序 DESC降序
-- ORDER BY 字段 怎么排
SELECT s.`studentno`,`studentname`,`studentresult`
FROM `student` AS s
INNER JOIN `result` AS r
WHERE s.`studentno`= r.`studentno`
ORDER BY `studentresult` DESC
LIMIT 1,2;
-- 每页只显示2条 起始值，页面大小
-- 网页应用 当前页 总的页数 页面大小
-- 网页放不下，缓解数据库 分页，瀑布流
-- 第一页 limit 0-5
-- 第二页 limit 5-5
-- 第N页  limit （N-1）*pagesize， pagesize
-- N代表当前页
-- 总页数=数据总数/页面大小
````

## 4.6 子查询

where（这个值是计算出来的）

本质：在` where`语句中嵌套一个子查询语句

```sql
-- ================ where =========================
-- 1、查询 高等数学-1，高等数学-2 的所有考试结果(学号，科目名字，成绩)，降序排列
-- 方式一 使用连接查询
SELECT `studentno`,s.`subjectno`,`studentresult`
FROM `result` r 
INNER JOIN `subject` s
ON r.`subjectno`=s.`subjectno`
WHERE s.`subjectno`=1 OR s.`subjectno`=2
ORDER BY r.`studentresult` DESC;

-- 方式二：使用子查询
SELECT `studentno`,`subjectno`,`studentresult`
FROM `result`
WHERE `subjectno` = (
	SELECT `subjectno`
	FROM `subject`
	WHERE `subjectname`='高等数学-2'
)
ORDER BY `studentresult` DESC;

-- 查询高等数学-1分数不小于80分的学生的学号和姓名
SELECT DISTINCT s.`studentno`,`studentname`
FROM `student` s
INNER JOIN `result` r
ON s.`studentno`=r.`studentno`
WHERE r.`studentresult`>=80 AND r.`subjectno`=(
	SELECT `subjectno` 
	FROM `subject`
	WHERE `subjectname` = '高等数学-1'
)

SELECT DISTINCT `studentno`,`studentname`
FROM `student` WHERE `studentno` IN (
	SELECT `studentno` FROM result
	WHERE studentresult>=80 
	AND subjectno = (
		SELECT subjectno 
		FROM `subject` 
		WHERE subjectname = '高等数学-1'
	)
);
```

# 5. MySQL函数

官网[MySQL :: MySQL 5.7 Reference Manual :: 12.1 Built-In Function and Operator Reference](https://dev.mysql.com/doc/refman/5.7/en/built-in-function-reference.html)

## 5.1 常用函数

```sql
-- ====================== 常用函数 ==============================
SELECT ABS(-8) -- 绝对值
SELECT CEILING(9.4) -- 向上取整
SELECT FLOOR(9,4) -- 向下取值
SELECT RAND() -- 返回一个随机数
SELECT SIGN(); -- 返回参数的符号 0-0 负数 -1 正数返回1

-- 字符串函数
SELECT CHAR_LENGTH('asdasdasdasd') -- 字符串长度
SELECT CONCAT('i','love','u')
SELECT INSERT('jjinhu',1,2,'asdcsdasd')
SELECT LOWER('KHFHasdasd')
SELECT UPPER('Tdsfdsfs')
SELECT INSTR('asdasdasdasdasda','sda')
SELECT REPLACE('jianchijiuchon','jiu','buhui')

-- 查询姓周的同学 换成邹
SELECT REPLACE(studentname, '周', '邹') FROM student
WHERE `studentname` LIKE '周%'

-- 时间和日期函数（记住）
SELECT CURRENT_DATE(); -- 获取当前日期
SELECT CURDATE()
SELECT NOW()
SELECT LOCALTIME()
SELECT SYSDATE()

SELECT YEAR(NOW())
SELECT MONTH(NOW())
SELECT DAY(NOW())
SELECT HOUR(NOW())
SELECT MINUTE(NOW())

-- 系统
SELECT SYSTEM_USER()
SELECT USER()
SELECT VERSION()
```



## 5.2 聚合函数(常用)

* COUNT()
* SUM()
* AVG()
* MAX()
* MIN()
* ...

```sql
-- ========================== 聚合函数 ===========
SELECT COUNT(`subjectname`) FROM `subject`; -- 忽略字段所有的null
SELECT COUNT(*) FROM `subject`; -- 不会忽略null 本质计算行数
SELECT COUNT(1) FROM `subject`; -- 不会忽略null 不会忽略null

SELECT SUM(`studentresult`) AS 总分 FROM result;
SELECT AVG(`studentresult`) AS 总分 FROM result;
SELECT MAX(`studentresult`) AS 总分 FROM result;
SELECT MIN(`studentresult`) AS 总分 FROM result;

-- 查询平均分大于80学生的平均分
SELECT `studentno`, 
AVG(`studentresult`) 平均分 , 
MAX(`studentresult`) 最高分, 
MIN(`studentresult`) 最低分 
FROM `result`
GROUP BY `studentno` -- 通过字段来分组
HAVING	平均分>=80;  -- 分组后过滤
```

## 5.3 数据库级别的MD5加密

MD5：

* 信息摘要算法，增强算法复杂度
* 不可逆
* 具体的值的MD5是一样的

```sql
-- 明文密码
INSERT INTO `testmd5`(`id`,`name`,`pwd`)
VALUES
(1,'张三','123456'),
(2,'李四','123456'),
(3,'王五','123456');

SELECT * FROM `testmd5`;

-- 加密
UPDATE testmd5 SET pwd=MD5(pwd) WHERE id=2;

SELECT * FROM `testmd5`;

-- 插入的时候加密
INSERT INTO `testmd5`(`id`,`name`,`pwd`)
VALUES
(4,'小明',MD5('123456'));

-- 如何校验，将用户传递进来的密码，进行MD5加密，然后比对加密后的值
SELECT * FROM `testmd5` WHERE `name`='小明' AND pwd = MD5('123456');
```

# 6. 事务

## 6.1 什么是事务

要么都成功，要么都失败

-------------------------

将一组SQL放到一个批次去执行

> 事务原则：ACID原则

* **原子性**：同时发生或者同时不发生
* **一致性**：最终一致性，双方的总钱数不变
* **隔离性**：多个用户同时操作，不会互相影响
  * **脏读**：一个事务读取了另一个事务未提交的数据
  * **不可重复读**：多次读取后结果不一致
  * **虚读，幻读**：读到别人新插入的数据
* **持久性**：事务结束后的数据不随着外界原因导致数据丢失，事务没有提交就恢复到原状，一旦提交了就持久到数据库

```sql
-- ===================== 事务 ======================
-- Mysql是默认开启事务自动提交的
-- 关闭
SET autocommit = 0; 
-- 开启（默认的）
SET autocommit = 1;

-- 手动处理事务
SET autocommit = 0; -- 先关闭自动保存
-- 事务开启
START TRANSACTION -- 标记事务的开始，以下是一组事务


-- 提交
COMMIT
-- 回滚
ROLLBACK

-- 事务结束
SET autocommit = 1; -- 事务结束，开启自动提交

-- 了解
SAVEPOINT 保存点名 -- 设置事务的一个保存点
ROLLBACK TO SAVEPOINT 保存点名 -- 回滚到保存点
RELEASE SAVEPOINT 保存点名 -- 撤销保存点
```

## 6.2 模拟转账

```sql
-- 模拟转账
CREATE DATABASE shop CHARACTER SET utf8 COLLATE utf8_general_ci 

USE shop

-- 建表
CREATE TABLE `account`(
  `id` INT(3) NOT NULL AUTO_INCREMENT,
  `name` VARCHAR(100) NOT NULL,
  `money` DECIMAL(9,2) NOT NULL,
  PRIMARY KEY (`id`)
)ENGINE=INNODB DEFAULT CHARSET=utf8;

-- 初始化数据
INSERT INTO account(`name`,`money`)
VALUES('A',2000.00),
('B',10000.00);

-- 模拟转账
SET autocommit = 0; -- 关闭自动提交

START TRANSACTION; -- 开启事务 （一组事务）

UPDATE account SET `money`=`money`-500 WHERE `name`='A'; -- A减500
UPDATE account SET `money`=`money`+500 WHERE `name`='B'; -- B加500

COMMIT; -- 提交事务，就会被持久化了

ROLLBACK; -- 回滚

SET autocommit = 1; -- 恢复自动提交
```

# 7 索引

>  Msql官方对索引的定义为：**索引（index）是帮助MySQL高效获取数据的数据结构**。
>
> 提取句子主干，就可以得到索引的本质：索引是数据结构。

## 7.1 索引的分类

> 在一个表中，主键索引只能有一个，唯一索引可以有多个

- 主键索引（primary key）
  - 唯一的标识，主键不可重复，只能有一个列作为主键
- 唯一索引 （unique key）
  - 避免重复的列出现，可以重复，多个列都可以标示为唯一索引
- 常规索引（key/index）
  - 默认的 index 或者key关键字来设置
- 全文索引（FullText）
  - 在特定的数据库引擎下才有，myisam
  - 快速定位数据

```sql
-- ============== 基础语法 =================
-- 显示所有索引信息
SHOW INDEX FROM student

-- 增加一个全文索引
ALTER TABLE school.student ADD FULLTEXT INDEX `studentname`(`studentname`)

-- explain 分析sql的执行情况
EXPLAIN SELECT * FROM student; -- 非全文索引
SELECT * FROM student WHERE MATCH(`studentname`) AGAINST('赵')

EXPLAIN SELECT * FROM student WHERE MATCH(`studentname`) AGAINST('赵')
```

## 7.2 测试索引

```sql
CREATE TABLE app_user (
  `id` BIGINT(20) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `name` VARCHAR(50)  DEFAULT '' COMMENT '用户昵称',
  `email` VARCHAR(50)  NOT NULL COMMENT '用户邮箱',
  `phone` VARCHAR(20)  DEFAULT '' COMMENT '手机号',
  `gender` TINYINT(4)  UNSIGNED DEFAULT '0' COMMENT '性别（0：男  1：女）',
  `password` VARCHAR(100)  NOT NULL COMMENT '密码',
  `age` TINYINT(4)  DEFAULT '0' COMMENT '年龄',
  `create_time` DATETIME  DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `update_time` TIMESTAMP NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
  PRIMARY KEY (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8mb4 COMMENT='app用户表'

-- 插入100万数据b (函数)

DELIMITER $$ -- 写函数之前必须要写，标志
CREATE FUNCTION mock_data()
RETURNS INT
BEGIN
  DECLARE num INT DEFAULT 1000000;
  DECLARE i INT DEFAULT 0;
  WHILE i<num DO
    INSERT INTO app_user(`name`,`email`,`phone`,`gender`,`password`,`age`)
    VALUES(CONCAT('用户',i),'123345@qq.com',CONCAT('18',FLOOR(RAND()*((999999999-100000000)+100000000))),FLOOR(RAND()*2),UUID(),FLOOR(RAND()*100));
    SET i = i+1;
  END WHILE;
  RETURN i;
END;

-- 执行函数
SELECT mock_data();

SELECT * FROM app_user WHERE `name` = '用户9999'; --  0.835 sec
SELECT * FROM app_user WHERE `name` = '用户100099'; --  0.929 sec
EXPLAIN SELECT * FROM app_user WHERE `name` = '用户100099' -- rows = 92312
-- id_表名_字段名
-- CREATE INDEX 索引名 ON 表名(`字段名`);
CREATE INDEX id_app_user_name ON `app_user`(`name`)

SELECT * FROM app_user WHERE `name` = '用户100099'; -- 执行耗时   : 0.025 sec
SELECT * FROM app_user WHERE `name` = '用户100099'; -- rows = 1
```

>  索引在大数据的时候，区别特别明显

## 7.3 索引原则

* 不是越多越好
* 不要对经常变动的数据加索引
* 小数据量不要加索引
* 索引一般要加在常用的字段上

>索引的数据结构

[CodingLabs - MySQL索引背后的数据结构及算法原理](http://blog.codinglabs.org/articles/theory-of-mysql-index.html)

Hash 类型

Btree：INNOBD的默认数据结构

# 8 权限管理

## 8.1 用户管理

> SQLyog 可视化管理

> SQL命令操作

```sql
-- ============ 创建用户 ================
CREATE USER cw IDENTIFIED BY '123456'

-- 修改密码 当前用户
SET PASSWORD = PASSWORD('123456')

-- 修改密码 指定用户
SET PASSWORD FOR cw = PASSWORD('123456')

-- 用户重命名
RENAME USER cw TO thecw

-- 授权 ALL PRIVILEGES on 库.表 to 用户名
-- ALL PRIVILEGES除了给别人授权都可以
GRANT ALL PRIVILEGES ON *.* TO thecw

-- 查看权限
SHOW GRANTS FOR thecw -- 查看指定用户的权限
SHOW GRANTS FOR root@localhost -- 查看root用户权限

-- 撤销权限 REVOKE 权限 on 库.表 from 谁
REVOKE ALL PRIVILEGES ON *.* FROM thecw

DROP USER thecw
```

## 8.2 数据库备份

保证重要数据不丢失，数据转移

备份的方式：

* 直接拷贝物理文件data文件
* 在sqlyog可视化工具种手动导出
* 命令行 mysqldump  （cmd命令行使用）

```bash
# 一张表 mysqldump -h主机 -u用户名 -p密码 数据库 表名 >物理磁盘位置/文件名
mysqldump -hlocalhost -uroot -p123456 school student >D:/a.sql

# 多张表 mysqldump -h主机 -u用户名 -p密码 数据库 表名1 表名2 >物理磁盘位置/文件名
mysqldump -hlocalhost -uroot -p123456 school student result >D:/a.sql

# 数据库 mysqldump -h主机 -u用户名 -p密码 数据库 >物理磁盘位置/文件名
mysqldump -hlocalhost -uroot -p123456 school >D:/a.sql

# 导入
# 登录的情况下，切换到指定的数据库
# source 备份文件
# 也可以这样
mysql -u用户名 -p密码 库名<备份文件
```

# 9 规范数据库设计

## 9.1 为什么需要设计

**当数据库比较复杂的时候，我们就需要设计了**

**糟糕的数据库设计**

- 数据冗余，浪费空间
- 数据库插入和删除都会麻烦、异常（屏蔽使用物理外键）
- 程序的性能差

**良好的数据库设计**

- 节省内存空间
- 保证数据库的完整性
- 方便我们开发系统

**软件开发中，关于数据库的设计**

- 分析需求，分析业务和需要处理的数据库的需求
- 概要设计：设计关系图E-R图

**设计数据库的步骤（个人博客）**

- 收集信息，分析需求

  - 用户表（用户登录注销，用户的个人信息，写博客，创建分类）

    ![image-20210704162821445](https://gitee.com/zhayuyao/img/raw/master/zhayuyao/img/image-20210704162821445.png)

  - 分类表（文章分类，谁创建的）

    ![image-20210704163120753](https://gitee.com/zhayuyao/img/raw/master/zhayuyao/img/image-20210704163120753.png)

  - 文章表（文章信息）

    ![image-20210704164131400](https://gitee.com/zhayuyao/img/raw/master/zhayuyao/img/image-20210704164131400.png)

  - 评论表

    ![image-20210704164555263](https://gitee.com/zhayuyao/img/raw/master/zhayuyao/img/image-20210704164555263.png)

  - 友链表（友情链接信息）

    ![image-20210704164938625](https://gitee.com/zhayuyao/img/raw/master/zhayuyao/img/image-20210704164938625.png)

  - 自定义表（系统信息，某个关键的字，或者一些主字段） `key:value`

  - 关注表(粉丝数)

    ![image-20210704165556127](https://gitee.com/zhayuyao/img/raw/master/zhayuyao/img/image-20210704165556127.png)

  - 说说表（发表心情， id...content...create_time）

- 标识实体（把需求落到每个字段）

- 标识实体之间的关系

  - 写博客：user --> blog
  - 创建分类：user --> category
  - 关注：user --> user
  - 友链：links
  - 评论：user --> user --> blog

(bbs / crm)

## 9.2 三大范式

**为什么需要数据规范化？**

- 信息重复
- 更新异常
- 插入异常
  - 无法正常显示信息
- 删除异常
  - 丢失有效的信息

> 三大范式

**第一范式（1NF）**

原子性：保证每一列不可再分

**第二范式（2NF）**

前提：满足第一范式

第二范式需要确保数据库表中的每一列都和主键相关，而不能只与主键的某一部分相关（主要针对联合主键而言）。

每张表只描述一件事情

**第三范式（3NF）**

前提：满足第一范式和第二范式

第三范式需要确保数据表中的每一列数据都和主键直接相关，而不能间接相关。

规范数据库的设计

>  **规范性和性能的问题**

关联查询的表不得超过三张表

- 考虑商业化的需求和目标（成本，用户体验）数据库的性能更加重要
- 在考虑性能的问题的时候，需要适当的考虑一下规范性
- 故意给某些表增加一些冗余的字段。（从多表查询中变为单表查询）
- 故意增加一些计算列（从大数据库降低为小数据量的查询：索引）

# 10 JDBC（重点）

## 10.1 数据库驱动

驱动：声卡，显卡，数据库

![image-20210713212553844](https://gitee.com/zhayuyao/img/raw/master/zhayuyao/img/image-20210713212553844.png)

我们的程序会通过数据库驱动，和数据库打交道！

## 10.2 JDBC

SUN公司为了简化开发人员的（对数据库的统一）操作，提供了一个（java操作数据库的）规范，俗称JDBC

这些规范的实现由具体的厂商去做~

对于开发人员来说，我们只需要掌握JDBC接口的操作即可！

![image-20210713213250382](https://gitee.com/zhayuyao/img/raw/master/zhayuyao/img/image-20210713213250382.png)

java.sql

javax.sql

还需要导入一个数据库驱动包 mysql-connector-java-5.1.47.jar

## 10.3 第一个JDBC的程序

> 创建测试数据库

```sql
CREATE DATABASE jdbcstudy CHARACTER SET utf8 COLLATE utf8_general_ci;

USER jdbcstudy;

CREATE TABLE users(
  `id` INT PRIMARY KEY,
  `name` VARCHAR(40),
  `password` VARCHAR(40),
  `email` VARCHAR(60),
  `birthday` DATE
);

INSERT INTO users(`id`,`name`,`password`,`email`,`birthday`)
VALUES(1,'张三','123456','zs@sina.com','1980-12-04'),
(2,'李四','123456','lisi@sina.com','1981-12-04'),
(3,'王五','123456','wangwu@sina.com','1982-12-04');
```

1. 创建一个普通项目
2. 导入数据库驱动（jar包）

![image-20210713215804024](https://gitee.com/zhayuyao/img/raw/master/zhayuyao/img/image-20210713215804024.png)

3. 编写测试代码

```
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

/**
 * @ClassName: JDBCDemo01
 * @Description: 我的第一个JDBC程序
 * @Author: zyy
 * @Date: 2021/07/13 21:59
 * @Version: 1.0
 */
public class JDBCDemo01 {
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        //1.加载驱动
//        DriverManager.registerDriver(new com.mysql.jdbc.Driver());
        //推荐这种写法加载驱动
        Class.forName("com.mysql.jdbc.Driver");
        //2.用户信息和URL
        // useSSL=true可能会报错
        String url = "jdbc:mysql://localhost:3306/jdbcstudy?useUnicode=true&characterEncoding=utf8&useSSL=false";
        String userName = "root";
        String passWord = "123456";
        //3.连接成功，数据库对象 Connection代表数据库
        Connection connection = DriverManager.getConnection(url, userName, passWord);
        //4.执行SQl的对象 Statement 执行的sql对象
        Statement statement = connection.createStatement();
        //5.执行SQL的对象 去 执行SQL ，可能存在结果，查看返回的结果
        String sql = "SELECT * FROM users";
        //返回的结果集 结果集中封装了我们全部的查询的结果
        ResultSet resultSet = statement.executeQuery(sql);
        while (resultSet.next()) {
            System.out.println("id="+resultSet.getObject("id"));
            System.out.println("name="+resultSet.getObject("name"));
            System.out.println("password="+resultSet.getObject("password"));
            System.out.println("email="+resultSet.getObject("email"));
            System.out.println("birthday="+resultSet.getObject("birthday"));
            System.out.println("===============================");
        }
        //6.释放连接
        resultSet.close();
        statement.close();
        connection.close();
    }
}
```

步骤总结：

1. 加载驱动
2. 连接数据库DriverManager
3. 获取执行SQL的对象 Statement
4. 获得返回的结果集
5. 释放连接

## 10.4 JDBC中对象解释

> DriverManager

```java
//1.加载驱动
//DriverManager.registerDriver(new com.mysql.jdbc.Driver());
//推荐这种写法加载驱动
Class.forName("com.mysql.jdbc.Driver");

Connection connection = DriverManager.getConnection(url, userName, passWord);
// connection代表数据库
// 数据库设置自动提交
// 事务提交
// 事务回滚
connection.setAutoCommit(true);
connection.commit();
connection.rollback();
```

> URL

```java
String url = "jdbc:mysql://localhost:3306/jdbcstudy?useUnicode=true&characterEncoding=utf8&useSSL=false";

// mysql默认端口3306
// 协议://主机地址:端口号/数据库名?参数1&参数2&参数3
// oracle默认端口1521
// jdbc:oracle:thin:@localhost:1521:sid
```

> Statement 执行sql对象 、 PreparedStatement 执行sql对象

```java
String sql = "SELECT * FROM users";//编写SQL

statement.executeQuery(sql);//执行查询 返回ResultSet
statement.executeUpdate(sql);//新增，删除，修改，都用这个，返回受影响的行数
statement.execute(sql);//执行任何SQL
```

> ResultSet 查询的结果集，封装了所有的查询结果

获得指定的数据类型

```java
//在不知道列类型的情况下使用
resultSet.getObject();
//如果知道列类型，就使用指定的类型
resultSet.getString();
resultSet.getInt();
resultSet.getDouble();
resultSet.getBigDecimal();
resultSet.getFloat();
resultSet.getDate();
//...
```

遍历，指针

```java
resultSet.beforeFirst();//移动到最前面
resultSet.afterLast();//移动到最后面
resultSet.next();//移动到下一个数据
resultSet.previous();//移动到前一行
resultSet.absolute(row);//移动到指定行
```

> 释放资源

```java
resultSet.close();
statement.close();
connection.close();//消耗资源
```

## 10.5 statement对象详解

**jdbc中的statement对象用于向数据库发送SQL语句，想完成对数据库的增删改查，只需要通过这个对象向数据库发送增删改查语句即可。**

Statement对象的executeUpdate方法，用于向数据库发送增、删、改的SQL语句，executeUpdate执行完后，将会返回一个整数（即增删改语句导致了数据库几行数据发送了变化）。

Statement.executeQuery方法用于向数据库发送查询语句，executeQuery方法返回代表查询结果的ResultSet对象。

> CRUD操作-create

使用executeUpdate(String sql)方法完成数据添加操作，示例操作：

```java
Statement statement = connection.createStatement();
String sql = "insert into user(...) values(...)";
int num = statement.executeUpdate(sql);
if (num > 0) {
    System.out.println("插入成功~");
}
```

> CRUD操作-delete

```java
Statement statement = connection.createStatement();
String sql = "delete from user where id=1";
int num = statement.executeUpdate(sql);
if (num > 0) {
    System.out.println("删除成功~");
}
```

> CRUD操作-update

```java
Statement statement = connection.createStatement();
String sql = "update user set name='' where name =''";
int num = statement.executeUpdate(sql);
if (num > 0) {
    System.out.println("修改成功~");
}
```

> CRUD操作-read

```java
Statement statement = connection.createStatement();
String sql = "SELECT * FROM users";
ResultSet resultSet = statement.executeQuery(sql);
while (resultSet.next()) {
    //根据获取列的数据类型，分别调用resultSet的相应方法映射到java对象中
}
```

> 代码实现

1. 提取工具类

   ```java
   import java.io.IOException;
   import java.io.InputStream;
   import java.sql.Connection;
   import java.sql.DriverManager;
   import java.sql.ResultSet;
   import java.sql.SQLException;
   import java.sql.Statement;
   import java.util.Properties;
   
   /**
    * @ClassName: JDBCUtils
    * @Description: TODO 类描述
    * @Author: zyy
    * @Date: 2021/07/14 17:48
    * @Version: 1.0
    */
   public class JDBCUtils {
       private static String driver = null;
       private static String url = null;
       private static String username = null;
       private static String password = null;
   
       static {
           try {
               InputStream in = JDBCUtils.class.getClassLoader().getResourceAsStream("db.properties");
               Properties properties = new Properties();
               properties.load(in);
   
               driver = properties.getProperty("driver");
               url = properties.getProperty("url");
               username = properties.getProperty("username");
               password = properties.getProperty("password");
   
               //驱动只用加载一次
               Class.forName(driver);
           } catch (IOException e) {
               e.printStackTrace();
           } catch (ClassNotFoundException e) {
               e.printStackTrace();
           }
       }
   
       /**
        * 获取连接
        */
       public static Connection getConnection() throws SQLException {
           return DriverManager.getConnection(url, username, password);
       }
   
       /**
        * 释放资源
        */
       public static void release(Connection con, Statement st, ResultSet rs) {
           if (rs != null) {
               try {
                   rs.close();
               } catch (SQLException e) {
                   e.printStackTrace();
               }
           }
           if (st != null) {
               try {
                   st.close();
               } catch (SQLException e) {
                   e.printStackTrace();
               }
           }
           if (con != null) {
               try {
                   con.close();
               } catch (SQLException e) {
                   e.printStackTrace();
               }
           }
       }
   }
   ```

   配置文件db.properties

   ![image-20210714221731402](https://gitee.com/zhayuyao/img/raw/master/zhayuyao/img/image-20210714221731402.png)

   ```properties
   driver=com.mysql.jdbc.Driver
   url=jdbc:mysql://localhost:3306/jdbcstudy?useUnicode=true&characterEncoding=utf8&useSSL=false
   username=root
   password=123456
   ```

2. 编写增删改的方法，`executeUpdate`

   ```java
   import com.zyy.lesson02.utils.JDBCUtils;
   
   import java.sql.Connection;
   import java.sql.ResultSet;
   import java.sql.SQLException;
   import java.sql.Statement;
   
   /**
    * @ClassName: TestInsert
    * @Description: TODO 类描述
    * @Author: zyy
    * @Date: 2021/07/14 18:19
    * @Version: 1.0
    */
   public class TestInsert {
       public static void main(String[] args) {
           Connection con = null;
           Statement st = null;
           ResultSet rs = null;
           try {
               con = JDBCUtils.getConnection();
               st = con.createStatement();
               String sql = "INSERT INTO users(`id`,`name`,`password`,`email`,`birthday`)\n" +
                       "VALUES (5,'钱七','123456','qianqi@sina.com','1988-12-04')";
               int num = st.executeUpdate(sql);
               if (num > 0) {
                   System.out.println("插入成功！");
               }
   
           } catch (SQLException e) {
               e.printStackTrace();
           } finally {
               JDBCUtils.release(con, st, rs);
           }
   
       }
   }
   ```

   ```java
   import com.zyy.lesson02.utils.JDBCUtils;
   
   import java.sql.Connection;
   import java.sql.ResultSet;
   import java.sql.SQLException;
   import java.sql.Statement;
   
   /**
    * @ClassName: TestDelete
    * @Description: TODO 类描述
    * @Author: zyy
    * @Date: 2021/07/14 22:11
    * @Version: 1.0
    */
   public class TestDelete {
       public static void main(String[] args) {
           Connection con = null;
           Statement st = null;
           ResultSet rs = null;
           try {
               con = JDBCUtils.getConnection();
               st = con.createStatement();
               String sql = "DELETE FROM users WHERE `id`=5";
               int num = st.executeUpdate(sql);
               if (num > 0) {
                   System.out.println("删除成功！");
               }
   
           } catch (SQLException e) {
               e.printStackTrace();
           } finally {
               JDBCUtils.release(con, st, rs);
           }
   
       }
   }
   ```

   ```java
   import com.zyy.lesson02.utils.JDBCUtils;
   
   import java.sql.Connection;
   import java.sql.ResultSet;
   import java.sql.SQLException;
   import java.sql.Statement;
   
   /**
    * @ClassName: TestUpdate
    * @Description: TODO 类描述
    * @Author: zyy
    * @Date: 2021/07/14 22:11
    * @Version: 1.0
    */
   public class TestUpdate {
       public static void main(String[] args) {
           Connection con = null;
           Statement st = null;
           ResultSet rs = null;
           try {
               con = JDBCUtils.getConnection();
               st = con.createStatement();
               String sql = "UPDATE users SET birthday='1990-12-01' WHERE id=1";
               int num = st.executeUpdate(sql);
               if (num > 0) {
                   System.out.println("更新成功！");
               }
   
           } catch (SQLException e) {
               e.printStackTrace();
           } finally {
               JDBCUtils.release(con, st, rs);
           }
   
       }
   }
   ```

3. 查询

   ```java
   import com.zyy.lesson02.utils.JDBCUtils;
   
   import java.sql.Connection;
   import java.sql.ResultSet;
   import java.sql.SQLException;
   import java.sql.Statement;
   
   /**
    * @ClassName: TestSelect
    * @Description: TODO 类描述
    * @Author: zyy
    * @Date: 2021/07/14 22:11
    * @Version: 1.0
    */
   public class TestSelect {
       public static void main(String[] args) {
           Connection con = null;
           Statement st = null;
           ResultSet rs = null;
           try {
               con = JDBCUtils.getConnection();
               st = con.createStatement();
               String sql = "SELECT * FROM users WHERE id=1";
               rs = st.executeQuery(sql);
               while (rs.next()) {
                   System.out.println("id="+rs.getInt("id"));
                   System.out.println("name="+rs.getString("name"));
               }
   
           } catch (SQLException e) {
               e.printStackTrace();
           } finally {
               JDBCUtils.release(con, st, rs);
           }
   
       }
   }
   ```

## 10.6 SQL注入的问题

sql存在漏洞，会被攻击导致数据泄露 **SQL会被拼接**

SQL注入即是指[web应用程序](https://baike.baidu.com/item/web应用程序/2498090)对用户输入数据的合法性没有判断或过滤不严，攻击者可以在web应用程序中事先定义好的查询语句的结尾上添加额外的[SQL语句](https://baike.baidu.com/item/SQL语句/5714895)，在管理员不知情的情况下实现非法操作，以此来实现欺骗[数据库服务器](https://baike.baidu.com/item/数据库服务器/613818)执行非授权的任意查询，从而进一步得到相应的数据信息。

```java
import com.zyy.lesson02.utils.JDBCUtils;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class SQLQuestion {
    public static void main(String[] args) {

        //正常登录
//        login("张三","1234567");

        //sql注入
        login("' or '1=1","123456");

    }

    /**
     * 登录业务
     */
    public static void login(String userName, String password) {
        Connection con = null;
        Statement st = null;
        ResultSet rs = null;
        try {
            con = JDBCUtils.getConnection();
            st = con.createStatement();
            String sql = "SELECT * FROM users WHERE `name`='"+userName+"' AND `password`='"+password+"'";
            // SELECT * FROM users WHERE `name`='' or '1=1' AND `password`='123456'
            System.out.println(sql);
            rs = st.executeQuery(sql);
            while (rs.next()) {
                System.out.println("id="+rs.getInt("id"));
                System.out.println("name="+rs.getString("name"));
            }

        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            JDBCUtils.release(con, st, rs);
        }
    }
}
```

导致结果：错误的用户名或者密码可以获取到全部的用户信息

## 10.7 PreparedStatement对象

防止SQL注入和效率更高

1. 新增

   ```java
   import com.zyy.lesson02.utils.JDBCUtils;
   
   import java.sql.Connection;
   import java.sql.PreparedStatement;
   import java.sql.ResultSet;
   import java.sql.SQLException;
   
   public class TestInsert {
       public static void main(String[] args) {
           Connection con = null;
           PreparedStatement st = null;
           ResultSet rs = null;
           try {
               con = JDBCUtils.getConnection();
               //使用?占位符代替参数
               String sql = "INSERT INTO 			  users(`id`,`name`,`password`,`email`,`birthday`) VALUES 	(?,?,?,?,?)";
               //预编译SQL，先写SQL，然后不执行
               st = con.prepareStatement(sql);
               //手动给参数赋值 下标 值，下标从1开始
               st.setInt(1, 5);
               st.setString(2, "钱七");
               st.setString(3, "123456");
               st.setString(4, "qianqi@sina.com");
               // new java.util.Date().getTime() 获得时间戳
               // new java.sql.Date() 转化为sql时间
               st.setDate(5, new java.sql.Date(new java.util.Date().getTime()));
               int num = st.executeUpdate();
               if (num > 0) {
                   System.out.println("插入成功！");
               }
           } catch (SQLException e) {
               e.printStackTrace();
           } finally {
               JDBCUtils.release(con, st, rs);
           }
   
       }
   }
   ```

2. 删除

   ```java
   import com.zyy.lesson02.utils.JDBCUtils;
   
   import java.sql.Connection;
   import java.sql.PreparedStatement;
   import java.sql.ResultSet;
   import java.sql.SQLException;
   
   /**
    * @ClassName: TestDelete
    * @Description: TODO 类描述
    * @Author: zyy
    * @Date: 2021/07/14 18:19
    * @Version: 1.0
    */
   public class TestDelete {
       public static void main(String[] args) {
           Connection con = null;
           PreparedStatement st = null;
           ResultSet rs = null;
           try {
               con = JDBCUtils.getConnection();
               //使用?占位符代替参数
               String sql = "DELETE FROM users WHERE `id`=?";
               //预编译SQL，先写SQL，然后不执行
               st = con.prepareStatement(sql);
               //手动给参数赋值
               st.setInt(1, 5);
               int num = st.executeUpdate();
               if (num > 0) {
                   System.out.println("删除成功！");
               }
   
           } catch (SQLException e) {
               e.printStackTrace();
           } finally {
               JDBCUtils.release(con, st, rs);
           }
   
       }
   }
   ```

3. 更新

   ```java
   import com.zyy.lesson02.utils.JDBCUtils;
   
   import java.sql.Connection;
   import java.sql.PreparedStatement;
   import java.sql.ResultSet;
   import java.sql.SQLException;
   
   public class TestUpdate {
       public static void main(String[] args) {
           Connection con = null;
           PreparedStatement st = null;
           ResultSet rs = null;
           try {
               con = JDBCUtils.getConnection();
               //使用?占位符代替参数
               String sql = "UPDATE users SET birthday=? WHERE id=?";
               //预编译SQL，先写SQL，然后不执行
               st = con.prepareStatement(sql);
               //手动给参数赋值
               st.setDate(1, new java.sql.Date(new java.util.Date().getTime()));
               st.setInt(2, 1);
               int num = st.executeUpdate();
               if (num > 0) {
                   System.out.println("修改成功！");
               }
   
           } catch (SQLException e) {
               e.printStackTrace();
           } finally {
               JDBCUtils.release(con, st, rs);
           }
       }
   }
   ```

4. 查询

   ```java
   import com.zyy.lesson02.utils.JDBCUtils;
   
   import java.sql.Connection;
   import java.sql.PreparedStatement;
   import java.sql.ResultSet;
   import java.sql.SQLException;
   
   public class TestSelect {
       public static void main(String[] args) {
           Connection con = null;
           PreparedStatement st = null;
           ResultSet rs = null;
           try {
               con = JDBCUtils.getConnection();
               //使用?占位符代替参数
               String sql = "SELECT * FROM users WHERE id=?";
               //预编译SQL，先写SQL，然后不执行
               st = con.prepareStatement(sql);
               //手动给参数赋值
               st.setInt(1, 1);
               rs = st.executeQuery();
               while (rs.next()) {
                   System.out.println(rs.getInt("id"));
             		System.out.println(rs.getString("name"));
               }
           } catch (SQLException e) {
               e.printStackTrace();
           } finally {
               JDBCUtils.release(con, st, rs);
           }
   
       }
   }
   ```

5. 防止sql注入

   ```java
   import com.zyy.lesson02.utils.JDBCUtils;
   
   import java.sql.Connection;
   import java.sql.PreparedStatement;
   import java.sql.ResultSet;
   import java.sql.SQLException;
   
   public class SQLQuestion {
       public static void main(String[] args) {
           //正常登录
   		//login("张三","123456");
   
           //sql注入
           login("' or '1=1", "123456");
       }
   
       /**
        * 登录业务
        */
       public static void login(String userName, String password) {
           Connection con = null;
           PreparedStatement st = null;
           ResultSet rs = null;
           try {
               con = JDBCUtils.getConnection();
               // PreparedStatement 防止SQL注入的本质，把传递进来的参数当做字符
               // 假设其中存在转义字符，比如说'会被直接转义
               String sql = "SELECT * FROM users WHERE `name`=? AND `password`=?";
               st = con.prepareStatement(sql);
               st.setString(1, userName);
               st.setString(2, password);
               rs = st.executeQuery();
               while (rs.next()) {
                   System.out.println("id=" + rs.getInt("id"));
                   System.out.println("name=" + rs.getString("name"));
               }
   
           } catch (SQLException e) {
               e.printStackTrace();
           } finally {
               JDBCUtils.release(con, st, rs);
           }
       }
   }
   ```

   执行结果：查不到任何结果

## 10.8 使用IDEA连接数据库

![image-20210715092417582](https://gitee.com/zhayuyao/img/raw/master/zhayuyao/img/image-20210715092417582.png)

连接成功后，就可以选择数据库

![image-20210715095230497](https://gitee.com/zhayuyao/img/raw/master/zhayuyao/img/image-20210715095230497.png)

![image-20210715095449617](https://gitee.com/zhayuyao/img/raw/master/zhayuyao/img/image-20210715095449617.png)

连接不上的话，可以看一下下面这里，配置对应的mysql版本

![image-20210715214953909](https://gitee.com/zhayuyao/img/raw/master/zhayuyao/img/image-20210715214953909.png)

双击数据库

![image-20210715095953750](https://gitee.com/zhayuyao/img/raw/master/zhayuyao/img/image-20210715095953750.png)

更新数据（提交）

![image-20210715100117897](https://gitee.com/zhayuyao/img/raw/master/zhayuyao/img/image-20210715100117897.png)

![image-20210715214142285](https://gitee.com/zhayuyao/img/raw/master/zhayuyao/img/image-20210715214142285.png)

idea编写sql

```sql
create table account
(
    id    int primary key auto_increment,
    name  varchar(40),
    money float
);

insert into account(name, money)
values ('A', 1000),
       ('B', 1000),
       ('C', 1000);
```

![image-20210715214803462](https://gitee.com/zhayuyao/img/raw/master/zhayuyao/img/image-20210715214803462.png)

## 10.9 事务

==要么都成功，要么都失败==

> ACID原则

原子性：要么全部成功，要么全部失败

一致性：总数不变

隔离性：多个进程互不干扰

持久性：一旦提交不可逆，持久化到数据库了

隔离性的问题：

脏读：一个事务读取了另外一个没有提交的事务

不可重复读：在同一个事务内，重复读取表中数据，表数据发生了改变

幻读：在一个事务内，读取到了别人插入的数据，导致前后读出来的结果不一致

> 代码实现

1. 开启事务`con.setAutoCommit(false);`
2. 一组业务执行完毕，提交事务
3. 可以在catch语句中显示的定义回滚语句，但是默认失败就会回滚

正常情况

```java
import com.zyy.lesson02.utils.JDBCUtils;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

/**
 * @ClassName: TestTransaction1
 * @Description: TODO 类描述
 * @Author: zyy
 * @Date: 2021/07/15 21:59
 * @Version: 1.0
 */
public class TestTransaction1 {
    public static void main(String[] args) {
        Connection con = null;
        PreparedStatement ps = null;
        ResultSet rs = null;

        try {
            con = JDBCUtils.getConnection();
            //关闭自动提交 自动会开启事务
            con.setAutoCommit(false);//开启事务
            // A 转 B 100元
            String sql1 = "update account set money=money-100 where name='A'";
            ps = con.prepareStatement(sql1);
            ps.executeUpdate();
            String sql2 = "update account set money=money+100 where name='B'";
            ps = con.prepareStatement(sql2);
            ps.executeUpdate();
            //业务完毕，提交事务
            con.commit();

            System.out.println("A 转 B 100元 成功！");
        } catch (SQLException e) {
            e.printStackTrace();
            try {
                con.rollback();
            } catch (SQLException ex) {
                ex.printStackTrace();
            }
        } finally {
            JDBCUtils.release(con, ps, rs);
        }
    }
}
```

异常情况

```java
import com.zyy.lesson02.utils.JDBCUtils;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

/**
 * @ClassName: TestTransaction1
 * @Description: TODO 类描述
 * @Author: zyy
 * @Date: 2021/07/15 21:59
 * @Version: 1.0
 */
public class TestTransaction2 {
    public static void main(String[] args) {
        Connection con = null;
        PreparedStatement ps = null;
        ResultSet rs = null;

        try {
            con = JDBCUtils.getConnection();
            //关闭自动提交 自动会开启事务
            con.setAutoCommit(false);//开启事务
            // A 转 B 100元
            String sql1 = "update account set money=money-100 where name='A'";
            ps = con.prepareStatement(sql1);
            ps.executeUpdate();

            //默认失败
            int x = 1/0; //一定会异常

            String sql2 = "update account set money=money+100 where name='B'";
            ps = con.prepareStatement(sql2);
            ps.executeUpdate();
            //业务完毕，提交事务
            con.commit();

            System.out.println("A 转 B 100元 成功！");
        } catch (SQLException e) {
            e.printStackTrace();
            //如果异常，默认也会回滚，下面不写也可以
//            try {
//                con.rollback();
//            } catch (SQLException ex) {
//                ex.printStackTrace();
//            }
        } finally {
            JDBCUtils.release(con, ps, rs);
        }
    }
}
```

## 10.10 数据库连接池

数据库连接 -- 执行完毕 -- 释放

连接-- 释放 是十分浪费系统资源的

**池化技术：准备一些预先的资源，过来就连接预先准备好的**

最小连接数：10  (常用连接)

最大连接数：100 （业务最高承载上线）

等待超时：100ms

编写连接池，实现一个接口DataSource

> 开源数据源实现

DBCP

C3P0

Druid: 阿里巴巴

使用了这些数据库连接池之后，我们在项目开发中就不需要编写连接数据库的代码了

> DBCP

需要用到的jar包

commons-dbcp-1.4

commons-pool-1.6

配置文件dbcp.properties

```properties
#连接设置
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/jdbcstudy?useUnicode=true&characterEncoding=utf8&useSSL=false
username=root
password=123456

#初始化连接
initialSize=10

#最大连接数量
maxActive=50

#最大空闲连接
maxIdle=20

#最小空闲连接
minIdle=5

#超时等待时间以毫秒为单位 6000毫秒/1000等于60秒
maxWait=60000
#JDBC驱动建立连接时附带的连接属性属性的格式必须为这样：【属性名=property;】
#注意：user 与 password 两个属性会被明确地传递，因此这里不需要包含他们。
connectionProperties=useUnicode=true;characterEncoding=UTF8

#指定由连接池所创建的连接的自动提交（auto-commit）状态。
defaultAutoCommit=true

#driver default 指定由连接池所创建的连接的只读（read-only）状态。
#如果没有设置该值，则“setReadOnly”方法将不被调用。（某些驱动并不支持只读模式，如：Informix）
defaultReadOnly=

#driver default 指定由连接池所创建的连接的事务级别（TransactionIsolation）。
#可用值为下列之一：（详情可见javadoc。）NONE,READ_UNCOMMITTED, READ_COMMITTED, REPEATABLE_READ, SERIALIZABLE
defaultTransactionIsolation=READ_COMMITTED
```

工具类

```java
import org.apache.commons.dbcp.BasicDataSourceFactory;

import javax.sql.DataSource;
import java.io.IOException;
import java.io.InputStream;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.Properties;

/**
 * @ClassName: JDBCDBCPUtils
 * @Description: TODO 类描述
 * @Author: zyy
 * @Date: 2021/07/14 17:48
 * @Version: 1.0
 */
public class JDBCDBCPUtils {
    private static DataSource dataSource = null;

    static {
        try {
            InputStream in = JDBCDBCPUtils.class.getClassLoader().getResourceAsStream("dbcp.properties");
            Properties properties = new Properties();
            properties.load(in);
            //创建数据源 工厂模式
            dataSource = BasicDataSourceFactory.createDataSource(properties);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    /**
     * 获取连接
     */
    public static Connection getConnection() throws SQLException {
        //从数据源中获取连接
        return dataSource.getConnection();
    }

    /**
     * 释放资源
     */
    public static void release(Connection con, Statement st, ResultSet rs) {
        if (rs != null) {
            try {
                rs.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (st != null) {
            try {
                st.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (con != null) {
            try {
                con.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```

测试类

```java
import com.zyy.lesson02.utils.JDBCUtils;
import com.zyy.lesson05.utils.JDBCDBCPUtils;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

/**
 * @ClassName: TestDBCP
 * @Description: TODO 类描述
 * @Author: zyy
 * @Date: 2021/07/15 22:41
 * @Version: 1.0
 */
public class TestDBCP {
    public static void main(String[] args) {
        Connection con = null;
        PreparedStatement st = null;
        ResultSet rs = null;
        try {
            con = JDBCDBCPUtils.getConnection();
            //使用?占位符代替参数
            String sql = "INSERT INTO users(`id`,`name`,`password`,`email`,`birthday`) VALUES (?,?,?,?,?)";
            //预编译SQL，先写SQL，然后不执行
            st = con.prepareStatement(sql);
            //手动给参数赋值
            st.setInt(1, 5);
            st.setString(2, "钱七");
            st.setString(3, "123456");
            st.setString(4, "qianqi@sina.com");
            st.setDate(5, new java.sql.Date(new java.util.Date().getTime()));
            int num = st.executeUpdate();
            if (num > 0) {
                System.out.println("插入成功！");
            }

        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            JDBCDBCPUtils.release(con, st, rs);
        }

    }
}
```



> C3P0

需要用到的jar包

c3p0-0.9.5.5.jar

mchange-commons-java-0.2.19.jar

配置文件c3p0-config.xml

```properties
<?xml version="1.0" encoding="UTF-8"?>
<c3p0-config>
    <!--
    c3p0的缺省（默认）配置
    如果在代码中ComboPooledDataSource ds=new ComboPooledDataSource();这样写就表示使用的是c3p0的缺省（默认）
    -->
    <default-config>
        <property name="driverClass">com.mysql.jdbc.Driver</property>
        <property name="jdbcUrl">jdbc:mysql://localhost:3306/jdbcstudy?useUnicode=true&amp;characterEncoding=utf8&amp;useSSL=false&amp;serverTimezone=UTC</property>
        <property name="user">root</property>
        <property name="password">123456</property>
        <property name="acquiredIncrement">5</property>
        <property name="initialPoolSize">10</property>
        <property name="minPoolSize">5</property>
        <property name="maxPoolSize">20</property>
    </default-config>


    <!--
    c3p0的命名配置
    如果在代码中ComboPooledDataSource ds=new ComboPooledDataSource("MySQL");这样写就表示使用的是name是MySQL
    -->
    <name-config name="MySQL">
        <property name="driverClass">com.mysql.jdbc.Driver</property>
        <property name="jdbcUrl">jdbc:mysql://localhost:3306/jdbcstudy?useUnicode=true&amp;characterEncoding=utf8&amp;useSSL=false&amp;serverTimezone=UTC</property>
        <property name="user">root</property>
        <property name="password">123456</property>
        <property name="acquiredIncrement">5</property>
        <property name="initialPoolSize">10</property>
        <property name="minPoolSize">5</property>
        <property name="maxPoolSize">20</property>
    </name-config>
</c3p0-config>
```

工具类

```java
import com.mchange.v2.c3p0.ComboPooledDataSource;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

/**
 * @ClassName: JDBCC3P0Utils
 * @Description: TODO 类描述
 * @Author: zyy
 * @Date: 2021/07/14 17:48
 * @Version: 1.0
 */
public class JDBCC3P0Utils {
    private static DataSource dataSource = null;
    //private static ComboPooledDataSource dataSource = null;
    static {
        try {
            //代码的方式配置 需要参数
//            dataSource = new ComboPooledDataSource();
//            dataSource.setDriverClass();
//            dataSource.setJdbcUrl();
//            dataSource.setUser();
//            dataSource.setPassword();
//            dataSource.setMaxPoolSize();
//            dataSource.setMinPoolSize();
            //配置文件写法
            dataSource = new ComboPooledDataSource("MySQL");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    /**
     * 获取连接
     */
    public static Connection getConnection() throws SQLException {
        //从数据源中获取连接
        return dataSource.getConnection();
    }

    /**
     * 释放资源
     */
    public static void release(Connection con, Statement st, ResultSet rs) {
        if (rs != null) {
            try {
                rs.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (st != null) {
            try {
                st.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (con != null) {
            try {
                con.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```

测试类

```java
import com.zyy.lesson05.utils.JDBCC3P0Utils;
import com.zyy.lesson05.utils.JDBCDBCPUtils;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;


public class TestC3P0 {
    public static void main(String[] args) {
        Connection con = null;
        PreparedStatement st = null;
        ResultSet rs = null;
        try {
            con = JDBCC3P0Utils.getConnection();
            //使用?占位符代替参数
            String sql = "INSERT INTO users(`id`,`name`,`password`,`email`,`birthday`) VALUES (?,?,?,?,?)";
            //预编译SQL，先写SQL，然后不执行
            st = con.prepareStatement(sql);
            //手动给参数赋值
            st.setInt(1, 6);
            st.setString(2, "刘八");
            st.setString(3, "123456");
            st.setString(4, "liuba@sina.com");
            st.setDate(5, new java.sql.Date(new java.util.Date().getTime()));
            int num = st.executeUpdate();
            if (num > 0) {
                System.out.println("插入成功！");
            }

        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            JDBCC3P0Utils.release(con, st, rs);
        }

    }
}
```

> 总结

无论用什么数据源，本质还是一样的，DataSource接口不会变，方法就不会变

[Welcome to The Apache Software Foundation!](https://www.apache.org/index.html#projects-list)

![image-20210719174041225](https://gitee.com/zhayuyao/img/raw/master/zhayuyao/img/image-20210719174041225.png)
