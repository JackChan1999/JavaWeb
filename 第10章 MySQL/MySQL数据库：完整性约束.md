# 1. 完整性约束
完整性约束是为了表的数据的正确性！如果数据不正确，那么一开始就不能添加到表中。

# 2. 主键

当某一列添加了主键约束后，那么这一列的数据就不能重复出现。这样每行记录中其主键列的值就是这一行的唯一标识。例如学生的学号可以用来做唯一标识，而学生的姓名是不能做唯一标识的，因为姓名有可能同名。

主键列的值不能为NULL，也不能重复！指定主键约束使用PRIMARY KEY关键字

- 创建表：定义列时指定主键：

```mysql
CREATE TABLE stu(
		sid	    CHAR(6) PRIMARY KEY,
		sname	VARCHAR(20),
		age		INT,
		gender	VARCHAR(10)
);
```

- 创建表：定义列之后独立指定主键：

```mysql
CREATE TABLE stu(
		sid	    CHAR(6),
		sname	VARCHAR(20),
		age		INT,
		gender	VARCHAR(10),
		PRIMARY KEY(sid)
);
```

- 修改表时指定主键：

```mysql
ALTER TABLE stu
ADD PRIMARY KEY(sid);
```

- 删除主键（只是删除主键约束，而不会删除主键列）：

```mysql
ALTER TABLE stu DROP PRIMARY KEY;
```

# 3. 主键自增长
MySQL提供了主键自动增长的功能！这样用户就不用再为是否有主键是否重复而烦恼了。当主键设置为自动增长后，在没有给出主键值时，主键的值会自动生成，而且是最大主键值+1，也就不会出现重复主键的可能了。

- 创建表时设置主键自增长（主键必须是整型才可以自增长）：

```mysql
CREATE TABLE stu(
		sid INT PRIMARY KEY AUTO_INCREMENT,
		sname	VARCHAR(20),
		age		INT,
		gender	VARCHAR(10)
);
```

- 修改表时设置主键自增长：

```mysql
ALTER TABLE stu CHANGE sid sid INT AUTO_INCREMENT;
```

- 修改表时删除主键自增长：

```mysql
ALTER TABLE stu CHANGE sid sid INT;
```

# 4. 非空
指定非空约束的列不能没有值，也就是说在插入记录时，对添加了非空约束的列一定要给值；在修改记录时，不能把非空列的值设置为NULL。

- 指定非空约束：

```mysql
CREATE TABLE stu(
		sid INT PRIMARY KEY AUTO_INCREMENT,
		sname VARCHAR(10) NOT NULL,
		age		INT,
		gender	VARCHAR(10)
);
```

当为sname字段指定为非空后，在向stu表中插入记录时，必须给sname字段指定值，否则会报错：

```mysql
INSERT INTO stu(sid) VALUES(1);
```
插入的记录中sname没有指定值，所以会报错！

# 5. 唯一
还可以为字段指定唯一约束！当为字段指定唯一约束后，那么字段的值必须是唯一的。这一点与主键相似！例如给stu表的sname字段指定唯一约束：

```mysql
CREATE TABLE tab_ab(
	sid INT PRIMARY KEY AUTO_INCREMENT,
	sname VARCHAR(10) UNIQUE
);

INSERT INTO sname(sid, sname) VALUES(1001, 'zs');
INSERT INTO sname(sid, sname) VALUES(1002, 'zs');
```
当两次插入相同的名字时，MySQL会报错！

# 6. 概念模型
对象模型：在java中是domain，例如：User、Student，可以双向关联，而且引用的是对象，而不是一个主键

关系模型：在数据库中的表，只能多方引用一方，而且引用的只是主键，而不是一整行记录。

当我们要完成一个软件系统时，需要把系统中的实体抽取出来，形成概念模型。例如部门、员工都是系统中的实体。概念模型中的实体最终会成为Java中的类、数据库中的表。

实体之间还存在着关系，关系有三种：

* 1对多：例如每个员工都从属一个部门，而一个部门可以有多个员工，其中员工是多方，而部门是一方。
* 1对1：例如老公和老婆就是一对一的关系，一个老公只能有一个老婆，而一个老婆只能有一个老公。
* 多对多：老师与学生的关系就是多对多，一个老师可以有多个学生，一个学生可以有多个老师。

概念模型在Java中成为实体类（javaBean），类就使用成员变量来完成关系，一般都是双向关联！多对一双向中关联，即员工关联部门，部门也关联员工

```java
class Employee {//多方关联一方
     ...
     private Department department;
  }
  class Department {//一方关联多方
     ...
     private List<Employee> employees;
  }

  class Husband {
     ...
     private Wife wife;
  }
  class Wife {
     ...
     private Husband
  }

  class Student {
     ...
     private List<Teacher> teachers
  }
  class Teacher {
     ...
     private List<Student> students;
  }
```

# 7. 外键
主外键是构成表与表关联的唯一途径！

- 外键必须是另一表的主键的值(外键要引用主键！)
- 外键可以重复
- 外键可以为空
- 一张表中可以有多个外键！

外键是另一张表的主键！例如员工表与部门表之间就存在关联关系，其中员工表中的部门编号字段就是外键，是相对部门表的外键。

我们再来看BBS系统中：用户表（t_user）、分类表（t_section）、帖子表（t_topic）三者之间的关系。

 ![mysql](http://img.blog.csdn.net/20161027155931290)

例如在t_section表中sid为1的记录说明有一个分类叫java，版主是t_user表中uid为1的用户，即zs！

例如在t_topic表中tid为2的记录是名字为“Java是咖啡”的帖子，它是java版块的帖子，它的作者是ww。

外键就是用来约束这一列的值必须是另一张表的主键值！！！

创建t_user表，指定uid为主键列：

```mysql
CREATE TABLE t_user(
	uid	INT PRIMARY KEY AUTO_INCREMENT,
	uname	VARCHAR(20) UNIQUE NOT NULL
);
```
创建t_section表，指定sid为主键列，u_id为相对t_user表的uid列的外键：

```mysql
CREATE TABLE t_section(
		sid	INT PRIMARY KEY AUTO_INCREMENT,
		sname	VARCHAR(30),
		u_id	INT,
		CONSTRAINT fk_t_user FOREIGN KEY(u_id) REFERENCES t_user(uid)
);
```
修改t_section表，指定u_id为相对t_user表的uid列的外键：
```mysql
ALTER TABLE t_section
ADD CONSTRAINT fk_t_user
FOREIGN KEY(u_id)
REFERENCES t_user(uid);
```
修改t_section表，删除u_id的外键约束：
```mysql
ALTER TABLE t_section
DROP FOREIGN KEY fk_t_user;
```

# 8. 表与表之间的关系
## 8.1 一对一

例如t_person表和t_card表，即人和身份证。这种情况需要找出主从关系，即谁是主表，谁是从表。人可以没有身份证，但身份证必须要有人才行，所以人是主表，而身份证是从表。设计从表可以有两种方案：

- 在t_card表中添加外键列（相对t_user表），并且给外键添加唯一约束；
- 给t_card表的主键添加外键约束（相对t_user表），即t_card表的主键也是外键。

在表中建立一对一关系比较特殊，需要让其中一张表的主键，即是主键又是外键。
```mysql
  create table husband(
    hid int PRIMARY KEY,
    ...
  );
  create table wife(
    wid int PRIMARY KEY,
    ...
    ADD CONSTRAINT fk_wife_wid FOREIGN KEY(wid) REFERENCES husband(hid)
  );
```
其中wife表的wid即是主键，又是相对husband表的外键！
husband.hid是主键，不能重复！
wife.wid是主键，不能重复，又是外键，必须来自husband.hid。
所以如果在wife表中有一条记录的wid为1，那么wife表中的其他记录的wid就不能再是1了，因为它是主键。
同时在husband.hid中必须存在1这个值，因为wid是外键。这就完成了一对一关系。

## 8.2 一对多（多对一）

最为常见的就是一对多！一对多和多对一，这是从哪个角度去看得出来的。t_user和t_section的关系，从t_user来看就是一对多，而从t_section的角度来看就是多对一！这种情况都是在多方创建外键！

数据库表中的多对一关系，只需要在多方使用一个独立的列来引用1方的主键即可

```mysql
 /*员工表*/
  create talbe emp (
    empno int primary key,/*员工编号*/
    ...
    deptno int/*所属部门的编号*/
  );

  /*部门表*/
  create table dept (
    deptno int  primary key,/*部门编号*/
    ...
  );
```

emp表中的deptno列的值表示当前员工所从属的部门编号。也就是说emp.deptno必须在dept表中是真实存在！但是我们必须要去对它进行约束，不然可能会出现员工所属的部门编号是不存在的。这种约束就是外键约束。

我们需要给emp.deptno添加外键约束，约束它的值必须在dept.deptno中存在。外键必须是另一个表的主键！

语法：CONSTRAINT 约束名称 FOREIGN KEY(外键列名) REFERENCES 关联表(关联表的主键)

```mysql
/*创建表时指定外键约束*/
  create talbe emp (
    empno int primary key,
    ...
    deptno int,
    CONSTRAINT fk_emp FOREIGN KEY(mgr) REFERENCES emp(empno)  
  );

  /*修改表时添加外键约束*/
  ALERT TABLE emp
  ADD CONSTRAINT fk_emp_deptno FOREIGN KEY(deptno) REFERENCES dept(deptno);

  /*修改表时删除外键约束*/
  ALTER TABLE emp
  DROP FOREIGN KEY fk_emp_deptno;/*约束名称*/
```

## 8.3 多对多

例如t_stu和t_teacher表，即一个学生可以有多个老师，而一个老师也可以有多个学生。这种情况通常需要创建中间表来处理多对多关系。例如再创建一张表t_stu_tea表，给出两个外键，一个相对t_stu表的外键，另一个相对t_teacher表的外键。

在表中建立多对多关系需要使用中间表，即需要三张表，在中间表中使用两个外键，分别引用其他两个表的主键。

```mysql
 create table student(
    sid int PRIMARY KEY,
    ...
  );
  create table teacher(
    tid int PRIMARY KEY,
    ...
  );

  create table stu_tea(
    sid int,
    tid int,
    ADD CONSTRAINT fk_stu_tea_sid FOREIGN KEY(sid) REFERENCES student(sid),
    ADD CONSTRAINT fk_stu_tea_tid FOREIGN KEY(tid) REFERENCES teacher(tid)
  );
```
这时在stu_tea这个中间表中的每条记录都是来说明student和teacher表的关系

例如在stu_tea表中的记录：sid为1001，tid为2001，这说明编号为1001的学生有一个编号为2001的老师
```
sid    tid
101    201 /*编号为101的学生有一个编号为201的老师*/
101    202 /*编号为101的学生有一个编号为202的老师*/
101    203 /*编号为101的学生有一个编号为203的老师*/
102    201 /*编号为102的学生有一个编号为201的老师*/
102    204 /*编号为102的学生有一个编号为204的老师*/
```
