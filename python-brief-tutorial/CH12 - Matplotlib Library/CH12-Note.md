- [CH12 - Matplotlib库](#ch12---matplotlib库)
  - [12.0 环境配置](#120-环境配置)
  - [12.1 Matplotlib库](#121-matplotlib库)
    - [12.1.1 折线图](#1211-折线图)
        - [marker设置坐标点](#marker设置坐标点)
        - [markersize 设置坐标点大小](#markersize-设置坐标点大小)
        - [颜色跟风格设置的简写](#颜色跟风格设置的简写)
        - [颜色\_风格\_线性 设置的简写](#颜色_风格_线性-设置的简写)
      - [调整坐标轴](#调整坐标轴)
      - [设置图形标签](#设置图形标签)
    - [12.1.2 散点图](#1212-散点图)
    - [12.1.3 柱形图](#1213-柱形图)
    - [12.1.4 多子图](#1214-多子图)
    - [12.1.5 直方图](#1215-直方图)
    - [12.1.6 误差图](#1216-误差图)
    - [12.1.7 面向对象的风格简介](#1217-面向对象的风格简介)
    - [12.1.8 三维图形简介](#1218-三维图形简介)
  - [12.2 Seaborn库-文艺青年的最爱](#122-seaborn库-文艺青年的最爱)
  - [12.3 Pandas 中的绘图函数概览](#123-pandas-中的绘图函数概览)




# CH12 - Matplotlib库

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221007090551954.png)

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221007090608537.png)

## 12.0 环境配置 

【1】 要不要plt.show()

* ipython中可用魔术方法   %matplotlib inline

* pycharm 中必须使用plt.show()


```python
%matplotlib inline # 配置，可以再ipython中生成就显示，而不需要多余plt.show来完成。
import matplotlib.pyplot as plt 
plt.style.use("seaborn-whitegrid") # 用来永久地改变风格，与下文with临时改变进行对比
```


```python
x = [1, 2, 3, 4]
y = [1, 4, 9, 16]
plt.plot(x, y)
plt.ylabel("squares")
# plt.show()   
```




    Text(0, 0.5, 'squares')




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_7_1.png)
    


【2】设置样式


```python
plt.style.available[:5]
```




    ['bmh', 'classic', 'dark_background', 'fast', 'fivethirtyeight']



临时地改变风格，采用with这个上下文管理器。


```python
with plt.style.context("seaborn-white"):
    plt.plot(x, y)
```


![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_11_0.png)
    


【3】将图像保存为文件


```python
import numpy as np
x = np.linspace(0, 10 ,100)
plt.plot(x, np.exp(x))
plt.savefig("my_figure.png")
```


![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_13_0.png)
    

## 12.1 Matplotlib库

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221007090619966.png)

### 12.1.1 折线图


```python
%matplotlib inline
import matplotlib.pyplot as plt
plt.style.use("seaborn-whitegrid")
import numpy as np
```


```python
x = np.linspace(0, 2*np.pi, 100)
plt.plot(x, np.sin(x))
```


    [<matplotlib.lines.Line2D at 0x18846169780>]


![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_17_1.png)
    


* 绘制多条曲线


```python
x = np.linspace(0, 2*np.pi, 100)
plt.plot(x, np.cos(x))
plt.plot(x, np.sin(x))
```




    [<matplotlib.lines.Line2D at 0x1884615f9e8>]




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_19_1.png)
    


【1】调整线条颜色和风格

* 调整线条颜色


```python
offsets = np.linspace(0, np.pi, 5)
colors = ["blue", "g", "r", "yellow", "pink"]
for offset, color in zip(offsets, colors):
    plt.plot(x, np.sin(x-offset), color=color)         # color可缩写为c
```


![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_22_0.png)
    


* 调整线条风格


```python
x = np.linspace(0, 10, 11)
offsets = list(range(8))
linestyles = ["solid", "dashed", "dashdot", "dotted", "-", "--", "-.", ":"]
for offset, linestyle in zip(offsets, linestyles):
    plt.plot(x, x+offset, linestyle=linestyle)        # linestyle可简写为ls
```


![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_24_0.png)
    


* 调整线宽


```python
x = np.linspace(0, 10, 11)
offsets = list(range(0, 12, 3))
linewidths = (i*2 for i in range(1,5))
for offset, linewidth in zip(offsets, linewidths):
    plt.plot(x, x+offset, linewidth=linewidth)                 # linewidth可简写为lw
```


![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_26_0.png)
    


* 调整数据点标记

##### marker设置坐标点


```python
x = np.linspace(0, 10, 11)
offsets = list(range(0, 12, 3))
markers = ["*", "+", "o", "s"]
for offset, marker in zip(offsets, markers):
    plt.plot(x, x+offset, marker=marker)   
```


![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_29_0.png)
    


##### markersize 设置坐标点大小


```python
x = np.linspace(0, 10, 11)
offsets = list(range(0, 12, 3))
markers = ["*", "+", "o", "s"]
for offset, marker in zip(offsets, markers):
    plt.plot(x, x+offset, marker=marker, markersize=10)      # markersize可简写为ms
```


![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_31_0.png)
    

##### 颜色跟风格设置的简写 

color_linestyles = ["g-", "b--", "k-.", "r:"]


```python
x = np.linspace(0, 10, 11)
offsets = list(range(0, 8, 2))
color_linestyles = ["g-", "b--", "k-.", "r:"]
for offset, color_linestyle in zip(offsets, color_linestyles):
    plt.plot(x, x+offset, color_linestyle)
```


![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_33_0.png)
    

##### 颜色\_风格\_线性 设置的简写 

color_marker_linestyles = ["g*-", "b+--", "ko-.", "rs:"]


```python
x = np.linspace(0, 10, 11)
offsets = list(range(0, 8, 2))
color_marker_linestyles = ["g*-", "b+--", "ko-.", "rs:"]
for offset, color_marker_linestyle in zip(offsets, color_marker_linestyles):
    plt.plot(x, x+offset, color_marker_linestyle)
```


![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_35_0.png)
​    


其他用法及颜色缩写、数据点标记缩写等请查看官方文档，如下：

https://matplotlib.org/api/_as_gen/matplotlib.pyplot.plot.html#matplotlib.pyplot.plot

#### 调整坐标轴

* xlim, ylim # 限制x，y轴


```python
x = np.linspace(0, 2*np.pi, 100)
plt.plot(x, np.sin(x))
plt.xlim(-1, 7)
plt.ylim(-1.5, 1.5)
```




    (-1.5, 1.5)




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_40_1.png)
    


* axis


```python
x = np.linspace(0, 2*np.pi, 100)
plt.plot(x, np.sin(x))
plt.axis([-2, 8, -2, 2])
```




    [-2, 8, -2, 2]




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_42_1.png)
    




tight 会紧凑一点


```python
x = np.linspace(0, 2*np.pi, 100)
plt.plot(x, np.sin(x))
plt.axis("tight")
```




    (0.0, 6.283185307179586, -0.9998741276738751, 0.9998741276738751)




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_45_1.png)
    


equal 会松一点


```python
x = np.linspace(0, 2*np.pi, 100)
plt.plot(x, np.sin(x))
plt.axis("equal")
```




    (0.0, 7.0, -1.0, 1.0)




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_47_1.png)
    



```python
?plt.axis # 可以查询其中的功能
```

    Object `plt.axis # 可以查询其中的功能` not found.


* 对数坐标


```python
x = np.logspace(0, 5, 100)
plt.plot(x, np.log(x))
plt.xscale("log")
```


![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_50_0.png)
    


* 调整坐标轴刻度

plt.xticks(np.arange(0, 12, step=1))


```python
x = np.linspace(0, 10, 100)
plt.plot(x, x**2)
plt.xticks(np.arange(0, 12, step=1))
```




    ([<matplotlib.axis.XTick at 0x18846412828>,
      <matplotlib.axis.XTick at 0x18847665898>,
      <matplotlib.axis.XTick at 0x18847665630>,
      <matplotlib.axis.XTick at 0x18847498978>,
      <matplotlib.axis.XTick at 0x18847498390>,
      <matplotlib.axis.XTick at 0x18847497d68>,
      <matplotlib.axis.XTick at 0x18847497748>,
      <matplotlib.axis.XTick at 0x18847497438>,
      <matplotlib.axis.XTick at 0x1884745f438>,
      <matplotlib.axis.XTick at 0x1884745fd68>,
      <matplotlib.axis.XTick at 0x18845fcf4a8>,
      <matplotlib.axis.XTick at 0x18845fcf320>],
     <a list of 12 Text xticklabel objects>)




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_52_1.png)
    



```python
x = np.linspace(0, 10, 100)
plt.plot(x, x**2)
plt.xticks(np.arange(0, 12, step=1), fontsize=15)
plt.yticks(np.arange(0, 110, step=10))
```




    ([<matplotlib.axis.YTick at 0x188474f0860>,
      <matplotlib.axis.YTick at 0x188474f0518>,
      <matplotlib.axis.YTick at 0x18847505a58>,
      <matplotlib.axis.YTick at 0x188460caac8>,
      <matplotlib.axis.YTick at 0x1884615c940>,
      <matplotlib.axis.YTick at 0x1884615cdd8>,
      <matplotlib.axis.YTick at 0x1884615c470>,
      <matplotlib.axis.YTick at 0x1884620c390>,
      <matplotlib.axis.YTick at 0x1884611f898>,
      <matplotlib.axis.YTick at 0x188461197f0>,
      <matplotlib.axis.YTick at 0x18846083f98>],
     <a list of 11 Text yticklabel objects>)




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_53_1.png)
    


* 调整刻度样式

plt.tick_params(axis="both", labelsize=15)


```python
x = np.linspace(0, 10, 100)
plt.plot(x, x**2)
plt.tick_params(axis="both", labelsize=15)
```


![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_55_0.png)
    


#### 设置图形标签


```python
x = np.linspace(0, 2*np.pi, 100)
plt.plot(x, np.sin(x))
plt.title("A Sine Curve", fontsize=20)
plt.xlabel("x", fontsize=15)
plt.ylabel("sin(x)", fontsize=15)
```




    Text(0, 0.5, 'sin(x)')




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_57_1.png)
    


【4】设置图例

* 默认


```python
x = np.linspace(0, 2*np.pi, 100)
plt.plot(x, np.sin(x), "b-", label="Sin")
plt.plot(x, np.cos(x), "r--", label="Cos")
plt.legend()
```




    <matplotlib.legend.Legend at 0x1884749f908>




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_60_1.png)
    


* 修饰图例


```python
import matplotlib.pyplot as plt 
import numpy as np
x = np.linspace(0, 2*np.pi, 100)
plt.plot(x, np.sin(x), "b-", label="Sin")
plt.plot(x, np.cos(x), "r--", label="Cos")
plt.ylim(-1.5, 2)
plt.legend(loc="upper center", frameon=True, fontsize=15) # frameon=True增加图例的边框
```




    <matplotlib.legend.Legend at 0x19126b53b80>




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_62_1.png)
    


【5】添加文字和箭头

* 添加文字


```python
x = np.linspace(0, 2*np.pi, 100)
plt.plot(x, np.sin(x), "b-")
plt.text(3.5, 0.5, "y=sin(x)", fontsize=15) # 前两个为文字的坐标，后面是内容和字号
```




    Text(3.5, 0.5, 'y=sin(x)')




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_65_1.png)
    


* 添加箭头


```python
x = np.linspace(0, 2*np.pi, 100)
plt.plot(x, np.sin(x), "b-")
plt.annotate('local min', xy=(1.5*np.pi, -1), xytext=(4.5, 0),
             arrowprops=dict(facecolor='black', shrink=0.1),
             )
```




    Text(4.5, 0, 'local min')




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_67_1.png)
    


### 12.1.2 散点图

【1】简单散点图


```python
x = np.linspace(0, 2*np.pi, 20)
plt.scatter(x, np.sin(x), marker="o", s=30, c="r")    # s 大小  c 颜色
```




    <matplotlib.collections.PathCollection at 0x188461eb4a8>




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_70_1.png)
    


【2】颜色配置


```python
x = np.linspace(0, 10, 100)
y = x**2
plt.scatter(x, y, c=y, cmap="inferno")  # 让c随着y的值变化在cmap中进行映射
plt.colorbar() # 输出颜色条
```




    <matplotlib.colorbar.Colorbar at 0x18848d392e8>




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_72_1.png)
    


颜色配置参考官方文档

https://matplotlib.org/examples/color/colormaps_reference.html

【3】根据数据控制点的大小


```python
x, y, colors, size = (np.random.rand(100) for i in range(4))
plt.scatter(x, y, c=colors, s=1000*size, cmap="viridis")
```




    <matplotlib.collections.PathCollection at 0x18847b48748>




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_76_1.png)
    


【4】透明度


```python
x, y, colors, size = (np.random.rand(100) for i in range(4))
plt.scatter(x, y, c=colors, s=1000*size, cmap="viridis", alpha=0.3)
plt.colorbar()
```




    <matplotlib.colorbar.Colorbar at 0x18848f2be10>




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_78_1.png)
    


【例】随机漫步


```python
from random import choice

class RandomWalk():
    """一个生产随机漫步的类"""
    def __init__(self, num_points=5000):
        self.num_points = num_points
        self.x_values = [0]
        self.y_values = [0]
    
    def fill_walk(self):
        while len(self.x_values) < self.num_points:
            x_direction = choice([1, -1])
            x_distance = choice([0, 1, 2, 3, 4])
            x_step = x_direction * x_distance
            
            y_direction = choice([1, -1])
            y_distance = choice([0, 1, 2, 3, 4])
            y_step = y_direction * y_distance            
        
            if x_step == 0 or y_step == 0:
                continue
            next_x = self.x_values[-1] + x_step
            next_y = self.y_values[-1] + y_step
            self.x_values.append(next_x)
            self.y_values.append(next_y)
```


```python
rw = RandomWalk(10000)
rw.fill_walk()
point_numbers = list(range(rw.num_points))
plt.figure(figsize=(12, 6))        # 设置画布大小        
plt.scatter(rw.x_values, rw.y_values, c=point_numbers, cmap="inferno", s=1)
plt.colorbar()
plt.scatter(0, 0, c="green", s=100)
plt.scatter(rw.x_values[-1], rw.y_values[-1], c="red", s=100)

plt.xticks([])
plt.yticks([])
```




    ([], <a list of 0 Text yticklabel objects>)




​    
![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_81_1.png)
​    


### 12.1.3 柱形图

【1】简单柱形图


```python
x = np.arange(1, 6)
plt.bar(x, 2*x, align="center", width=0.5, alpha=0.5, color='yellow', edgecolor='red')
plt.tick_params(axis="both", labelsize=13)
```


![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_84_0.png)
    



```python
x = np.arange(1, 6)
plt.bar(x, 2*x, align="center", width=0.5, alpha=0.5, color='yellow', edgecolor='red')
plt.xticks(x, ('G1', 'G2', 'G3', 'G4', 'G5'))
plt.tick_params(axis="both", labelsize=13) 
```


![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_85_0.png)
    



```python
x = ('G1', 'G2', 'G3', 'G4', 'G5')
y = 2 * np.arange(1, 6)
plt.bar(x, y, align="center", width=0.5, alpha=0.5, color='yellow', edgecolor='red')
plt.tick_params(axis="both", labelsize=13) 
```


![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_86_0.png)
    



```python
x = ["G"+str(i) for i in range(5)]
y = 1/(1+np.exp(-np.arange(5)))

colors = ['red', 'yellow', 'blue', 'green', 'gray']
plt.bar(x, y, align="center", width=0.5, alpha=0.5, color=colors)
plt.tick_params(axis="both", labelsize=13)
```


![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_87_0.png)
    


【2】累加柱形图


```python
x = np.arange(5)
y1 = np.random.randint(20, 30, size=5)
y2 = np.random.randint(20, 30, size=5)
plt.bar(x, y1, width=0.5, label="man")
plt.bar(x, y2, width=0.5, bottom=y1, label="women")
plt.legend()
```




    <matplotlib.legend.Legend at 0x2052db25cc0>




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_89_1.png)
    


【3】并列柱形图


```python
x = np.arange(15)
y1 = x+1
y2 = y1+np.random.random(15)
plt.bar(x, y1, width=0.3, label="man")
plt.bar(x+0.3, y2, width=0.3, label="women")
plt.legend()
```




    <matplotlib.legend.Legend at 0x2052daf35f8>




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_91_1.png)
    


【4】横向柱形图barh


```python
x = ['G1', 'G2', 'G3', 'G4', 'G5']
y = 2 * np.arange(1, 6)
plt.barh(x, y, align="center", height=0.5, alpha=0.8, color="blue", edgecolor="red") # 注意这里将bar改为barh，宽度用height设置
plt.tick_params(axis="both", labelsize=13)
```


![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_93_0.png)
    


### 12.1.4 多子图

【1】简单多子图


```python
def f(t):
    return np.exp(-t) * np.cos(2*np.pi*t)

t1 = np.arange(0.0, 5.0, 0.1)
t2 = np.arange(0.0, 5.0, 0.02)

plt.subplot(211)
plt.plot(t1, f(t1), "bo-", markerfacecolor="r", markersize=5)
plt.title("A tale of 2 subplots")
plt.ylabel("Damped oscillation")

plt.subplot(212)
plt.plot(t2, np.cos(2*np.pi*t2), "r--")
plt.xlabel("time (s)")
plt.ylabel("Undamped")
```




    Text(0, 0.5, 'Undamped')




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_96_1.png)
    


【2】多行多列子图


```python
x = np.random.random(10)
y = np.random.random(10)

plt.subplots_adjust(hspace=0.5, wspace=0.3)

plt.subplot(321)
plt.scatter(x, y, s=80, c="b", marker=">")

plt.subplot(322)
plt.scatter(x, y, s=80, c="g", marker="*")

plt.subplot(323)
plt.scatter(x, y, s=80, c="r", marker="s")

plt.subplot(324)
plt.scatter(x, y, s=80, c="c", marker="p")

plt.subplot(325)
plt.scatter(x, y, s=80, c="m", marker="+")

plt.subplot(326)
plt.scatter(x, y, s=80, c="y", marker="H")
```




    <matplotlib.collections.PathCollection at 0x2052d9f63c8>




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_98_1.png)
    


【3】不规则多子图


```python
def f(x):
    return np.exp(-x) * np.cos(2*np.pi*x)


x = np.arange(0.0, 3.0, 0.01)
grid = plt.GridSpec(2, 3, wspace=0.4, hspace=0.3) # 两行三列的网格

plt.subplot(grid[0, 0]) # 第一行第一列位置
plt.plot(x, f(x))

plt.subplot(grid[0, 1:]) # 第一行后两列的位置
plt.plot(x, f(x), "r--", lw=2)

plt.subplot(grid[1, :]) # 第二行所有位置
plt.plot(x, f(x), "g-.", lw=3)
```




    [<matplotlib.lines.Line2D at 0x2052d6fae80>]




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_100_1.png)
    


### 12.1.5 直方图

【1】普通频次直方图


```python
mu, sigma = 100, 15
x = mu + sigma * np.random.randn(10000)

plt.hist(x, bins=50, facecolor='g', alpha=0.75)
```




    (array([  1.,   0.,   0.,   5.,   3.,   5.,   1.,  10.,  15.,  19.,  37.,
             55.,  81.,  94., 125., 164., 216., 258., 320., 342., 401., 474.,
            483., 590., 553., 551., 611., 567., 515., 558., 470., 457., 402.,
            347., 261., 227., 206., 153., 128.,  93.,  79.,  41.,  22.,  17.,
             21.,   9.,   2.,   8.,   1.,   2.]),
     array([ 40.58148736,  42.82962161,  45.07775586,  47.32589011,
             49.57402436,  51.82215862,  54.07029287,  56.31842712,
             58.56656137,  60.81469562,  63.06282988,  65.31096413,
             67.55909838,  69.80723263,  72.05536689,  74.30350114,
             76.55163539,  78.79976964,  81.04790389,  83.29603815,
             85.5441724 ,  87.79230665,  90.0404409 ,  92.28857515,
             94.53670941,  96.78484366,  99.03297791, 101.28111216,
            103.52924641, 105.77738067, 108.02551492, 110.27364917,
            112.52178342, 114.76991767, 117.01805193, 119.26618618,
            121.51432043, 123.76245468, 126.01058893, 128.25872319,
            130.50685744, 132.75499169, 135.00312594, 137.25126019,
            139.49939445, 141.7475287 , 143.99566295, 146.2437972 ,
            148.49193145, 150.74006571, 152.98819996]),
     <a list of 50 Patch objects>)




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_103_1.png)
    


【2】概率密度


```python
mu, sigma = 100, 15
x = mu + sigma * np.random.randn(10000)

plt.hist(x, 50, density=True, color="r")# 概率密度图
plt.xlabel('Smarts')
plt.ylabel('Probability')
plt.title('Histogram of IQ')
plt.text(60, .025, r'$\mu=100,\ \sigma=15$')
plt.xlim(40, 160)
plt.ylim(0, 0.03)
```




    (0, 0.03)




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_105_1.png)
    



```python
mu, sigma = 100, 15
x = mu + sigma * np.random.randn(10000)

plt.hist(x, bins=50, density=True, color="r", histtype='step') #不填充，只获得边缘
plt.xlabel('Smarts')
plt.ylabel('Probability')
plt.title('Histogram of IQ')
plt.text(60, .025, r'$\mu=100,\ \sigma=15$')
plt.xlim(40, 160)
plt.ylim(0, 0.03)
```




    (0, 0.03)




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_106_1.png)
    



```python
from scipy.stats import norm
mu, sigma = 100, 15 # 想获得真正高斯分布的概率密度图
x = mu + sigma * np.random.randn(10000)
# 先获得bins，即分配的区间
_, bins, __ = plt.hist(x, 50, density=True)
y = norm.pdf(bins, mu, sigma) # 通过norm模块计算符合的概率密度
plt.plot(bins, y, 'r--', lw=3)  
plt.xlabel('Smarts')
plt.ylabel('Probability')
plt.title('Histogram of IQ')
plt.text(60, .025, r'$\mu=100,\ \sigma=15$')
plt.xlim(40, 160)
plt.ylim(0, 0.03)
```




    (0, 0.03)




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_107_1.png)
    


【3】累计概率分布


```python
mu, sigma = 100, 15
x = mu + sigma * np.random.randn(10000)

plt.hist(x, 50, density=True, cumulative=True, color="r") # 将累计cumulative设置为true即可
plt.xlabel('Smarts')
plt.ylabel('Cum_Probability')
plt.title('Histogram of IQ')
plt.text(60, 0.8, r'$\mu=100,\ \sigma=15$')
plt.xlim(50, 165)
plt.ylim(0, 1.1)
```




    (0, 1.1)




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_109_1.png)
    


【例】模拟投两个骰子


```python
class Die():
    "模拟一个骰子的类"
    
    def __init__(self, num_sides=6):
        self.num_sides = num_sides
    
    def roll(self):
        return np.random.randint(1, self.num_sides+1)
```

* 重复投一个骰子


```python
die = Die()
results = []
for i in range(60000):
    result = die.roll()
    results.append(result)
    
plt.hist(results, bins=6, range=(0.75, 6.75), align="mid", width=0.5)
plt.xlim(0 ,7)
```




    (0, 7)




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_113_1.png)
    


* 重复投两个骰子


```python
die1 = Die()
die2 = Die()
results = []
for i in range(60000):
    result = die1.roll()+die2.roll()
    results.append(result)
    
plt.hist(results, bins=11, range=(1.75, 12.75), align="mid", width=0.5)
plt.xlim(1 ,13)
plt.xticks(np.arange(1, 14))
```




    ([<matplotlib.axis.XTick at 0x2052fae23c8>,
      <matplotlib.axis.XTick at 0x2052ff1fa20>,
      <matplotlib.axis.XTick at 0x2052fb493c8>,
      <matplotlib.axis.XTick at 0x2052e9b5a20>,
      <matplotlib.axis.XTick at 0x2052e9b5e80>,
      <matplotlib.axis.XTick at 0x2052e9b5978>,
      <matplotlib.axis.XTick at 0x2052e9cc668>,
      <matplotlib.axis.XTick at 0x2052e9ccba8>,
      <matplotlib.axis.XTick at 0x2052e9ccdd8>,
      <matplotlib.axis.XTick at 0x2052fac5668>,
      <matplotlib.axis.XTick at 0x2052fac5ba8>,
      <matplotlib.axis.XTick at 0x2052fac5dd8>,
      <matplotlib.axis.XTick at 0x2052fad9668>],
     <a list of 13 Text xticklabel objects>)




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_115_1.png)
    


### 12.1.6 误差图

【1】基本误差图


```python
x = np.linspace(0, 10 ,50)
dy = 0.5 # 每个点的y值误差设置为0.5
y = np.sin(x) + dy*np.random.randn(50)

plt.errorbar(x, y , yerr=dy, fmt="+b")
```




    <ErrorbarContainer object of 3 artists>




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_118_1.png)
    


【2】柱形图误差图


```python
menMeans = (20, 35, 30, 35, 27)
womenMeans = (25, 32, 34, 20, 25)
menStd = (2, 3, 4, 1, 2)
womenStd = (3, 5, 2, 3, 3)
ind = ['G1', 'G2', 'G3', 'G4', 'G5'] 
width = 0.35       

p1 = plt.bar(ind, menMeans, width=width, label="Men", yerr=menStd)
p2 = plt.bar(ind, womenMeans, width=width, bottom=menMeans, label="Men", yerr=womenStd)

plt.ylabel('Scores')
plt.title('Scores by group and gender')
plt.yticks(np.arange(0, 81, 10))
plt.legend()
```




    <matplotlib.legend.Legend at 0x20531035630>




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_120_1.png)
    


### 12.1.7 面向对象的风格简介

【例1】 普通图


```python
x = np.linspace(0, 5, 10)
y = x ** 2

fig = plt.figure(figsize=(8,4), dpi=80)        # 图像
axes = fig.add_axes([0.1, 0.1, 0.8, 0.8])      # 轴 left, bottom, width, height (range 0 to 1)

axes.plot(x, y, 'r')
axes.set_xlabel('x')
axes.set_ylabel('y')
axes.set_title('title')
```




    Text(0.5, 1.0, 'title')




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_123_1.png)
    


【2】画中画


```python
x = np.linspace(0, 5, 10)
y = x ** 2

fig = plt.figure()

ax1 = fig.add_axes([0.1, 0.1, 0.8, 0.8]) 
ax2 = fig.add_axes([0.2, 0.5, 0.4, 0.3]) 

ax1.plot(x, y, 'r')

ax1.set_xlabel('x')
ax1.set_ylabel('y')
ax1.set_title('title')

ax2.plot(y, x, 'g')
ax2.set_xlabel('y')
ax2.set_ylabel('x')
ax2.set_title('insert title')
```




    Text(0.5, 1.0, 'insert title')




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_125_1.png)
    


【3】 多子图


```python
def f(t):
    return np.exp(-t) * np.cos(2*np.pi*t)


t1 = np.arange(0.0, 3.0, 0.01)

fig= plt.figure()
fig.subplots_adjust(hspace=0.4, wspace=0.4)

ax1 = plt.subplot(2, 2, 1)
ax1.plot(t1, f(t1))
ax1.set_title("Upper left")

ax2 = plt.subplot(2, 2, 2)
ax2.plot(t1, f(t1))
ax2.set_title("Upper right")

ax3 = plt.subplot(2, 1, 2)
ax3.plot(t1, f(t1))
ax3.set_title("Lower")
```




    Text(0.5, 1.0, 'Lower')




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_127_1.png)
    


### 12.1.8 三维图形简介

【1】三维数据点与线


```python
from mpl_toolkits import mplot3d # 注意要导入mplot3d

ax = plt.axes(projection="3d")
zline = np.linspace(0, 15, 1000)
xline = np.sin(zline)
yline = np.cos(zline)
ax.plot3D(xline, yline ,zline)# 线的绘制

zdata = 15*np.random.random(100)
xdata = np.sin(zdata)
ydata = np.cos(zdata)
ax.scatter3D(xdata, ydata ,zdata, c=zdata, cmap="spring") # 点的绘制
```




    <mpl_toolkits.mplot3d.art3d.Path3DCollection at 0x2052fd1e5f8>




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_130_1.png)
    


【2】三维数据曲面图


```python
def f(x, y):
    return np.sin(np.sqrt(x**2 + y**2))

x = np.linspace(-6, 6, 30)
y = np.linspace(-6, 6, 30)
X, Y = np.meshgrid(x, y) # 网格化
Z = f(X, Y)

ax = plt.axes(projection="3d")
ax.plot_surface(X, Y, Z, cmap="viridis") # 设置颜色映射
```




    <mpl_toolkits.mplot3d.art3d.Poly3DCollection at 0x20531baa5c0>




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_132_1.png)
    



```python
import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits import mplot3d

t = np.linspace(0, 2*np.pi, 1000)
X = np.sin(t)
Y = np.cos(t)
Z = np.arange(t.size)[:, np.newaxis]

ax = plt.axes(projection="3d")
ax.plot_surface(X, Y, Z, cmap="viridis")
```




    <mpl_toolkits.mplot3d.art3d.Poly3DCollection at 0x1c540cf1cc0>




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_133_1.png)
    


## 12.2 Seaborn库-文艺青年的最爱

【1】Seaborn 与 Matplotlib

Seaborn 是一个基于 matplotlib 且数据结构与 pandas 统一的统计图制作库


```python
x = np.linspace(0, 10, 500)
y = np.cumsum(np.random.randn(500, 6), axis=0)

with plt.style.context("classic"):
    plt.plot(x, y)
    plt.legend("ABCDEF", ncol=2, loc="upper left")   
```


![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_137_0.png)
    



```python
import seaborn as sns

x = np.linspace(0, 10, 500)
y = np.cumsum(np.random.randn(500, 6), axis=0)
sns.set()# 改变了格式
plt.figure(figsize=(10, 6))
plt.plot(x, y)
plt.legend("ABCDEF", ncol=2, loc="upper left")
```




    <matplotlib.legend.Legend at 0x20533d825f8>




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_138_1.png)
    


【2】柱形图的对比


```python
x = ['G1', 'G2', 'G3', 'G4', 'G5']
y = 2 * np.arange(1, 6)

plt.figure(figsize=(8, 4))
plt.barh(x, y, align="center", height=0.5, alpha=0.8, color="blue")
plt.tick_params(axis="both", labelsize=13)
```


![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_140_0.png)
    



```python
import seaborn as sns

plt.figure(figsize=(8, 4))
x = ['G5', 'G4', 'G3', 'G2', 'G1']
y = 2 * np.arange(5, 0, -1)
#sns.barplot(y, x)
sns.barplot(y, x, linewidth=5)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x20533e92048>




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_141_1.png)
    



```python
sns.barplot?
```

【3】以鸢尾花数据集为例


```python
iris = sns.load_dataset("iris")
```


```python
iris.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sepal_length</th>
      <th>sepal_width</th>
      <th>petal_length</th>
      <th>petal_width</th>
      <th>species</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5.1</td>
      <td>3.5</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>setosa</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4.9</td>
      <td>3.0</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>setosa</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4.7</td>
      <td>3.2</td>
      <td>1.3</td>
      <td>0.2</td>
      <td>setosa</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4.6</td>
      <td>3.1</td>
      <td>1.5</td>
      <td>0.2</td>
      <td>setosa</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5.0</td>
      <td>3.6</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>setosa</td>
    </tr>
  </tbody>
</table>
</div>




```python
sns.pairplot(data=iris, hue="species")
```




    <seaborn.axisgrid.PairGrid at 0x205340655f8>




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_146_1.png)
    


## 12.3 Pandas 中的绘图函数概览


```python
import pandas as pd
```

【1】线形图


```python
df = pd.DataFrame(np.random.randn(1000, 4).cumsum(axis=0),
                  columns=list("ABCD"),
                  index=np.arange(1000))
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-1.311443</td>
      <td>0.970917</td>
      <td>-1.635011</td>
      <td>-0.204779</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-1.618502</td>
      <td>0.810056</td>
      <td>-1.119246</td>
      <td>1.239689</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-3.558787</td>
      <td>1.431716</td>
      <td>-0.816201</td>
      <td>1.155611</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-5.377557</td>
      <td>-0.312744</td>
      <td>0.650922</td>
      <td>0.352176</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-3.917045</td>
      <td>1.181097</td>
      <td>1.572406</td>
      <td>0.965921</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.plot()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x20534763f28>




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_151_1.png)
    



```python
df = pd.DataFrame()
df.plot?
```

【2】柱形图


```python
df2 = pd.DataFrame(np.random.rand(10, 4), columns=['a', 'b', 'c', 'd'])
df2
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
      <th>d</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.587600</td>
      <td>0.098736</td>
      <td>0.444757</td>
      <td>0.877475</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.580062</td>
      <td>0.451519</td>
      <td>0.212318</td>
      <td>0.429673</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.415307</td>
      <td>0.784083</td>
      <td>0.891205</td>
      <td>0.756287</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.190053</td>
      <td>0.350987</td>
      <td>0.662549</td>
      <td>0.729193</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.485602</td>
      <td>0.109974</td>
      <td>0.891554</td>
      <td>0.473492</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0.331884</td>
      <td>0.128957</td>
      <td>0.204303</td>
      <td>0.363420</td>
    </tr>
    <tr>
      <th>6</th>
      <td>0.962750</td>
      <td>0.431226</td>
      <td>0.917682</td>
      <td>0.972713</td>
    </tr>
    <tr>
      <th>7</th>
      <td>0.483410</td>
      <td>0.486592</td>
      <td>0.439235</td>
      <td>0.875210</td>
    </tr>
    <tr>
      <th>8</th>
      <td>0.054337</td>
      <td>0.985812</td>
      <td>0.469016</td>
      <td>0.894712</td>
    </tr>
    <tr>
      <th>9</th>
      <td>0.730905</td>
      <td>0.237166</td>
      <td>0.043195</td>
      <td>0.600445</td>
    </tr>
  </tbody>
</table>
</div>



* 多组数据竖图


```python
df2.plot.bar()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x20534f1cb00>




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_156_1.png)
    


* 多组数据累加竖图


```python
df2.plot.bar(stacked=True) # 累加的柱形图
```




    <matplotlib.axes._subplots.AxesSubplot at 0x20534f22208>




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_158_1.png)
    


* 多组数据累加横图


```python
df2.plot.barh(stacked=True) # 变为barh
```




    <matplotlib.axes._subplots.AxesSubplot at 0x2053509d048>




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_160_1.png)
    


【3】直方图和密度图


```python
df4 = pd.DataFrame({"A": np.random.randn(1000) - 3, "B": np.random.randn(1000),
                     "C": np.random.randn(1000) + 3})
df4.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-4.250424</td>
      <td>1.043268</td>
      <td>1.356106</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-2.393362</td>
      <td>-0.891620</td>
      <td>3.787906</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-4.411225</td>
      <td>0.436381</td>
      <td>1.242749</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-3.465659</td>
      <td>-0.845966</td>
      <td>1.540347</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-3.606850</td>
      <td>1.643404</td>
      <td>3.689431</td>
    </tr>
  </tbody>
</table>
</div>



* 普通直方图


```python
df4.plot.hist(bins=50)
```


    <matplotlib.axes._subplots.AxesSubplot at 0x20538383b38>




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_164_1.png)
    


* 累加直方图


```python
df4['A'].plot.hist(cumulative=True)
```


    <matplotlib.axes._subplots.AxesSubplot at 0x2053533bbe0>




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_166_1.png)
    


* 概率密度图


```python
df4['A'].plot(kind="kde")
```


    <matplotlib.axes._subplots.AxesSubplot at 0x205352c4e48>




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_168_1.png)
    


* 差分


```python
df = pd.DataFrame(np.random.randn(1000, 4).cumsum(axis=0),
                  columns=list("ABCD"),
                  index=np.arange(1000))
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-0.277843</td>
      <td>-0.310656</td>
      <td>-0.782999</td>
      <td>-0.049032</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.644248</td>
      <td>-0.505115</td>
      <td>-0.363842</td>
      <td>0.399116</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-0.614141</td>
      <td>-1.227740</td>
      <td>-0.787415</td>
      <td>-0.117485</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-0.055964</td>
      <td>-2.376631</td>
      <td>-0.814320</td>
      <td>-0.716179</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.058613</td>
      <td>-2.355537</td>
      <td>-2.174291</td>
      <td>0.351918</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.diff().hist(bins=50, color="r")
```


    array([[<matplotlib.axes._subplots.AxesSubplot object at 0x000002053942A6A0>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000002053957FE48>],
           [<matplotlib.axes._subplots.AxesSubplot object at 0x00000205395A4780>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x00000205395D4128>]],
          dtype=object)


![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_171_1.png)
    



```python
df = pd.DataFrame()
df.hist?
```

【4】散点图


```python
housing = pd.read_csv("housing.csv")
housing.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>longitude</th>
      <th>latitude</th>
      <th>housing_median_age</th>
      <th>total_rooms</th>
      <th>total_bedrooms</th>
      <th>population</th>
      <th>households</th>
      <th>median_income</th>
      <th>median_house_value</th>
      <th>ocean_proximity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-122.23</td>
      <td>37.88</td>
      <td>41.0</td>
      <td>880.0</td>
      <td>129.0</td>
      <td>322.0</td>
      <td>126.0</td>
      <td>8.3252</td>
      <td>452600.0</td>
      <td>NEAR BAY</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-122.22</td>
      <td>37.86</td>
      <td>21.0</td>
      <td>7099.0</td>
      <td>1106.0</td>
      <td>2401.0</td>
      <td>1138.0</td>
      <td>8.3014</td>
      <td>358500.0</td>
      <td>NEAR BAY</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-122.24</td>
      <td>37.85</td>
      <td>52.0</td>
      <td>1467.0</td>
      <td>190.0</td>
      <td>496.0</td>
      <td>177.0</td>
      <td>7.2574</td>
      <td>352100.0</td>
      <td>NEAR BAY</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-122.25</td>
      <td>37.85</td>
      <td>52.0</td>
      <td>1274.0</td>
      <td>235.0</td>
      <td>558.0</td>
      <td>219.0</td>
      <td>5.6431</td>
      <td>341300.0</td>
      <td>NEAR BAY</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-122.25</td>
      <td>37.85</td>
      <td>52.0</td>
      <td>1627.0</td>
      <td>280.0</td>
      <td>565.0</td>
      <td>259.0</td>
      <td>3.8462</td>
      <td>342200.0</td>
      <td>NEAR BAY</td>
    </tr>
  </tbody>
</table>
</div>




```python
"""基于地理数据的人口、房价可视化"""
# 圆的半价大小代表每个区域人口数量(s),颜色代表价格(c),用预定义的jet表进行可视化
with sns.axes_style("white"):
    housing.plot(kind="scatter", x="longitude", y="latitude", alpha=0.6,
                 s=housing["population"]/100, label="population",
                 c="median_house_value", cmap="jet", colorbar=True, figsize=(12, 8))
plt.legend()
plt.axis([-125, -113.5, 32, 43])
```




    [-125, -113.5, 32, 43]




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_175_1.png)
    



```python
housing.plot(kind="scatter", x="median_income", y="median_house_value", alpha=0.8)
```

    'c' argument looks like a single numeric RGB or RGBA sequence, which should be avoided as value-mapping will have precedence in case its length matches with 'x' & 'y'.  Please use a 2-D array with a single row if you really want to specify the same RGB or RGBA value for all points.





    <matplotlib.axes._subplots.AxesSubplot at 0x2053a45a9b0>




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_176_2.png)
    


【5】多子图


```python
df = pd.DataFrame(np.random.randn(1000, 4).cumsum(axis=0),
                  columns=list("ABCD"),
                  index=np.arange(1000))
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-0.134510</td>
      <td>0.364371</td>
      <td>-0.831193</td>
      <td>-0.796903</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.130102</td>
      <td>1.003402</td>
      <td>-0.622822</td>
      <td>-1.640771</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.066873</td>
      <td>0.126174</td>
      <td>0.180913</td>
      <td>-2.928643</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-1.686890</td>
      <td>-0.050740</td>
      <td>0.312582</td>
      <td>-2.379455</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.655660</td>
      <td>-0.390920</td>
      <td>-1.144121</td>
      <td>-2.625653</td>
    </tr>
  </tbody>
</table>
</div>



* 默认情形


```python
df.plot(subplots=True, figsize=(6, 16))
```




    array([<matplotlib.axes._subplots.AxesSubplot object at 0x0000020539BF46D8>,
           <matplotlib.axes._subplots.AxesSubplot object at 0x0000020539C11898>,
           <matplotlib.axes._subplots.AxesSubplot object at 0x0000020539C3D0B8>,
           <matplotlib.axes._subplots.AxesSubplot object at 0x0000020539C60908>],
          dtype=object)




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_180_1.png)
    


* 设定图形安排


```python
df.plot(subplots=True, layout=(2, 2), figsize=(16, 6), sharex=False)
```




    array([[<matplotlib.axes._subplots.AxesSubplot object at 0x000002053D9C2898>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000002053D9F5668>],
           [<matplotlib.axes._subplots.AxesSubplot object at 0x000002053D68BF98>,
            <matplotlib.axes._subplots.AxesSubplot object at 0x000002053D6B7940>]],
          dtype=object)




![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/output_182_1.png)
    


其他内容请参考Pandas中文文档

https://www.pypandas.cn/docs/user_guide/visualization.html#plot-formatting





[返回首页](https://github.com/timerring/pytorch-for-beginners)
