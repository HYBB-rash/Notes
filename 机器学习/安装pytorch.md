# 安装pytorch

官网给出的命令是

```shell
conda install pytorch torchvision cpuonly -c pytorch
```

实际上我们不需要后面的`-c pytorch`，不信你试试，肯定报让你改url的错

请用这个命令

```shell
conda install pytorch torchvision cpuonly
```

## 作图

```python
def use_svg_display():
    display.set_matplotlib_formats('svg')
    
def set_figsize(figsize=(10, 6)):
    use_svg_display()
    plt.rcParams['figure.figsize'] = figsize
```

