# DataFrame

DataFrame是一个类似于二维数组或表格(如excel)的对象，既有行索引，又有列索引

- 行索引，表明不同行，横向索引，叫index，0轴，axis=0
- 列索引，表名不同列，纵向索引，叫columns，1轴，axis=1

![img](E:\all sorts of learning programme software\tools\markdown\typore\Typora\images\df.png)

## DataFrame的创建

```python
# 导入pandas
import pandas as pd

pd.DataFrame(data=None, index=None, columns=None)
```

- 参数：
	- index：行标签。如果没有传入索引参数，则默认会自动创建一个从0-N的整数索引。
	- columns：列标签。如果没有传入索引参数，则默认会自动创建一个从0-N的整数索引。
- 通过已有数据创建

举例一：

```python
pd.DataFrame(np.random.randn(2,3))
```

![image-20190624084616637](E:\all sorts of learning programme software\tools\markdown\typore\Typora\images\dataframe创建举例.png)

回忆咱们在前面直接使用np创建的数组显示方式，比较两者的区别。

举例二：创建学生成绩表

```python
# 生成10名同学，5门功课的数据
score = np.random.randint(40, 100, (10, 5))

# 结果
array([[92, 55, 78, 50, 50],
       [71, 76, 50, 48, 96],
       [45, 84, 78, 51, 68],
       [81, 91, 56, 54, 76],
       [86, 66, 77, 67, 95],
       [46, 86, 56, 61, 99],
       [46, 95, 44, 46, 56],
       [80, 50, 45, 65, 57],
       [41, 93, 90, 41, 97],
       [65, 83, 57, 57, 40]])
```

**但是这样的数据形式很难看到存储的是什么的样的数据，可读性比较差！！**

**问题：如何让数据更有意义的显示**？

```python
# 使用Pandas中的数据结构
score_df = pd.DataFrame(score)
```

![image-20190624085446295](E:\all sorts of learning programme software\tools\markdown\typore\Typora\images\score1.png)

给分数数据增加行列索引,显示效果更佳

效果：

![image-20190624090129098](E:\all sorts of learning programme software\tools\markdown\typore\Typora\images\score2.png)

- 增加行、列索引

```python
# 构造行索引序列
subjects = ["语文", "数学", "英语", "政治", "体育"]

# 构造列索引序列
stu = ['同学' + str(i) for i in range(score_df.shape[0])]

# 添加行索引
data = pd.DataFrame(score, columns=subjects, index=stu)
```

##  DataFrame的属性

- **shape**

```python
data.shape

# 结果
(10, 5)
```

- **index**

DataFrame的行索引列表

```python
data.index

# 结果
Index(['同学0', '同学1', '同学2', '同学3', '同学4', '同学5', '同学6', '同学7', '同学8', '同学9'], dtype='object')
```

- **columns**

DataFrame的列索引列表

```python
data.columns

# 结果
Index(['语文', '数学', '英语', '政治', '体育'], dtype='object')
```

- **values**

直接获取其中array的值

```python
data.values

array([[92, 55, 78, 50, 50],
       [71, 76, 50, 48, 96],
       [45, 84, 78, 51, 68],
       [81, 91, 56, 54, 76],
       [86, 66, 77, 67, 95],
       [46, 86, 56, 61, 99],
       [46, 95, 44, 46, 56],
       [80, 50, 45, 65, 57],
       [41, 93, 90, 41, 97],
       [65, 83, 57, 57, 40]])
```

- **T**

转置

```
data.T
```

结果

![image-20190624094546890](E:\all sorts of learning programme software\tools\markdown\typore\Typora\images\score转置结果.png)

- **head(5)**：显示前5行内容

如果不补充参数，默认5行。填入参数N则显示前N行

```python
data.head(5)
```

![image-20190624094816880](E:\all sorts of learning programme software\tools\markdown\typore\Typora\images\score_head.png)

- **tail(5)**:显示后5行内容

如果不补充参数，默认5行。填入参数N则显示后N行

```python
data.tail(5)
```

##  DatatFrame索引的设置

需求：

![image-20190624095757620](E:\all sorts of learning programme software\tools\markdown\typore\Typora\images\score修改索引.png)

###  修改行列索引值

```python
stu = ["学生_" + str(i) for i in range(score_df.shape[0])]

# 必须整体全部修改
data.index = stu
```

注意：以下修改方式是错误的

```python
# 错误修改方式
data.index[3] = '学生_3'
```

###  重设索引

- reset_index(drop=False)
	- 设置新的下标索引
	- drop:默认为False，不删除原来索引，如果为True,删除原来的索引值

```python
# 重置索引,drop=False
data.reset_index()
```

![image-20190624100048415](E:\all sorts of learning programme software\tools\markdown\typore\Typora\images\重设索引1.png)

```python
# 重置索引,drop=True
data.reset_index(drop=True)
```

###  以某列值设置为新的索引

- set_index(keys, drop=True)
	- **keys** : 列索引名成或者列索引名称的列表
	- **drop** : boolean, default True.当做新的索引，删除原来的列

设置新索引案例

1、创建

```python
df = pd.DataFrame({'month': [1, 4, 7, 10],
                    'year': [2012, 2014, 2013, 2014],
                    'sale':[55, 40, 84, 31]})

   month  sale  year
0  1      55    2012
1  4      40    2014
2  7      84    2013
3  10     31    2014
```

2、以月份设置新的索引

```python
df.set_index('month')
       sale  year
month
1      55    2012
4      40    2014
7      84    2013
10     31    2014
```

3、设置多个索引，以年和月份

```python
df = df.set_index(['year', 'month'])
df
            sale
year  month
2012  1     55
2014  4     40
2013  7     84
2014  10    31
```

> 注：通过刚才的设置，这样DataFrame就变成了一个具有MultiIndex的DataFrame。

