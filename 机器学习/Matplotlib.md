# Matplotlib

擅长2D作图，不擅长3D作图

## 尝试画一张图

```python
# 1. 创建画布

plt.figure(figsize=(20,8),dpi=100)

# 2. 绘制图像

plt.plot([1,2,3,4,5,6,7],[10,15,13,18,16,20,10])

# 3. 图像显示

plt.show()
```

可以得到一个这样的图

![image-20200409174223704](E:\all sorts of learning programme software\tools\markdown\typore\Typora\upload\image-20200409174223704.png)

## 将ipynb文件转化成markdown文件

```shell
jupyter nbconvert --to markdown "文件名"
```

## 绘制基本图像


```python
# 0. 准备数据

x = range(60)
y_shanghai = [random.uniform(15,18) for i in x]

# 1. 创建画布

plt.figure(figsize=(20,8), dpi=80)

# 2. 绘制图像

plt.plot(x, y_shanghai)

# 3. 显示图像

plt.show()
```


![png](E:/code/ml/ai/Scripts/ml_code/1. 基础图像功能实现_files/1. 基础图像功能实现_3_0.png)


## 实现一些其他功能


```python
# 0. 准备数据

x = range(60)
y_shanghai = [random.uniform(15,18) for i in x]

# 1. 创建画布

plt.figure(figsize=(20,8), dpi=80)

# 2. 绘制图像

plt.plot(x, y_shanghai)

# 2.1 添加x，y刻度

x_ticks_label = ["11点{}分".format(i) for i in x]
y_ticks = range(40)

# 修改x,y轴坐标的刻度显示

plt.xticks(x[::5],x_ticks_label[::5]) # 坐标刻度不可用直接用字符串进行修改
plt.yticks(y_ticks[::5])

# 2.2 添加网格显示

plt.grid(True, ls='--', alpha=1)

# 2.3 添加描述信息

plt.xlabel('时间')
plt.ylabel('温度')
plt.title('中午11点-12点某城市温度变化图', fontsize=20)

# 2.4 图像保存

plt.savefig("./test.png") # 注意：plt.show()会释放figure资源，如果在显示图像之后保存图片将只能保存空图片。

# 3. 显示图像

plt.show()
```


![png](E:/code/ml/ai/Scripts/ml_code/1. 基础图像功能实现_files/1. 基础图像功能实现_5_0.png)


## 在一个坐标系中绘制多个图像


```python
# 0. 准备数据

x = range(60)
y_shanghai = [random.uniform(15,18) for i in x]

# 增加北京的温度数据

y_beijing = [random.uniform(1,3) for i in x]

# 1. 创建画布

plt.figure(figsize=(20,8), dpi=80)

# 2. 绘制图像

plt.plot(x, y_shanghai, label="上海")

# 使用多次plot可以画多个折线

plt.plot(x, y_beijing, 'r--', label="北京")

plt.legend(loc="best")

# 2.1 添加x，y刻度

x_ticks_label = ["11点{}分".format(i) for i in x]
y_ticks = range(40)

# 修改x,y轴坐标的刻度显示

plt.xticks(x[::5],x_ticks_label[::5]) # 坐标刻度不可用直接用字符串进行修改
plt.yticks(y_ticks[::5])

# 2.2 添加网格显示

plt.grid(True, ls='--', alpha=1)

# 2.3 添加描述信息

plt.xlabel('时间')
plt.ylabel('温度')
plt.title('中午11点-12点某城市温度变化图', fontsize=20)

# 2.4 图像保存

plt.savefig("./test.png") # 注意：plt.show()会释放figure资源，如果在显示图像之后保存图片将只能保存空图片。

# 3. 显示图像

plt.show()
```


![png](E:/code/ml/ai/Scripts/ml_code/1. 基础图像功能实现_files/1. 基础图像功能实现_7_0.png)


## 多个坐标系实现绘图


```python
# 0. 准备数据
x = range(60)
y_shanghai = [random.uniform(15,18) for i in x]

# 增加北京的温度数据
y_beijing = [random.uniform(1,3) for i in x]

# 1. 创建画布
fig, axes = plt.subplots(1,2,figsize=(20,8), dpi=80)

# 2. 绘制图像
axes[0].plot(x, y_shanghai, label="上海")
axes[1].plot(x, y_beijing, 'r--', label="北京")

# 2.1 添加x，y刻度

x_ticks_label = ["11点{}分".format(i) for i in x]
y_ticks = range(40)

# 2.2 刻度显示
axes[0].set_xticks(x[::5])
axes[0].set_yticks(y_ticks[::5])
axes[0].set_xticklabels(x_ticks_label[::5])
axes[1].set_xticks(x[::5])
axes[1].set_yticks(y_ticks[::5])
axes[1].set_xticklabels(x_ticks_label[::5])

# 2.3 网格显示
axes[0].grid(True, ls="--", alpha=0.5)
axes[1].grid(True, ls="--", alpha=0.5)

# 2.4 添加描述信息
axes[0].set_xlabel("时间")
axes[0].set_ylabel("温度")
axes[0].set_title("中午11点--12点某城市温度变化图", fontsize=20)
axes[1].set_xlabel("时间")
axes[1].set_ylabel("温度")
axes[1].set_title("中午11点--12点某城市温度变化图", fontsize=20)

# 2.5 保存图片
plt.savefig("./test.png")

# 2.6 添加图例
axes[0].legend(loc=0)
axes[1].legend(loc=0)

# 3. 显示图像
plt.show()
```


![png](E:/code/ml/ai/Scripts/ml_code/1. 基础图像功能实现_files/1. 基础图像功能实现_9_0.png)


## 折线图


```python
import numpy as np

# 0.准备数据
x = np.linspace(-10, 10, 1000)
y = np.sin(x)

# 1.创建画布
plt.figure(figsize=(20, 8), dpi=100)

# 2.绘制函数图像
plt.plot(x, y)
# 2.1 添加网格显示
plt.grid()

# 3.显示图像
plt.show()
```


![png](E:/code/ml/ai/Scripts/ml_code/1. 基础图像功能实现_files/1. 基础图像功能实现_11_0.png)

## 小结

- 添加x,y轴刻度【知道】
	- plt.xticks()
	- plt.yticks()
	- **注意:在传递进去的第一个参数必须是数字,不能是字符串,如果是字符串吗,需要进行替换操作**
- 添加网格显示【知道】
	- plt.grid(linestyle="--", alpha=0.5)
- 添加描述信息【知道】
	- plt.xlabel()
	- plt.ylabel()
	- plt.title()
- 图像保存【知道】
	- plt.savefig("路径")
- 多次plot【了解】
	- 直接进行添加就OK
- 显示图例【知道】
	- plt.legend(loc="best")
	- **注意:一定要在plt.plot()里面设置一个label,如果不设置,没法显示**
- 多个坐标系显示【了解】
	- plt.subplots(nrows=, ncols=)
- 折线图的应用【知道】
	- 1.应用于观察数据的变化
	- 2.可是画出一些数学函数图像

