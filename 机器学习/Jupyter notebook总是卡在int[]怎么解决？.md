# Jupyter notebook总是卡在int[*]怎么解决？

## 先看看后台的日志是怎么回事

运行Jupyter notebook会有一个命令行在运行，可以看看出现在error附近的的句子的意思再具体搜索解决方案。

![image-20200418104044628](https://raw.githubusercontent.com/HYBB-rash/cnBlogs/master/img/20200418104046.png)

## 提供可能的一个解决方案

有可能是ipykernel包版本不对。

```shell
pip install --upgrade ipykernel
```

