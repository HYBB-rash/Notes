# 中级SQL

## 连接表达式

接下来讨论`join` 表达式。

### 连接条件

在T-SQL中，连接条件用`ON` ， 和`where` 类似。

T-SQL中`join` 默认为`inner join` 。

```mysql
SELECT	*
  FROM	student JOIN takes ON student.ID = takes.ID;

SELECT 	*
  FROM	student INNER JOIN takes ON student.ID = takes.ID;
```

上面的写法和下面这个写法结果相同

```mssql
SELECT	*
  FROM	student, takes
 WHERE	student.ID = takes.ID;
```

### 外连接

这个连接可以解决内不能查询出的结果。比如查询选课，如果某个课没有被选过或者某个学生没有选课，那它就不会被查询出来。外连接可以解决这个问题。

实际上有3种形式的外连接

1. 左外连接：只保留左边关系中的元组。
2. 右外连接：只保留右边关系中的元组。
3. 全外连接：两个关系都保留。

如果其中出现没有匹配的结果，则将被赋值称为`null` 。

用`T-SQL` 实现一个左外连接

```mssql
SELECT	student.ID
  FROM	student LEFT OUTER JOIN takes ON student.ID = takes.ID
 WHERE	takes.course_id IS NULL;
```

这能找到没选课学生的ID。

然后右外连接是基本一样的，就是对称的。

全外连接在`T-SQL` 的实现差不多。

```mssql
SELECT	*
  FROM	(	SELECT	*
			  FROM	student
			 WHERE	dept_name = 'Comp. Sci.') t1
		FULL OUTER JOIN
		(	SELECT	*
			  FROM	takes
			 WHERE	semester = 'Spring' AND [year] = 2009) t2
		ON t1.ID = t2.ID;
```

`on` 是一种声明，它可以为没有贡献的元组添加空值。而`where` 是一种

## 视图

视图是一张虚表（虚关系），它并没有实际存储在数据库中。它是需要时才进行查询。相当于是储存一段用于查询的SQL语句。

### 视图定义

主要格式是：

```mssql
create view v as <关系表达式>
```

嗯，用`T-SQL` 实现一下。

```mssql
CREATE VIEW faculty AS
	SELECT	ID, name, dept_name
	  FROM	instructor;
```

还有一个

```mssql
CREATE VIEW physics_fall_2009 AS
	SELECT	course.course_id, sec_id, building, room_number
	  FROM	course, [section]
	 WHERE	course.course_id = [section].course_id
			AND course.dept_name = 'Physics'
			AND [section].semester = 'Fall'
            AND [section].[year] = '2009';
```

### SQL查询中使用视图

就跟平时用关系一样：

```mssql
SELECT	course_id
  FROM	physics_fall_2009
 WHERE	building = 'Watson';
```

