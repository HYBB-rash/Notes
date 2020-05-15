# SQL

## SQL查询语言概览

## SQL数据定义

关于SQL对数据的定义。

### 基本类型

* `char(n)`：固定长度字符串，不可变，长度由用户指定
* `varchar(n)`：可变长度字符串，不可变，最大长度由用户指定
* `int`：整数类型
* `smallint`：小整数类型
* `numeric(p, d)`：定点数，精度由用户指定。
* `real, double precision`：浮点数和双精度浮点数
* `float(n)`：精度至少为`n`的浮点数

### 基本模式定义

#### 建表

实现一个简单的建表：

```mssql
create table SC(
	Sno char(8) not null,
	Cno char(3) not null,
	Grade tinyint not null,
	primary key (Sno, Cno),
	foreign key (Sno) references Student(Sno),
	foreign key (Cno) references Course(Cno)
);
```

* 数据定义：`name type(…) ,`
* 完整性约束：`… key (…) references table(…)`

完整性约束最常用：

* `primary key (…, …)` ：设置主键，非空且唯一
* `foreigh key (…) references table(...)` ：设置外键。
* `not null`：不允许为空值 

#### 插入

实现一个简单的插入：

```mssql
INSERT INTO classroom VALUES ('Packard', '101', '500');
```

不标明属性的顺序，默认按照表的顺序来插入。如果不希望按表的顺序来，可以：

```mssql
INSERT INTO classroom(..., ..., ...) VALUES ('Packard', '101', '500');
```

可以指定属性名进行插入。

#### 删除

```mssql
delete from student;
```

删除表中所有元组。但！表还在。

```mssql
delete table student;
```

这个就是直接把表都给删了。

#### 更改属性

```mssql
alter table r add name type(...);
```

给某表加上一个新属性。

```mssql
alter table r drop name;
```

给某表删去某个属性。

## SQL查询的基本结构

### 单关系查询

#### distinct

下面这段语句就能完成一个简单的单关系查询：

```mysql
SELECT		NAME
FROM 		instructor;
```

而我们有时候的数据单拿一个属性出来，数据是会大量重复的，比如我们执行下列语句：

```mysql
SELECT		dept_name
FROM 		instructor;
```

就会出现大量的重复。

在关系模型的数学定义中，我们不允许关系里有重复的元素。但在实践中，去重是一件非常耗时的事情。因此SQL的实现允许出现重复的数据。

我们也可以加入关键词distinct手动去重：

```mysql
SELECT DISTINCT dept_name
FROM 			instructor;
```

在SQL中默认不去重，但我们也可以显式地表达不去重：

```mysql
SELECT ALL	dept_name
FROM 		instructor;
```

这种写法可以，但没必要。只需要知道`distinct`即可。

#### 运算符

元组的属性可以使用运算符进行运算：

```mysql
SELECT		ID, NAME, dept_name, salary*1.1
FROM 		instructor;
```

它会将所有数据都显示成1.1倍。但实际上仅仅是显示而已，这种操作不会实际影响关系中的数据。

#### where

它与编程语言中的if十分相像，只不过表达方式稍稍有点区别：

```mysql
SELECT		NAME
FROM 		instructor
WHERE		dept_name = 'Comp. Sci.' AND salary > 70000;
```

这是它的简单用法，想好好用好得知道where实际的操作是什么，一会会提及。

### 多关系查询

实现一个简单的多表查询：

```mysql
SELECT		NAME, instructor.dept_name, building
FROM 		instructor, department
WHERE		instructor.`dept_name` = department.`dept_name`;
```

#### 一个SQL查询的含义

实际上，一个SQL可以被理解成下面三步：

1. 先是`from`子句，为列出的关系产生笛卡儿积。
2. 在是`where`语句，挑选出符合条件的元祖
3. 最后是`select`，输出指定的属性

假如这个多关系查询忘记写`where`，会产生一个非常大的无用数据集，因此，在使用多表查询的时候，一定不要忘记写`where`。

### 自然连接

实现一个简单的自然连接：

```mysql
SELECT		NAME, course_id
FROM 		instructor NATURAL JOIN teaches;
```

#### natural join

自然连接只考虑两个关系中都出现的属性上取值相同的元组对。

且对于`from`来说，`instructor NATURAL JOIN teaches`返回的结果是一个关系。

而自然连接也存在一些问题：很有可能会出现冲突的属性，或者是我们并不希望被合并的属性，被强行合并了：

```mysql
SELECT		NAME, title
FROM 		instructor NATURAL JOIN teaches, course
WHERE		teaches.`course_id` = course.`course_id`;

--------------------------------------------------------------------------------------------------------------
SELECT 		NAME, title
FROM 		instructor NATURAL JOIN teaches NATURAL JOIN course;
```

这两段运行结果看起来是一样的，实际上结果并不相同，因为`natural join`合并了我们并不期望合并的属性。

#### join (…) using (…)

为保留`natural join`的优点SQL提供了一种解决方案：

```mysql
SELECT		NAME, title
FROM 		(instructor NATURAL JOIN teaches) JOIN course USING (course_id);
```

使用`join (…) using (…)`结构，能够自己指定自然连接的属性，就不会出现冲突合并的情况

## 附加的基本运算

### 更名运算

实现一个简单的更名例子：

```mysql
SELECT		NAME AS instructor_name, course_id
FROM 		instructor, teaches
WHERE		instructor.`ID` = teaches.`ID`;
```

它最方便的地方就是可以将长名字变成短名字，同时可以用于区分同关系的。

### 字符串运算

字符串运算对大小写敏感，它不同于SQL语句。并且SQL库中会有丰富的处理字符串的函数，例如大写，小写，去空格……

在其中，还有一种操作符`like`可以完成模糊查询：

* %：匹配任意字符串
* _：匹配任意一个字符串

一个简单的实现例子：

```mysql
SELECT		dept_name
FROM 		department
WHERE		building LIKE '%Watson%';
```

有时候我们需要匹配%之类的符号，可以使用`escape`定义转义符：

同时也支持`not like`寻找不匹配项。

### select子句

`*`代表所有属性

### 元组的显示次序

我们可以让结果对次序进行控制。实现一个简单的例子：

```mysql
SELECT		NAME
FROM 		instructor
WHERE		dept_name = 'Physics'
ORDER BY	NAME;
```

`order by` 默认升序，我们可以给他指定顺序。用`asc`代表升序，`desc`代表降序。

```mysql
SELECT		*
FROM 		instructor
ORDER BY	salary DESC, NAME ASC;
```

### where子句

#### between and

`between and`可以用来表示范围

这段语句：

```mysql
SELECT		NAME
FROM 		instructor
WHERE		salary <= 100000 AND salary >= 90000;
```

与下面这段查询结果相同：

```mysql
SELECT		NAME
FROM 		instructor
WHERE		salary BETWEEN 90000 AND 100000;
```

并且可用`not between`。

#### 字典序比较

如果我们需要编写这么一段SQL：

```mysql
SELECT		NAME, course_id
FROM 		instructor, teaches
WHERE		instructor.`ID` = teaches.`ID` AND dept_name = 'Biology';
```

可以被改写成下列形式（按字典序比较）：

```mysql
SELECT		NAME, course_id
FROM 		instructor, teaches
WHERE		(instructor.`ID`, dept_name) = (teaches.`ID`, 'Biology');
```

## 集合运算

SQL总共提供了3中集合运算的方法

### 并运算

实现一个简单的`union`：

```mysql
(
	SELECT		course_id
	FROM 		section
	WHERE		semester = 'Fall' AND YEAR = 2009
)
UNION
(
	SELECT		course_id
	FROM 		section
	WHERE		semester = 'Spring' AND YEAR = 2010
);
```

`union` 运算自动去重。希望不去重，需要用`all` 关键字：

```mysql
(
	SELECT		course_id
	FROM 		section
	WHERE		semester = 'Fall' AND YEAR = 2009
)
UNION ALL
(
	SELECT		course_id
	FROM 		section
	WHERE		semester = 'Spring' AND YEAR = 2010
);
```

### 交运算

遗憾的是`MySQL` 没有提供`intersect` 关键字。我们需要自己实现这个功能：

```mysql
SELECT		s1.course_id
FROM 		section AS s1 JOIN section AS s2 USING (course_id)
WHERE		(s1.`semester`	, s2.`semester`	, s1.`year`	, s2.`year`) 
	= 		('Fall'			, 'Spring'		, 2009		, 2010);

```

这种写法保留了重复，因此我们加上`distinct` 去除重复：

```mysql
SELECT DISTINCT	s1.course_id
FROM 			section AS s1 JOIN section AS s2 USING (course_id)
WHERE			(s1.`semester`	, s2.`semester`	, s1.`year`	, s2.`year`) 
	= 			('Fall'			, 'Spring'		, 2009		, 2010);
```

### 差运算

也很遗憾，`MySQL` 没有实现。我们自己动手造轮子：

```

```

## 空值

`null` 是一个特殊值，它不能用简单的1或0来表示。应该算作是未定义。于是任何与空值的比较结果都不能简单地看成是`true` 还是`false` ，我们定义了另外一个结果值`unknown` 来表示与`null` 的比较结果。

### null

所以，我们判断空值，需要用关键词`is null` ：

```mysql
SELECT		NAME
FROM 		instructor
WHERE		salary IS NULL;
```

关键词`is not null` ：

```mysql
SELECT		NAME
FROM 		instructor
WHERE		salary IS NOT NULL;
```

`null` 可以被替换成`unknown` 。

值得注意的是，虽然我们认为`null` 属于未定义值，按逻辑说，两个`null` 拿出做`distinct` 的话，理应是被当作两个不同的元素。但实际上，SQL认为他们是相同的，只会保留一份。

## 聚集函数

SQL提供了5个固有聚集函数：

* 平均值：`avg`
* 最小值：`min`
* 最大值：`max`
* 总和：`sum`
* 计数：`count`

### 基本聚集

举一个平均值得聚集函数例子：

```mysql
SELECT		AVG(salary)
FROM 		instructor
WHERE		dept_name = 'Comp. Sci.';
```

聚集函数允许更名：

```mysql
SELECT		AVG(salary) AS avg_salary
FROM 		instructor
WHERE		dept_name = 'Comp. Sci.';
```

聚集函数中允许使用`distinct` 谓词：

```mysql
SELECT		COUNT(DISTINCT ID)
FROM 		teaches
WHERE		semester = 'Spring' AND YEAR = 2010;
```

但SQL不允许`*`使用`distinct`，只用`max`和`min`可以用，但是没啥意义。

### 分组聚集

简单实现一个分组聚集：

```mysql
SELECT		dept_name, AVG(salary) AS avg_salary
FROM 		instructor
GROUP BY	dept_name;
```

简单来说就是按给定的属性分组，只要值相同就会被分到同一组。

如果不指定，就默认整个关系为同一个分组。

需要注意的是，如果进行了分组，那么`select` 语句中只能出现`group by` 指定的属性而不能出现别的属性。别的属性只能出现在聚集函数的内部而不能直接被`select` ，否则这样的查询是错误的。

### having子句

简单实现一个`having`：

```mysql
SELECT		dept_name, AVG(salary) AS avg_salary
FROM 		instructor
GROUP BY	dept_name
HAVING		AVG(salary) > 42000;
```

`having` 子句是在形成分组后，才开始对分组进行条件限制。且`having` 子句对属性的要求与`group by` 相同。

整套查询的过程分为5步：

1. `from` 整理出一个关系。
2. `where` 对结果关系进行处理。
3. `group by` 对上一步结果进行分组。
4. `having` 对上一步结果进行限定
5. `select` 对上一步结果进行投影得到最终结果。

### 对空值与布尔值的聚集

SQL标准在聚集的时候会选择忽略`null` 值进行聚集。会出现一些奇怪的现象。有可能造成输出就是空值的情况

## 嵌套子查询

实际上就是先查一个表，把这个表当成已有的表来用。

### 集合成员资格

`in` 关键词可以查询某个值是否在关系中：

```mysql
SELECT DISTINCT course_id
FROM 		section
WHERE		semester = 'Fall' 
		AND YEAR = 2009 
		AND course_id IN(
			SELECT		course_id
			FROM 		section
			WHERE		semester = 'Spring' AND YEAR = 2010);

```

当然也可以用`not in`。

这种写法可以应用在集合运算中的交和差运算。

同时`in` 可以使用字典序查询

```mysql
SELECT	COUNT(DISTINCT ID)
FROM 	takes
WHERE	(course_id, sec_id, semester, YEAR) IN (SELECT	course_id, sec_id, semester, YEAR
												FROM 	teaches
												WHERE	teaches.`ID` = 10101);
```

### 集合的比较

用`some`关键字可以完成集合的比较。例如至少比某个集合中的某个数据要大：

```mysql
SELECT	NAME
FROM 	instructor
WHERE	salary > SOME(	SELECT	salary
						FROM 	instructor
						WHERE	dept_name = 'Biology');

```

`some`当然可以搭配各种不一样的运算符。

值得注意的是，`= some` 等价于关键字`in`， 但`<>some` 并不等价于`not in`：

```mysql
SELECT	NAME
FROM 	instructor
WHERE	salary <> SOME(	SELECT	salary
						FROM 	instructor
						WHERE	dept_name = 'Biology');

SELECT	NAME
FROM 	instructor
WHERE	salary NOT IN(	SELECT	salary
						FROM 	instructor
						WHERE	dept_name = 'Biology');	
```

两者逻辑是不一样的：

* `<>some`：$A_1 \neq B_1 \space or \space A_2 \neq B_2 \space or \space A_3 \neq B_3 \space \dots$
* `not in` ：$A_1 \neq B_1 \space and \space A_2 \neq B_2 \space and \space A_3 \neq B_3 \space \dots$

所以两个结果并不相同。

### 空关系测试

`exists` 关键字可以用来测试一个子查询的结果是否存在元组：

```mysql
SELECT	course_id
FROM 	section AS S
WHERE	semester = 'Fall' AND YEAR = 2009
		AND EXISTS(	SELECT	*
					FROM 	section AS T
					WHERE	semester = 'Spring' AND YEAR = 2010 AND S.`course_id` = T.`course_id`);
```

`exists` 内部子查询有结果集，就返回`true` ，如果是空集返回`false` 。

#### 相关子查询

上面这个例子，我们可以看到，子查询调用了外部的表。它与各大语言中的局部变量和全局变量类似。内层的子结构变量的作用域只能作用于内层，外层无法调用内部的变量，只能调用子查询的结果集。而外层的变量作用域覆盖到了内层，内层可以使用外层的变量。

### 重复元组存在性测试

`unique` 用于查询子查询中是否存在重复元组：

此结构未被广泛实现。不理会了。

### from子句中的子查询

实现一个`from` 子查询，它能办到很多单靠`group by`办不到的事：

```mysql
SELECT	dept_name, avg_salary
FROM 	(	SELECT		dept_name, AVG(salary) AS avg_salary
			FROM 		instructor
			GROUP BY	dept_name)
			AS 	dept_avg
WHERE	avg_salary > 42000;
```

### with子句

实际上就是将子查询写在外面，起一个别名，让整段查询逻辑清晰：

```mssql
WITH	max_budget(VALUE) AS
	(	SELECT	MAX(budget)
		FROM 	department)
SELECT	budget
FROM 	department, max_budget
WHERE	department.budget = max_budget.value;
```

感叹一下，MySQL连with都不支持……

### 标量子查询

SQL支持当你确定该子查询结果一定只有一个结果时，将该子查询当成一个值来用。

## 数据库修改

### 删除

删除只能删除一个元组，不能只删除一个数据。

```mssql
delete from instructor
where dept_name = 'Finance';
```

记得写`where` 不然删的是整个表。



