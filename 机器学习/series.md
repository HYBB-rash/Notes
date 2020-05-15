# series

Series是一个类似于一维数组的数据结构，它能够保存任何类型的数据，比如整数、字符串、浮点数等，**主要由一组数据和与之相关的索引两部分构成。**

![img](E:\all sorts of learning programme software\tools\markdown\typore\Typora\images\Series.png)

## Series的创建

```python
# 导入pandas
import pandas as pd

pd.Series(data=None, index=None, dtype=None)
```

- 参数：
	- data：传入的数据，可以是ndarray、list等
	- index：索引，必须是唯一的，且与数据的长度相等。如果没有传入索引参数，则默认会自动创建一个从0-N的整数索引。
	- dtype：数据的类型

通过已有数据创建

- 指定内容，默认索引

```python
pd.Series(np.arange(10))
# 运行结果
0    0
1    1
2    2
3    3
4    4
5    5
6    6
7    7
8    8
9    9
dtype: int64
```

- 指定索引

```python
pd.Series([6.7,5.6,3,10,2], index=[1,2,3,4,5])
# 运行结果
1     6.7
2     5.6
3     3.0
4    10.0
5     2.0
dtype: float64
```

- 通过字典数据创建

```python
color_count = pd.Series({'red':100, 'blue':200, 'green': 500, 'yellow':1000})
color_count
# 运行结果
blue       200
green      500
red        100
yellow    1000
dtype: int64
```

## Series的属性

为了更方便地操作Series对象中的索引和数据，**Series中提供了两个属性index和values**

- index

```python
color_count.index

# 结果
Index(['blue', 'green', 'red', 'yellow'], dtype='object')
```

- values

```python
color_count.values

# 结果
array([ 200,  500,  100, 1000])
```

也可以使用索引来获取数据：

```python
color_count[2]

# 结果
100
```