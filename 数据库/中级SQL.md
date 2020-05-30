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

视图可以显式地指定名字：

```mssql
CREATE VIEW department_total_salary(dept_name, total_salary) AS
	  SELECT	dept_name, SUM(salary)
	    FROM	instructor
	GROUP BY	dept_name;
```

虽然没指定`SUM`的名字，但是可以在定义视图的时候使用其中的名字。

视图可以嵌套使用：

```mssql
CREATE VIEW physics_fall_2009_watson AS
	SELECT	course_id, room_number
	  FROM	physics_fall_2009
     WHERE	building = 'Watson';
```

### 物化视图

视图中使用的实际关系如果发生改变，那视图的查询结果也会改变。

这一过程称为物化视图维护。

### 视图更新

视图的更新是受限制的：

1. `from`语句只有一个关系。
2. `select`子句只包含有关系的属性名，不包含其他表达式。
3. 属性值可以取`null`。
4. 查询中不包含`group by`或者`having`。

满足以上限制，视图允许执行`update`，`insert`，`delete`操作。

视图末尾可以定义`with check option`。只要不满足`where`条件，更新操作也不会成功。

## 事务

SQL标准规定当一条SQL语句被执行，就相当于开始了一个事务。

1. `commit work`提交当前事务。对该事务中做的操作在数据库中做永久性更新。
2. `rollback work`回滚当前事务。撤销对事务中做的操作。

一旦执行了`commit work`就不能用`rollback work`回滚。

## 完整性约束

完整性约束保证授权用户对数据库的操作不会破坏数据的一致性。

### 单关系上的约束

在`create table`命令中允许的完整性约束有：

1. `not null`
2. `unique`
3. `check (<谓词>)`

#### `not null`约束

声明之后，不能插入空值。主键默认有`not null`。

#### `unique`约束

声明之后，不能插入属性值相同的值。

注意：`null`值是一个例外。`null <> null`

#### `check`子句约束

声明之后，要满足`check`子句谓词的的限制。它可以是满足枚举类型。因此应该按照标准是可以接`select`语句。但是没有一家数据库实现了这个东西。

### 参照完整性

建立外键的时候，是将几张表联系了起来。因此，在某些操作中，是不能直接删除子表的的数据，否则主表的数据会在子表中不存在。

这种约束叫做参照完整性约束或者子集依赖。

默认情况下，外码参照的是子表的主码属性。SQL支持显示指定被参照关系的属性列表。但是这个约束一定得是主键或者有`unique`约束。

```mssql
dept_name varchar(20) references department 
```

如果违反了参照完整性约束，通常是拒绝操作。

但是实际上可以声明级联删除来恢复完整性约束。

通过增加：

```mssql
on delete cascade
on delete cascade
```

来级联更新，可以`set default`，可以`set null`。

### 事务中对完整性约束得违反

略

### 复杂`check`条件与断言

略

