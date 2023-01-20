- [CH11 - Pandas库](#ch11---pandas库)
  - [引子](#引子)
  - [11.1 对象创建](#111-对象创建)
    - [11.1.1 Pandas Series对象](#1111-pandas-series对象)
    - [11.1.2 Pandas DataFrame对象](#1112-pandas-dataframe对象)
  - [11.2   DataFrame性质](#112---dataframe性质)
  - [11.3 数值运算及统计分析](#113-数值运算及统计分析)
  - [11.4 缺失值处理](#114-缺失值处理)
  - [11.5 合并数据](#115-合并数据)
  - [11.6 分组和数据透视表](#116-分组和数据透视表)
  - [11.7 其他](#117-其他)


# CH11 - Pandas库

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221002210956895.png)

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221002211012978.png)

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221002211020709.png)

## 引子

Numpy 在向量化的数值计算中表现优异

但是在处理更灵活、复杂的数据任务： 

如为数据添加标签、处理缺失值、分组和透视表等方面  

Numpy显得力不从心

**而基于Numpy构建的Pandas库，提供了使得数据分析变得更快更简单的高级数据结构和操作工具**

##   11.1 对象创建

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221002211040984.png)

###  11.1.1 Pandas Series对象

Series 是带标签数据的一维数组

**Series对象的创建**

通用结构: pd.Series(data, index=index, dtype=dtype)

data：数据，可以是列表，字典或Numpy数组

index：索引，为可选参数

dtype: 数据类型，为可选参数

**1、用列表创建**

* index缺省，默认为整数序列


```python
import pandas as pd

data = pd.Series([1.5, 3, 4.5, 6])
data
```


    0    1.5
    1    3.0
    2    4.5
    3    6.0
    dtype: float64



* 增加index


```python
data = pd.Series([1.5, 3, 4.5, 6], index=["a", "b", "c", "d"])
data
```


    a    1.5
    b    3.0
    c    4.5
    d    6.0
    dtype: float64



* 增加数据类型
  
  缺省则从传入的数据自动判断


```python
data = pd.Series([1, 2, 3, 4], index=["a", "b", "c", "d"])    
data
```


    a    1
    b    2
    c    3
    d    4
    dtype: int64




```python
data = pd.Series([1, 2, 3, 4], index=["a", "b", "c", "d"], dtype="float")
data
```


    a    1.0
    b    2.0
    c    3.0
    d    4.0
    dtype: float64



**注意：数据支持多种类型**
* 混合后数据类型变为object


```python
data = pd.Series([1, 2, "3", 4], index=["a", "b", "c", "d"])
data
```


    a    1
    b    2
    c    3
    d    4
    dtype: object




```python
data["a"]
```


    1




```python
data["c"]
```


    '3'



**数据类型可被强制改变**


```python
data = pd.Series([1, 2, "3", 4], index=["a", "b", "c", "d"], dtype=float)
data
```


    a    1.0
    b    2.0
    c    3.0
    d    4.0
    dtype: float64




```python
data["c"]
```


    3.0



不能转为浮点数则会报错


```python
data = pd.Series([1, 2, "a", 4], index=["a", "b", "c", "d"], dtype=float)
data
```


    ---------------------------------------------------------------------------
    
    NameError                                 Traceback (most recent call last)
    
    ~\AppData\Local\Temp/ipykernel_9236/4046912764.py in <module>
    ----> 1 data = pd.Series([1, 2, "a", 4], index=["a", "b", "c", "d"], dtype=float)
          2 data


    NameError: name 'pd' is not defined


**2、用一维numpy数组创建**


```python
import numpy as np

x = np.arange(5)
pd.Series(x)
```


    0    0
    1    1
    2    2
    3    3
    4    4
    dtype: int32



**3、用字典创建**

* 默认以键为index 值为data


```python
population_dict = {"BeiJing": 2154,
                   "ShangHai": 2424,
                   "ShenZhen": 1303,
                   "HangZhou": 981 }
population = pd.Series(population_dict)    
population
```


    BeiJing     2154
    ShangHai    2424
    ShenZhen    1303
    HangZhou     981
    dtype: int64



* 字典创建，如果指定index，则会到字典的键中筛选，找不到的，值设为NaN


```python
population = pd.Series(population_dict, index=["BeiJing", "HangZhou", "c", "d"])    
population
```


    BeiJing     2154.0
    HangZhou     981.0
    c              NaN
    d              NaN
    dtype: float64



**4、data为标量的情况**


```python
pd.Series(5, index=[100, 200, 300])
```


    100    5
    200    5
    300    5
    dtype: int64



### 11.1.2 Pandas DataFrame对象

DataFrame 是带标签数据的多维数组

**DataFrame对象的创建**

通用结构: pd.DataFrame(data, index=index, columns=columns)

data：数据，可以是列表，字典或Numpy数组

index：索引，为可选参数

columns: 列标签，为可选参数

**1、通过Series对象创建**


```python
population_dict = {"BeiJing": 2154,
                   "ShangHai": 2424,
                   "ShenZhen": 1303,
                   "HangZhou": 981 }

population = pd.Series(population_dict)    
pd.DataFrame(population)
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
      <th>0</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>BeiJing</th>
      <td>2154</td>
    </tr>
    <tr>
      <th>ShangHai</th>
      <td>2424</td>
    </tr>
    <tr>
      <th>ShenZhen</th>
      <td>1303</td>
    </tr>
    <tr>
      <th>HangZhou</th>
      <td>981</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.DataFrame(population, columns=["population"])
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
      <th>population</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>BeiJing</th>
      <td>2154</td>
    </tr>
    <tr>
      <th>ShangHai</th>
      <td>2424</td>
    </tr>
    <tr>
      <th>ShenZhen</th>
      <td>1303</td>
    </tr>
    <tr>
      <th>HangZhou</th>
      <td>981</td>
    </tr>
  </tbody>
</table>
</div>



**2、通过Series对象字典创建**


```python
GDP_dict = {"BeiJing": 30320,
            "ShangHai": 32680,
            "ShenZhen": 24222,
            "HangZhou": 13468 }

GDP = pd.Series(GDP_dict)
GDP
```




    BeiJing     30320
    ShangHai    32680
    ShenZhen    24222
    HangZhou    13468
    dtype: int64




```python
pd.DataFrame({"population": population,
              "GDP": GDP})
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
      <th>population</th>
      <th>GDP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>BeiJing</th>
      <td>2154</td>
      <td>30320</td>
    </tr>
    <tr>
      <th>ShangHai</th>
      <td>2424</td>
      <td>32680</td>
    </tr>
    <tr>
      <th>ShenZhen</th>
      <td>1303</td>
      <td>24222</td>
    </tr>
    <tr>
      <th>HangZhou</th>
      <td>981</td>
      <td>13468</td>
    </tr>
  </tbody>
</table>
</div>



**注意：数量不够的会自动补齐**


```python
pd.DataFrame({"population": population,
              "GDP": GDP,
              "country": "China"})
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
      <th>population</th>
      <th>GDP</th>
      <th>country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>BeiJing</th>
      <td>2154</td>
      <td>30320</td>
      <td>China</td>
    </tr>
    <tr>
      <th>ShangHai</th>
      <td>2424</td>
      <td>32680</td>
      <td>China</td>
    </tr>
    <tr>
      <th>ShenZhen</th>
      <td>1303</td>
      <td>24222</td>
      <td>China</td>
    </tr>
    <tr>
      <th>HangZhou</th>
      <td>981</td>
      <td>13468</td>
      <td>China</td>
    </tr>
  </tbody>
</table>
</div>



**3、通过字典列表对象创建**

* 字典索引作为index，字典键作为columns


```python
import numpy as np
import pandas as pd

data = [{"a": i, "b": 2*i} for i in range(3)]
data
```




    [{'a': 0, 'b': 0}, {'a': 1, 'b': 2}, {'a': 2, 'b': 4}]




```python
data = pd.DataFrame(data)
data
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>



行的标签没有排，因此行从0开始，列的标签延续。

* 从中取出一列数据


```python
data1 = data["a"].copy()
data1
```




    0    0
    1    1
    2    2
    Name: a, dtype: int64




```python
data1[0] = 10
data1
```




    0    10
    1     1
    2     2
    Name: a, dtype: int64




```python
data
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>



* 不存在的键，会默认值为NaN


```python
data = [{"a": 1, "b":1},{"b": 3, "c":4}]
data
```




    [{'a': 1, 'b': 1}, {'b': 3, 'c': 4}]




```python
pd.DataFrame(data)
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.0</td>
      <td>1</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NaN</td>
      <td>3</td>
      <td>4.0</td>
    </tr>
  </tbody>
</table>
</div>



**4、通过Numpy二维数组创建**


```python
data = np.random.randint(10, size=(3, 2))
data
```




    array([[1, 6],
           [2, 9],
           [4, 0]])




```python
pd.DataFrame(data, columns=["foo", "bar"], index=["a", "b", "c"])
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
      <th>foo</th>
      <th>bar</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>1</td>
      <td>6</td>
    </tr>
    <tr>
      <th>b</th>
      <td>2</td>
      <td>9</td>
    </tr>
    <tr>
      <th>c</th>
      <td>4</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



## 11.2   DataFrame性质

**1、属性**


```python
data = pd.DataFrame({"pop": population, "GDP": GDP})
data
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
      <th>pop</th>
      <th>GDP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>BeiJing</th>
      <td>2154</td>
      <td>30320</td>
    </tr>
    <tr>
      <th>ShangHai</th>
      <td>2424</td>
      <td>32680</td>
    </tr>
    <tr>
      <th>ShenZhen</th>
      <td>1303</td>
      <td>24222</td>
    </tr>
    <tr>
      <th>HangZhou</th>
      <td>981</td>
      <td>13468</td>
    </tr>
  </tbody>
</table>
</div>



**（1）df.values  返回numpy数组表示的数据**


```python
data.values
```




    array([[ 2154, 30320],
           [ 2424, 32680],
           [ 1303, 24222],
           [  981, 13468]], dtype=int64)



**（2）df.index 返回行索引**


```python
data.index
```




    Index(['BeiJing', 'ShangHai', 'ShenZhen', 'HangZhou'], dtype='object')



**（3）df.columns 返回列索引**


```python
data.columns
```




    Index(['pop', 'GDP'], dtype='object')



**（4）df.shape  形状**


```python
data.shape
```




    (4, 2)



**（5） pd.size 大小**


```python
data.size
```




    8



**（6）pd.dtypes 返回每列数据类型**


```python
data.dtypes
```




    pop    int64
    GDP    int64
    dtype: object



**2、索引**


```python
data
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
      <th>pop</th>
      <th>GDP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>BeiJing</th>
      <td>2154</td>
      <td>30320</td>
    </tr>
    <tr>
      <th>ShangHai</th>
      <td>2424</td>
      <td>32680</td>
    </tr>
    <tr>
      <th>ShenZhen</th>
      <td>1303</td>
      <td>24222</td>
    </tr>
    <tr>
      <th>HangZhou</th>
      <td>981</td>
      <td>13468</td>
    </tr>
  </tbody>
</table>
</div>



**（1）获取列**

* 字典式


```python
data["pop"]
```




    BeiJing     2154
    ShangHai    2424
    ShenZhen    1303
    HangZhou     981
    Name: pop, dtype: int64




```python
data[["GDP", "pop"]]
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
      <th>GDP</th>
      <th>pop</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>BeiJing</th>
      <td>30320</td>
      <td>2154</td>
    </tr>
    <tr>
      <th>ShangHai</th>
      <td>32680</td>
      <td>2424</td>
    </tr>
    <tr>
      <th>ShenZhen</th>
      <td>24222</td>
      <td>1303</td>
    </tr>
    <tr>
      <th>HangZhou</th>
      <td>13468</td>
      <td>981</td>
    </tr>
  </tbody>
</table>
</div>



* 对象属性式


```python
data.GDP
```




    BeiJing     30320
    ShangHai    32680
    ShenZhen    24222
    HangZhou    13468
    Name: GDP, dtype: int64



**（2）获取行**

* 绝对索引 df.loc


```python
data.loc["BeiJing"]
```




    pop     2154
    GDP    30320
    Name: BeiJing, dtype: int64




```python
data.loc[["BeiJing", "HangZhou"]]
```

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>pop</th>
      <th>GDP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>BeiJing</th>
      <td>2154</td>
      <td>30320</td>
    </tr>
    <tr>
      <th>HangZhou</th>
      <td>981</td>
      <td>13468</td>
    </tr>
  </tbody>
</table>
</div>



* 相对索引 df.iloc


```python
data
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
      <th>pop</th>
      <th>GDP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>BeiJing</th>
      <td>2154</td>
      <td>30320</td>
    </tr>
    <tr>
      <th>ShangHai</th>
      <td>2424</td>
      <td>32680</td>
    </tr>
    <tr>
      <th>ShenZhen</th>
      <td>1303</td>
      <td>24222</td>
    </tr>
    <tr>
      <th>HangZhou</th>
      <td>981</td>
      <td>13468</td>
    </tr>
  </tbody>
</table>
</div>




```python
data.iloc[0]
```




    pop     2154
    GDP    30320
    Name: BeiJing, dtype: int64




```python
data.iloc[[1, 3]]
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
      <th>pop</th>
      <th>GDP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>ShangHai</th>
      <td>2424</td>
      <td>32680</td>
    </tr>
    <tr>
      <th>HangZhou</th>
      <td>981</td>
      <td>13468</td>
    </tr>
  </tbody>
</table>
</div>



**（3）获取标量**


```python
data
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
      <th>pop</th>
      <th>GDP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>BeiJing</th>
      <td>2154</td>
      <td>30320</td>
    </tr>
    <tr>
      <th>ShangHai</th>
      <td>2424</td>
      <td>32680</td>
    </tr>
    <tr>
      <th>ShenZhen</th>
      <td>1303</td>
      <td>24222</td>
    </tr>
    <tr>
      <th>HangZhou</th>
      <td>981</td>
      <td>13468</td>
    </tr>
  </tbody>
</table>
</div>




```python
data.loc["BeiJing", "GDP"]
```




    30320




```python
data.iloc[0, 1]
```




    30320




```python
data.values[0][1]
```




    30320



**（4）Series对象的索引**


```python
type(data.GDP)
```




    pandas.core.series.Series




```python
GDP
```




    BeiJing     30320
    ShangHai    32680
    ShenZhen    24222
    HangZhou    13468
    dtype: int64




```python
GDP["BeiJing"]
```




    30320



**3、切片**


```python
dates = pd.date_range(start='2019-01-01', periods=6)
dates
```




    DatetimeIndex(['2019-01-01', '2019-01-02', '2019-01-03', '2019-01-04',
                   '2019-01-05', '2019-01-06'],
                  dtype='datetime64[ns]', freq='D')




```python
df = pd.DataFrame(np.random.randn(6,4), index=dates, columns=["A", "B", "C", "D"])
df
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
      <th>2019-01-01</th>
      <td>-0.935378</td>
      <td>-0.190742</td>
      <td>0.925984</td>
      <td>-0.818969</td>
    </tr>
    <tr>
      <th>2019-01-02</th>
      <td>-0.234414</td>
      <td>-1.194674</td>
      <td>1.080779</td>
      <td>-2.294395</td>
    </tr>
    <tr>
      <th>2019-01-03</th>
      <td>-0.141572</td>
      <td>0.058118</td>
      <td>1.102248</td>
      <td>1.207726</td>
    </tr>
    <tr>
      <th>2019-01-04</th>
      <td>0.305088</td>
      <td>0.535920</td>
      <td>-0.978434</td>
      <td>0.177251</td>
    </tr>
    <tr>
      <th>2019-01-05</th>
      <td>0.313383</td>
      <td>0.234041</td>
      <td>0.163155</td>
      <td>-0.296649</td>
    </tr>
    <tr>
      <th>2019-01-06</th>
      <td>0.250613</td>
      <td>-0.904400</td>
      <td>-0.858240</td>
      <td>-1.573342</td>
    </tr>
  </tbody>
</table>
</div>



**（1）行切片**


```python
df["2019-01-01": "2019-01-03"]
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
      <th>2019-01-01</th>
      <td>-0.935378</td>
      <td>-0.190742</td>
      <td>0.925984</td>
      <td>-0.818969</td>
    </tr>
    <tr>
      <th>2019-01-02</th>
      <td>-0.234414</td>
      <td>-1.194674</td>
      <td>1.080779</td>
      <td>-2.294395</td>
    </tr>
    <tr>
      <th>2019-01-03</th>
      <td>-0.141572</td>
      <td>0.058118</td>
      <td>1.102248</td>
      <td>1.207726</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.loc["2019-01-01": "2019-01-03"]
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
      <th>2019-01-01</th>
      <td>-0.935378</td>
      <td>-0.190742</td>
      <td>0.925984</td>
      <td>-0.818969</td>
    </tr>
    <tr>
      <th>2019-01-02</th>
      <td>-0.234414</td>
      <td>-1.194674</td>
      <td>1.080779</td>
      <td>-2.294395</td>
    </tr>
    <tr>
      <th>2019-01-03</th>
      <td>-0.141572</td>
      <td>0.058118</td>
      <td>1.102248</td>
      <td>1.207726</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.iloc[0: 3]
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
      <th>2019-01-01</th>
      <td>-0.935378</td>
      <td>-0.190742</td>
      <td>0.925984</td>
      <td>-0.818969</td>
    </tr>
    <tr>
      <th>2019-01-02</th>
      <td>-0.234414</td>
      <td>-1.194674</td>
      <td>1.080779</td>
      <td>-2.294395</td>
    </tr>
    <tr>
      <th>2019-01-03</th>
      <td>-0.141572</td>
      <td>0.058118</td>
      <td>1.102248</td>
      <td>1.207726</td>
    </tr>
  </tbody>
</table>
</div>



注意：这里的3是取不到的。

**（2）列切片**


```python
df
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
      <th>2019-01-01</th>
      <td>-0.935378</td>
      <td>-0.190742</td>
      <td>0.925984</td>
      <td>-0.818969</td>
    </tr>
    <tr>
      <th>2019-01-02</th>
      <td>-0.234414</td>
      <td>-1.194674</td>
      <td>1.080779</td>
      <td>-2.294395</td>
    </tr>
    <tr>
      <th>2019-01-03</th>
      <td>-0.141572</td>
      <td>0.058118</td>
      <td>1.102248</td>
      <td>1.207726</td>
    </tr>
    <tr>
      <th>2019-01-04</th>
      <td>0.305088</td>
      <td>0.535920</td>
      <td>-0.978434</td>
      <td>0.177251</td>
    </tr>
    <tr>
      <th>2019-01-05</th>
      <td>0.313383</td>
      <td>0.234041</td>
      <td>0.163155</td>
      <td>-0.296649</td>
    </tr>
    <tr>
      <th>2019-01-06</th>
      <td>0.250613</td>
      <td>-0.904400</td>
      <td>-0.858240</td>
      <td>-1.573342</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.loc[:, "A": "C"]
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
      <th>2019-01-01</th>
      <td>-0.935378</td>
      <td>-0.190742</td>
      <td>0.925984</td>
    </tr>
    <tr>
      <th>2019-01-02</th>
      <td>-0.234414</td>
      <td>-1.194674</td>
      <td>1.080779</td>
    </tr>
    <tr>
      <th>2019-01-03</th>
      <td>-0.141572</td>
      <td>0.058118</td>
      <td>1.102248</td>
    </tr>
    <tr>
      <th>2019-01-04</th>
      <td>0.305088</td>
      <td>0.535920</td>
      <td>-0.978434</td>
    </tr>
    <tr>
      <th>2019-01-05</th>
      <td>0.313383</td>
      <td>0.234041</td>
      <td>0.163155</td>
    </tr>
    <tr>
      <th>2019-01-06</th>
      <td>0.250613</td>
      <td>-0.904400</td>
      <td>-0.858240</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.iloc[:, 0: 3]
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
      <th>2019-01-01</th>
      <td>-0.935378</td>
      <td>-0.190742</td>
      <td>0.925984</td>
    </tr>
    <tr>
      <th>2019-01-02</th>
      <td>-0.234414</td>
      <td>-1.194674</td>
      <td>1.080779</td>
    </tr>
    <tr>
      <th>2019-01-03</th>
      <td>-0.141572</td>
      <td>0.058118</td>
      <td>1.102248</td>
    </tr>
    <tr>
      <th>2019-01-04</th>
      <td>0.305088</td>
      <td>0.535920</td>
      <td>-0.978434</td>
    </tr>
    <tr>
      <th>2019-01-05</th>
      <td>0.313383</td>
      <td>0.234041</td>
      <td>0.163155</td>
    </tr>
    <tr>
      <th>2019-01-06</th>
      <td>0.250613</td>
      <td>-0.904400</td>
      <td>-0.858240</td>
    </tr>
  </tbody>
</table>
</div>



**（3）多种多样的取值**


```python
df
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
      <th>2019-01-01</th>
      <td>-0.935378</td>
      <td>-0.190742</td>
      <td>0.925984</td>
      <td>-0.818969</td>
    </tr>
    <tr>
      <th>2019-01-02</th>
      <td>-0.234414</td>
      <td>-1.194674</td>
      <td>1.080779</td>
      <td>-2.294395</td>
    </tr>
    <tr>
      <th>2019-01-03</th>
      <td>-0.141572</td>
      <td>0.058118</td>
      <td>1.102248</td>
      <td>1.207726</td>
    </tr>
    <tr>
      <th>2019-01-04</th>
      <td>0.305088</td>
      <td>0.535920</td>
      <td>-0.978434</td>
      <td>0.177251</td>
    </tr>
    <tr>
      <th>2019-01-05</th>
      <td>0.313383</td>
      <td>0.234041</td>
      <td>0.163155</td>
      <td>-0.296649</td>
    </tr>
    <tr>
      <th>2019-01-06</th>
      <td>0.250613</td>
      <td>-0.904400</td>
      <td>-0.858240</td>
      <td>-1.573342</td>
    </tr>
  </tbody>
</table>
</div>



* 行、列同时切片


```python
df.loc["2019-01-02": "2019-01-03", "C":"D"]
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
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-01-02</th>
      <td>1.080779</td>
      <td>-2.294395</td>
    </tr>
    <tr>
      <th>2019-01-03</th>
      <td>1.102248</td>
      <td>1.207726</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.iloc[1: 3, 2:]
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
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-01-02</th>
      <td>1.080779</td>
      <td>-2.294395</td>
    </tr>
    <tr>
      <th>2019-01-03</th>
      <td>1.102248</td>
      <td>1.207726</td>
    </tr>
  </tbody>
</table>
</div>



* 行切片，列分散取值


```python
df.loc["2019-01-04": "2019-01-06", ["A", "C"]]
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
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-01-04</th>
      <td>0.305088</td>
      <td>-0.978434</td>
    </tr>
    <tr>
      <th>2019-01-05</th>
      <td>0.313383</td>
      <td>0.163155</td>
    </tr>
    <tr>
      <th>2019-01-06</th>
      <td>0.250613</td>
      <td>-0.858240</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.iloc[3:, [0, 2]]
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
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-01-04</th>
      <td>0.305088</td>
      <td>-0.978434</td>
    </tr>
    <tr>
      <th>2019-01-05</th>
      <td>0.313383</td>
      <td>0.163155</td>
    </tr>
    <tr>
      <th>2019-01-06</th>
      <td>0.250613</td>
      <td>-0.858240</td>
    </tr>
  </tbody>
</table>
</div>



* 行分散取值，列切片


```python
df.loc[["2019-01-02", "2019-01-06"], "C": "D"]
```

上面这种方式是行不通的。


```python
df.iloc[[1, 5], 0: 3]
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
      <th>2019-01-02</th>
      <td>-0.234414</td>
      <td>-1.194674</td>
      <td>1.080779</td>
    </tr>
    <tr>
      <th>2019-01-06</th>
      <td>0.250613</td>
      <td>-0.904400</td>
      <td>-0.858240</td>
    </tr>
  </tbody>
</table>
</div>



* 行、列均分散取值


```python
df.loc[["2019-01-04", "2019-01-06"], ["A", "D"]]
```

同样，上面这种方式是行不通的。


```python
df.iloc[[1, 5], [0, 3]]
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
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-01-02</th>
      <td>-0.234414</td>
      <td>-2.294395</td>
    </tr>
    <tr>
      <th>2019-01-06</th>
      <td>0.250613</td>
      <td>-1.573342</td>
    </tr>
  </tbody>
</table>
</div>



**4、布尔索引**

相当于numpy当中的掩码操作。


```python
df
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
      <th>2019-01-01</th>
      <td>-0.935378</td>
      <td>-0.190742</td>
      <td>0.925984</td>
      <td>-0.818969</td>
    </tr>
    <tr>
      <th>2019-01-02</th>
      <td>-0.234414</td>
      <td>-1.194674</td>
      <td>1.080779</td>
      <td>-2.294395</td>
    </tr>
    <tr>
      <th>2019-01-03</th>
      <td>-0.141572</td>
      <td>0.058118</td>
      <td>1.102248</td>
      <td>1.207726</td>
    </tr>
    <tr>
      <th>2019-01-04</th>
      <td>0.305088</td>
      <td>0.535920</td>
      <td>-0.978434</td>
      <td>0.177251</td>
    </tr>
    <tr>
      <th>2019-01-05</th>
      <td>0.313383</td>
      <td>0.234041</td>
      <td>0.163155</td>
      <td>-0.296649</td>
    </tr>
    <tr>
      <th>2019-01-06</th>
      <td>0.250613</td>
      <td>-0.904400</td>
      <td>-0.858240</td>
      <td>-1.573342</td>
    </tr>
  </tbody>
</table>
</div>




```python
df > 0
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
      <th>2019-01-01</th>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2019-01-02</th>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2019-01-03</th>
      <td>False</td>
      <td>True</td>
      <td>True</td>
      <td>True</td>
    </tr>
    <tr>
      <th>2019-01-04</th>
      <td>True</td>
      <td>True</td>
      <td>False</td>
      <td>True</td>
    </tr>
    <tr>
      <th>2019-01-05</th>
      <td>True</td>
      <td>True</td>
      <td>True</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2019-01-06</th>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>




```python
df[df > 0]
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
      <th>2019-01-01</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.925984</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2019-01-02</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.080779</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2019-01-03</th>
      <td>NaN</td>
      <td>0.058118</td>
      <td>1.102248</td>
      <td>1.207726</td>
    </tr>
    <tr>
      <th>2019-01-04</th>
      <td>0.305088</td>
      <td>0.535920</td>
      <td>NaN</td>
      <td>0.177251</td>
    </tr>
    <tr>
      <th>2019-01-05</th>
      <td>0.313383</td>
      <td>0.234041</td>
      <td>0.163155</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2019-01-06</th>
      <td>0.250613</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



可以观察到，为true的部分都被取到了，而false没有。


```python
df.A > 0
```




    2019-01-01    False
    2019-01-02    False
    2019-01-03    False
    2019-01-04     True
    2019-01-05     True
    2019-01-06     True
    Freq: D, Name: A, dtype: bool




```python
df[df.A > 0]
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
      <th>2019-01-04</th>
      <td>0.305088</td>
      <td>0.535920</td>
      <td>-0.978434</td>
      <td>0.177251</td>
    </tr>
    <tr>
      <th>2019-01-05</th>
      <td>0.313383</td>
      <td>0.234041</td>
      <td>0.163155</td>
      <td>-0.296649</td>
    </tr>
    <tr>
      <th>2019-01-06</th>
      <td>0.250613</td>
      <td>-0.904400</td>
      <td>-0.858240</td>
      <td>-1.573342</td>
    </tr>
  </tbody>
</table>
</div>



* isin（）方法


```python
df2 = df.copy()
df2['E'] = ['one', 'one', 'two', 'three', 'four', 'three']
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>E</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-01-01</th>
      <td>-0.935378</td>
      <td>-0.190742</td>
      <td>0.925984</td>
      <td>-0.818969</td>
      <td>one</td>
    </tr>
    <tr>
      <th>2019-01-02</th>
      <td>-0.234414</td>
      <td>-1.194674</td>
      <td>1.080779</td>
      <td>-2.294395</td>
      <td>one</td>
    </tr>
    <tr>
      <th>2019-01-03</th>
      <td>-0.141572</td>
      <td>0.058118</td>
      <td>1.102248</td>
      <td>1.207726</td>
      <td>two</td>
    </tr>
    <tr>
      <th>2019-01-04</th>
      <td>0.305088</td>
      <td>0.535920</td>
      <td>-0.978434</td>
      <td>0.177251</td>
      <td>three</td>
    </tr>
    <tr>
      <th>2019-01-05</th>
      <td>0.313383</td>
      <td>0.234041</td>
      <td>0.163155</td>
      <td>-0.296649</td>
      <td>four</td>
    </tr>
    <tr>
      <th>2019-01-06</th>
      <td>0.250613</td>
      <td>-0.904400</td>
      <td>-0.858240</td>
      <td>-1.573342</td>
      <td>three</td>
    </tr>
  </tbody>
</table>
</div>




```python
ind = df2["E"].isin(["two", "four"])
ind     
```




    2019-01-01    False
    2019-01-02    False
    2019-01-03     True
    2019-01-04    False
    2019-01-05     True
    2019-01-06    False
    Freq: D, Name: E, dtype: bool




```python
df2[ind]
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
      <th>E</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-01-03</th>
      <td>-0.141572</td>
      <td>0.058118</td>
      <td>1.102248</td>
      <td>1.207726</td>
      <td>two</td>
    </tr>
    <tr>
      <th>2019-01-05</th>
      <td>0.313383</td>
      <td>0.234041</td>
      <td>0.163155</td>
      <td>-0.296649</td>
      <td>four</td>
    </tr>
  </tbody>
</table>
</div>



**（5）赋值**


```python
df
```

* DataFrame 增加新列


```python
s1 = pd.Series([1, 2, 3, 4, 5, 6], index=pd.date_range('20190101', periods=6))
s1
```




    2019-01-01    1
    2019-01-02    2
    2019-01-03    3
    2019-01-04    4
    2019-01-05    5
    2019-01-06    6
    Freq: D, dtype: int64




```python
df["E"] = s1
df
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
      <th>E</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-01-01</th>
      <td>-0.935378</td>
      <td>-0.190742</td>
      <td>0.925984</td>
      <td>-0.818969</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2019-01-02</th>
      <td>-0.234414</td>
      <td>-1.194674</td>
      <td>1.080779</td>
      <td>-2.294395</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2019-01-03</th>
      <td>-0.141572</td>
      <td>0.058118</td>
      <td>1.102248</td>
      <td>1.207726</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2019-01-04</th>
      <td>0.305088</td>
      <td>0.535920</td>
      <td>-0.978434</td>
      <td>0.177251</td>
      <td>4</td>
    </tr>
    <tr>
      <th>2019-01-05</th>
      <td>0.313383</td>
      <td>0.234041</td>
      <td>0.163155</td>
      <td>-0.296649</td>
      <td>5</td>
    </tr>
    <tr>
      <th>2019-01-06</th>
      <td>0.250613</td>
      <td>-0.904400</td>
      <td>-0.858240</td>
      <td>-1.573342</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>



* 修改赋值


```python
df.loc["2019-01-01", "A"] = 0
df
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
      <th>E</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-01-01</th>
      <td>0.000000</td>
      <td>-0.190742</td>
      <td>0.925984</td>
      <td>-0.818969</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2019-01-02</th>
      <td>-0.234414</td>
      <td>-1.194674</td>
      <td>1.080779</td>
      <td>-2.294395</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2019-01-03</th>
      <td>-0.141572</td>
      <td>0.058118</td>
      <td>1.102248</td>
      <td>1.207726</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2019-01-04</th>
      <td>0.305088</td>
      <td>0.535920</td>
      <td>-0.978434</td>
      <td>0.177251</td>
      <td>4</td>
    </tr>
    <tr>
      <th>2019-01-05</th>
      <td>0.313383</td>
      <td>0.234041</td>
      <td>0.163155</td>
      <td>-0.296649</td>
      <td>5</td>
    </tr>
    <tr>
      <th>2019-01-06</th>
      <td>0.250613</td>
      <td>-0.904400</td>
      <td>-0.858240</td>
      <td>-1.573342</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.iloc[0, 1] = 0
df
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
      <th>E</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-01-01</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.925984</td>
      <td>-0.818969</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2019-01-02</th>
      <td>-0.234414</td>
      <td>-1.194674</td>
      <td>1.080779</td>
      <td>-2.294395</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2019-01-03</th>
      <td>-0.141572</td>
      <td>0.058118</td>
      <td>1.102248</td>
      <td>1.207726</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2019-01-04</th>
      <td>0.305088</td>
      <td>0.535920</td>
      <td>-0.978434</td>
      <td>0.177251</td>
      <td>4</td>
    </tr>
    <tr>
      <th>2019-01-05</th>
      <td>0.313383</td>
      <td>0.234041</td>
      <td>0.163155</td>
      <td>-0.296649</td>
      <td>5</td>
    </tr>
    <tr>
      <th>2019-01-06</th>
      <td>0.250613</td>
      <td>-0.904400</td>
      <td>-0.858240</td>
      <td>-1.573342</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>




```python
df["D"] = np.array([5]*len(df))   # 可简化成df["D"] = 5
df
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
      <th>E</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-01-01</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.925984</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2019-01-02</th>
      <td>-0.234414</td>
      <td>-1.194674</td>
      <td>1.080779</td>
      <td>5</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2019-01-03</th>
      <td>-0.141572</td>
      <td>0.058118</td>
      <td>1.102248</td>
      <td>5</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2019-01-04</th>
      <td>0.305088</td>
      <td>0.535920</td>
      <td>-0.978434</td>
      <td>5</td>
      <td>4</td>
    </tr>
    <tr>
      <th>2019-01-05</th>
      <td>0.313383</td>
      <td>0.234041</td>
      <td>0.163155</td>
      <td>5</td>
      <td>5</td>
    </tr>
    <tr>
      <th>2019-01-06</th>
      <td>0.250613</td>
      <td>-0.904400</td>
      <td>-0.858240</td>
      <td>5</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>



* 修改index和columns


```python
df.index = [i for i in range(len(df))]
df
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
      <th>E</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.925984</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-0.234414</td>
      <td>-1.194674</td>
      <td>1.080779</td>
      <td>5</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-0.141572</td>
      <td>0.058118</td>
      <td>1.102248</td>
      <td>5</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.305088</td>
      <td>0.535920</td>
      <td>-0.978434</td>
      <td>5</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.313383</td>
      <td>0.234041</td>
      <td>0.163155</td>
      <td>5</td>
      <td>5</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0.250613</td>
      <td>-0.904400</td>
      <td>-0.858240</td>
      <td>5</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.columns = [i for i in range(df.shape[1])]
df
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
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.925984</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-0.234414</td>
      <td>-1.194674</td>
      <td>1.080779</td>
      <td>5</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-0.141572</td>
      <td>0.058118</td>
      <td>1.102248</td>
      <td>5</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.305088</td>
      <td>0.535920</td>
      <td>-0.978434</td>
      <td>5</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.313383</td>
      <td>0.234041</td>
      <td>0.163155</td>
      <td>5</td>
      <td>5</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0.250613</td>
      <td>-0.904400</td>
      <td>-0.858240</td>
      <td>5</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>



## 11.3 数值运算及统计分析

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221002211052367.png)

**1、数据的查看**


```python
import pandas as pd
import numpy as np

dates = pd.date_range(start='2019-01-01', periods=6)
df = pd.DataFrame(np.random.randn(6,4), index=dates, columns=["A", "B", "C", "D"])
df
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
      <th>2019-01-01</th>
      <td>-0.854043</td>
      <td>0.412345</td>
      <td>-2.296051</td>
      <td>-0.048964</td>
    </tr>
    <tr>
      <th>2019-01-02</th>
      <td>1.371364</td>
      <td>-0.121454</td>
      <td>-0.299653</td>
      <td>1.095375</td>
    </tr>
    <tr>
      <th>2019-01-03</th>
      <td>-0.714591</td>
      <td>-1.103224</td>
      <td>0.979250</td>
      <td>0.319455</td>
    </tr>
    <tr>
      <th>2019-01-04</th>
      <td>-1.397557</td>
      <td>0.426008</td>
      <td>0.233861</td>
      <td>-1.651887</td>
    </tr>
    <tr>
      <th>2019-01-05</th>
      <td>0.434026</td>
      <td>0.459830</td>
      <td>-0.095444</td>
      <td>1.220302</td>
    </tr>
    <tr>
      <th>2019-01-06</th>
      <td>-0.133876</td>
      <td>0.074500</td>
      <td>-1.028147</td>
      <td>0.605402</td>
    </tr>
  </tbody>
</table>
</div>



**（1）查看前面的行**


```python
df.head()    # 默认5行，也可以进行设置
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
      <th>2019-01-01</th>
      <td>-0.854043</td>
      <td>0.412345</td>
      <td>-2.296051</td>
      <td>-0.048964</td>
    </tr>
    <tr>
      <th>2019-01-02</th>
      <td>1.371364</td>
      <td>-0.121454</td>
      <td>-0.299653</td>
      <td>1.095375</td>
    </tr>
    <tr>
      <th>2019-01-03</th>
      <td>-0.714591</td>
      <td>-1.103224</td>
      <td>0.979250</td>
      <td>0.319455</td>
    </tr>
    <tr>
      <th>2019-01-04</th>
      <td>-1.397557</td>
      <td>0.426008</td>
      <td>0.233861</td>
      <td>-1.651887</td>
    </tr>
    <tr>
      <th>2019-01-05</th>
      <td>0.434026</td>
      <td>0.459830</td>
      <td>-0.095444</td>
      <td>1.220302</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.head(2)
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
      <th>2019-01-01</th>
      <td>-0.854043</td>
      <td>0.412345</td>
      <td>-2.296051</td>
      <td>-0.048964</td>
    </tr>
    <tr>
      <th>2019-01-02</th>
      <td>1.371364</td>
      <td>-0.121454</td>
      <td>-0.299653</td>
      <td>1.095375</td>
    </tr>
  </tbody>
</table>
</div>



**（2）查看后面的行**


```python
df.tail()    # 默认5行
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
      <th>2019-01-02</th>
      <td>1.371364</td>
      <td>-0.121454</td>
      <td>-0.299653</td>
      <td>1.095375</td>
    </tr>
    <tr>
      <th>2019-01-03</th>
      <td>-0.714591</td>
      <td>-1.103224</td>
      <td>0.979250</td>
      <td>0.319455</td>
    </tr>
    <tr>
      <th>2019-01-04</th>
      <td>-1.397557</td>
      <td>0.426008</td>
      <td>0.233861</td>
      <td>-1.651887</td>
    </tr>
    <tr>
      <th>2019-01-05</th>
      <td>0.434026</td>
      <td>0.459830</td>
      <td>-0.095444</td>
      <td>1.220302</td>
    </tr>
    <tr>
      <th>2019-01-06</th>
      <td>-0.133876</td>
      <td>0.074500</td>
      <td>-1.028147</td>
      <td>0.605402</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.tail(3) 
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
      <th>2019-01-04</th>
      <td>-1.397557</td>
      <td>0.426008</td>
      <td>0.233861</td>
      <td>-1.651887</td>
    </tr>
    <tr>
      <th>2019-01-05</th>
      <td>0.434026</td>
      <td>0.459830</td>
      <td>-0.095444</td>
      <td>1.220302</td>
    </tr>
    <tr>
      <th>2019-01-06</th>
      <td>-0.133876</td>
      <td>0.074500</td>
      <td>-1.028147</td>
      <td>0.605402</td>
    </tr>
  </tbody>
</table>
</div>



**（3）查看总体信息**


```python
df.iloc[0, 3] = np.nan
df
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
      <th>2019-01-01</th>
      <td>-0.854043</td>
      <td>0.412345</td>
      <td>-2.296051</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2019-01-02</th>
      <td>1.371364</td>
      <td>-0.121454</td>
      <td>-0.299653</td>
      <td>1.095375</td>
    </tr>
    <tr>
      <th>2019-01-03</th>
      <td>-0.714591</td>
      <td>-1.103224</td>
      <td>0.979250</td>
      <td>0.319455</td>
    </tr>
    <tr>
      <th>2019-01-04</th>
      <td>-1.397557</td>
      <td>0.426008</td>
      <td>0.233861</td>
      <td>-1.651887</td>
    </tr>
    <tr>
      <th>2019-01-05</th>
      <td>0.434026</td>
      <td>0.459830</td>
      <td>-0.095444</td>
      <td>1.220302</td>
    </tr>
    <tr>
      <th>2019-01-06</th>
      <td>-0.133876</td>
      <td>0.074500</td>
      <td>-1.028147</td>
      <td>0.605402</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    DatetimeIndex: 6 entries, 2019-01-01 to 2019-01-06
    Freq: D
    Data columns (total 4 columns):
    A    6 non-null float64
    B    6 non-null float64
    C    6 non-null float64
    D    5 non-null float64
    dtypes: float64(4)
    memory usage: 240.0 bytes


**2、Numpy通用函数同样适用于Pandas**

**（1）向量化运算**


```python
x = pd.DataFrame(np.arange(4).reshape(1, 4))
x
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
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>




```python
x+5
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
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5</td>
      <td>6</td>
      <td>7</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
np.exp(x)
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
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.0</td>
      <td>2.718282</td>
      <td>7.389056</td>
      <td>20.085537</td>
    </tr>
  </tbody>
</table>
</div>




```python
y = pd.DataFrame(np.arange(4,8).reshape(1, 4))
y
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
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>4</td>
      <td>5</td>
      <td>6</td>
      <td>7</td>
    </tr>
  </tbody>
</table>
</div>




```python
x*y
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
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>5</td>
      <td>12</td>
      <td>21</td>
    </tr>
  </tbody>
</table>
</div>



**（2）矩阵化运算**


```python
np.random.seed(42)
x = pd.DataFrame(np.random.randint(10, size=(30, 30)))
x
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
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
      <th>7</th>
      <th>8</th>
      <th>9</th>
      <th>...</th>
      <th>20</th>
      <th>21</th>
      <th>22</th>
      <th>23</th>
      <th>24</th>
      <th>25</th>
      <th>26</th>
      <th>27</th>
      <th>28</th>
      <th>29</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>6</td>
      <td>3</td>
      <td>7</td>
      <td>4</td>
      <td>6</td>
      <td>9</td>
      <td>2</td>
      <td>6</td>
      <td>7</td>
      <td>4</td>
      <td>...</td>
      <td>4</td>
      <td>0</td>
      <td>9</td>
      <td>5</td>
      <td>8</td>
      <td>0</td>
      <td>9</td>
      <td>2</td>
      <td>6</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>8</td>
      <td>2</td>
      <td>4</td>
      <td>2</td>
      <td>6</td>
      <td>4</td>
      <td>8</td>
      <td>6</td>
      <td>1</td>
      <td>3</td>
      <td>...</td>
      <td>2</td>
      <td>0</td>
      <td>3</td>
      <td>1</td>
      <td>7</td>
      <td>3</td>
      <td>1</td>
      <td>5</td>
      <td>5</td>
      <td>9</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>5</td>
      <td>1</td>
      <td>9</td>
      <td>1</td>
      <td>9</td>
      <td>3</td>
      <td>7</td>
      <td>6</td>
      <td>8</td>
      <td>...</td>
      <td>6</td>
      <td>8</td>
      <td>7</td>
      <td>0</td>
      <td>7</td>
      <td>7</td>
      <td>2</td>
      <td>0</td>
      <td>7</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>0</td>
      <td>4</td>
      <td>9</td>
      <td>6</td>
      <td>9</td>
      <td>8</td>
      <td>6</td>
      <td>8</td>
      <td>7</td>
      <td>...</td>
      <td>0</td>
      <td>2</td>
      <td>4</td>
      <td>2</td>
      <td>0</td>
      <td>4</td>
      <td>9</td>
      <td>6</td>
      <td>6</td>
      <td>8</td>
    </tr>
    <tr>
      <th>4</th>
      <td>9</td>
      <td>9</td>
      <td>2</td>
      <td>6</td>
      <td>0</td>
      <td>3</td>
      <td>3</td>
      <td>4</td>
      <td>6</td>
      <td>6</td>
      <td>...</td>
      <td>9</td>
      <td>6</td>
      <td>8</td>
      <td>6</td>
      <td>0</td>
      <td>0</td>
      <td>8</td>
      <td>8</td>
      <td>3</td>
      <td>8</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2</td>
      <td>6</td>
      <td>5</td>
      <td>7</td>
      <td>8</td>
      <td>4</td>
      <td>0</td>
      <td>2</td>
      <td>9</td>
      <td>7</td>
      <td>...</td>
      <td>2</td>
      <td>0</td>
      <td>4</td>
      <td>0</td>
      <td>7</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6</td>
      <td>4</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
      <td>4</td>
      <td>9</td>
      <td>5</td>
      <td>6</td>
      <td>...</td>
      <td>5</td>
      <td>0</td>
      <td>8</td>
      <td>5</td>
      <td>2</td>
      <td>3</td>
      <td>3</td>
      <td>2</td>
      <td>9</td>
      <td>2</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2</td>
      <td>3</td>
      <td>6</td>
      <td>3</td>
      <td>8</td>
      <td>0</td>
      <td>7</td>
      <td>6</td>
      <td>1</td>
      <td>7</td>
      <td>...</td>
      <td>3</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>4</td>
      <td>4</td>
      <td>6</td>
      <td>8</td>
      <td>8</td>
      <td>2</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2</td>
      <td>2</td>
      <td>3</td>
      <td>7</td>
      <td>5</td>
      <td>7</td>
      <td>0</td>
      <td>7</td>
      <td>3</td>
      <td>0</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>2</td>
      <td>8</td>
      <td>3</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>9</th>
      <td>3</td>
      <td>7</td>
      <td>7</td>
      <td>6</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>5</td>
      <td>6</td>
      <td>...</td>
      <td>4</td>
      <td>2</td>
      <td>3</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>4</td>
      <td>5</td>
      <td>2</td>
      <td>8</td>
    </tr>
    <tr>
      <th>10</th>
      <td>4</td>
      <td>7</td>
      <td>0</td>
      <td>4</td>
      <td>2</td>
      <td>0</td>
      <td>3</td>
      <td>4</td>
      <td>6</td>
      <td>0</td>
      <td>...</td>
      <td>5</td>
      <td>6</td>
      <td>1</td>
      <td>9</td>
      <td>1</td>
      <td>9</td>
      <td>0</td>
      <td>7</td>
      <td>0</td>
      <td>8</td>
    </tr>
    <tr>
      <th>11</th>
      <td>5</td>
      <td>6</td>
      <td>9</td>
      <td>6</td>
      <td>9</td>
      <td>2</td>
      <td>1</td>
      <td>8</td>
      <td>7</td>
      <td>9</td>
      <td>...</td>
      <td>6</td>
      <td>5</td>
      <td>2</td>
      <td>8</td>
      <td>9</td>
      <td>5</td>
      <td>9</td>
      <td>9</td>
      <td>5</td>
      <td>0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>3</td>
      <td>9</td>
      <td>5</td>
      <td>5</td>
      <td>4</td>
      <td>0</td>
      <td>7</td>
      <td>4</td>
      <td>4</td>
      <td>6</td>
      <td>...</td>
      <td>0</td>
      <td>7</td>
      <td>2</td>
      <td>9</td>
      <td>6</td>
      <td>9</td>
      <td>4</td>
      <td>9</td>
      <td>4</td>
      <td>6</td>
    </tr>
    <tr>
      <th>13</th>
      <td>8</td>
      <td>4</td>
      <td>0</td>
      <td>9</td>
      <td>9</td>
      <td>0</td>
      <td>1</td>
      <td>5</td>
      <td>8</td>
      <td>7</td>
      <td>...</td>
      <td>5</td>
      <td>8</td>
      <td>4</td>
      <td>0</td>
      <td>3</td>
      <td>4</td>
      <td>9</td>
      <td>9</td>
      <td>4</td>
      <td>6</td>
    </tr>
    <tr>
      <th>14</th>
      <td>3</td>
      <td>0</td>
      <td>4</td>
      <td>6</td>
      <td>9</td>
      <td>9</td>
      <td>5</td>
      <td>4</td>
      <td>3</td>
      <td>1</td>
      <td>...</td>
      <td>6</td>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>7</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>15</th>
      <td>4</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>7</td>
      <td>9</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>...</td>
      <td>6</td>
      <td>3</td>
      <td>9</td>
      <td>4</td>
      <td>1</td>
      <td>7</td>
      <td>3</td>
      <td>8</td>
      <td>4</td>
      <td>8</td>
    </tr>
    <tr>
      <th>16</th>
      <td>3</td>
      <td>9</td>
      <td>4</td>
      <td>8</td>
      <td>7</td>
      <td>2</td>
      <td>0</td>
      <td>2</td>
      <td>3</td>
      <td>1</td>
      <td>...</td>
      <td>8</td>
      <td>0</td>
      <td>0</td>
      <td>3</td>
      <td>8</td>
      <td>5</td>
      <td>2</td>
      <td>0</td>
      <td>3</td>
      <td>8</td>
    </tr>
    <tr>
      <th>17</th>
      <td>2</td>
      <td>8</td>
      <td>6</td>
      <td>3</td>
      <td>2</td>
      <td>9</td>
      <td>4</td>
      <td>4</td>
      <td>2</td>
      <td>8</td>
      <td>...</td>
      <td>6</td>
      <td>9</td>
      <td>4</td>
      <td>2</td>
      <td>6</td>
      <td>1</td>
      <td>8</td>
      <td>9</td>
      <td>9</td>
      <td>0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>5</td>
      <td>6</td>
      <td>7</td>
      <td>9</td>
      <td>8</td>
      <td>1</td>
      <td>9</td>
      <td>1</td>
      <td>4</td>
      <td>4</td>
      <td>...</td>
      <td>3</td>
      <td>5</td>
      <td>2</td>
      <td>5</td>
      <td>6</td>
      <td>9</td>
      <td>9</td>
      <td>2</td>
      <td>6</td>
      <td>2</td>
    </tr>
    <tr>
      <th>19</th>
      <td>1</td>
      <td>9</td>
      <td>3</td>
      <td>7</td>
      <td>8</td>
      <td>6</td>
      <td>0</td>
      <td>2</td>
      <td>8</td>
      <td>0</td>
      <td>...</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>3</td>
      <td>8</td>
      <td>1</td>
      <td>8</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>20</th>
      <td>4</td>
      <td>5</td>
      <td>5</td>
      <td>2</td>
      <td>6</td>
      <td>8</td>
      <td>9</td>
      <td>7</td>
      <td>5</td>
      <td>7</td>
      <td>...</td>
      <td>3</td>
      <td>5</td>
      <td>0</td>
      <td>8</td>
      <td>0</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>21</th>
      <td>2</td>
      <td>4</td>
      <td>8</td>
      <td>1</td>
      <td>9</td>
      <td>7</td>
      <td>1</td>
      <td>4</td>
      <td>6</td>
      <td>7</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>8</td>
      <td>2</td>
      <td>0</td>
      <td>4</td>
      <td>6</td>
      <td>5</td>
      <td>0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>22</th>
      <td>4</td>
      <td>5</td>
      <td>2</td>
      <td>4</td>
      <td>6</td>
      <td>4</td>
      <td>4</td>
      <td>4</td>
      <td>9</td>
      <td>9</td>
      <td>...</td>
      <td>1</td>
      <td>7</td>
      <td>6</td>
      <td>9</td>
      <td>9</td>
      <td>1</td>
      <td>5</td>
      <td>5</td>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>23</th>
      <td>0</td>
      <td>5</td>
      <td>4</td>
      <td>8</td>
      <td>0</td>
      <td>6</td>
      <td>4</td>
      <td>4</td>
      <td>1</td>
      <td>2</td>
      <td>...</td>
      <td>8</td>
      <td>5</td>
      <td>0</td>
      <td>7</td>
      <td>6</td>
      <td>9</td>
      <td>2</td>
      <td>0</td>
      <td>4</td>
      <td>3</td>
    </tr>
    <tr>
      <th>24</th>
      <td>9</td>
      <td>7</td>
      <td>0</td>
      <td>9</td>
      <td>0</td>
      <td>3</td>
      <td>7</td>
      <td>4</td>
      <td>1</td>
      <td>5</td>
      <td>...</td>
      <td>3</td>
      <td>7</td>
      <td>8</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>9</td>
      <td>2</td>
      <td>2</td>
      <td>4</td>
    </tr>
    <tr>
      <th>25</th>
      <td>4</td>
      <td>1</td>
      <td>9</td>
      <td>5</td>
      <td>4</td>
      <td>5</td>
      <td>0</td>
      <td>4</td>
      <td>8</td>
      <td>9</td>
      <td>...</td>
      <td>9</td>
      <td>3</td>
      <td>0</td>
      <td>7</td>
      <td>0</td>
      <td>2</td>
      <td>3</td>
      <td>7</td>
      <td>5</td>
      <td>9</td>
    </tr>
    <tr>
      <th>26</th>
      <td>6</td>
      <td>7</td>
      <td>1</td>
      <td>9</td>
      <td>7</td>
      <td>2</td>
      <td>6</td>
      <td>2</td>
      <td>6</td>
      <td>1</td>
      <td>...</td>
      <td>0</td>
      <td>6</td>
      <td>5</td>
      <td>9</td>
      <td>8</td>
      <td>0</td>
      <td>3</td>
      <td>8</td>
      <td>3</td>
      <td>9</td>
    </tr>
    <tr>
      <th>27</th>
      <td>2</td>
      <td>8</td>
      <td>1</td>
      <td>3</td>
      <td>5</td>
      <td>1</td>
      <td>7</td>
      <td>7</td>
      <td>0</td>
      <td>2</td>
      <td>...</td>
      <td>8</td>
      <td>0</td>
      <td>4</td>
      <td>5</td>
      <td>4</td>
      <td>5</td>
      <td>5</td>
      <td>6</td>
      <td>3</td>
      <td>7</td>
    </tr>
    <tr>
      <th>28</th>
      <td>6</td>
      <td>8</td>
      <td>6</td>
      <td>2</td>
      <td>2</td>
      <td>7</td>
      <td>4</td>
      <td>3</td>
      <td>7</td>
      <td>5</td>
      <td>...</td>
      <td>1</td>
      <td>7</td>
      <td>9</td>
      <td>2</td>
      <td>4</td>
      <td>5</td>
      <td>9</td>
      <td>5</td>
      <td>3</td>
      <td>2</td>
    </tr>
    <tr>
      <th>29</th>
      <td>3</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>9</td>
      <td>5</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
      <td>...</td>
      <td>1</td>
      <td>3</td>
      <td>0</td>
      <td>4</td>
      <td>8</td>
      <td>0</td>
      <td>8</td>
      <td>7</td>
      <td>5</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
<p>30 rows × 30 columns</p>
</div>



* 转置


```python
z = x.T
z
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
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
      <th>7</th>
      <th>8</th>
      <th>9</th>
      <th>...</th>
      <th>20</th>
      <th>21</th>
      <th>22</th>
      <th>23</th>
      <th>24</th>
      <th>25</th>
      <th>26</th>
      <th>27</th>
      <th>28</th>
      <th>29</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>6</td>
      <td>8</td>
      <td>3</td>
      <td>2</td>
      <td>9</td>
      <td>2</td>
      <td>6</td>
      <td>2</td>
      <td>2</td>
      <td>3</td>
      <td>...</td>
      <td>4</td>
      <td>2</td>
      <td>4</td>
      <td>0</td>
      <td>9</td>
      <td>4</td>
      <td>6</td>
      <td>2</td>
      <td>6</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3</td>
      <td>2</td>
      <td>5</td>
      <td>0</td>
      <td>9</td>
      <td>6</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
      <td>7</td>
      <td>...</td>
      <td>5</td>
      <td>4</td>
      <td>5</td>
      <td>5</td>
      <td>7</td>
      <td>1</td>
      <td>7</td>
      <td>8</td>
      <td>8</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>7</td>
      <td>4</td>
      <td>1</td>
      <td>4</td>
      <td>2</td>
      <td>5</td>
      <td>0</td>
      <td>6</td>
      <td>3</td>
      <td>7</td>
      <td>...</td>
      <td>5</td>
      <td>8</td>
      <td>2</td>
      <td>4</td>
      <td>0</td>
      <td>9</td>
      <td>1</td>
      <td>1</td>
      <td>6</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>2</td>
      <td>9</td>
      <td>9</td>
      <td>6</td>
      <td>7</td>
      <td>0</td>
      <td>3</td>
      <td>7</td>
      <td>6</td>
      <td>...</td>
      <td>2</td>
      <td>1</td>
      <td>4</td>
      <td>8</td>
      <td>9</td>
      <td>5</td>
      <td>9</td>
      <td>3</td>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>6</td>
      <td>6</td>
      <td>1</td>
      <td>6</td>
      <td>0</td>
      <td>8</td>
      <td>2</td>
      <td>8</td>
      <td>5</td>
      <td>2</td>
      <td>...</td>
      <td>6</td>
      <td>9</td>
      <td>6</td>
      <td>0</td>
      <td>0</td>
      <td>4</td>
      <td>7</td>
      <td>5</td>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>9</td>
      <td>4</td>
      <td>9</td>
      <td>9</td>
      <td>3</td>
      <td>4</td>
      <td>1</td>
      <td>0</td>
      <td>7</td>
      <td>0</td>
      <td>...</td>
      <td>8</td>
      <td>7</td>
      <td>4</td>
      <td>6</td>
      <td>3</td>
      <td>5</td>
      <td>2</td>
      <td>1</td>
      <td>7</td>
      <td>9</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2</td>
      <td>8</td>
      <td>3</td>
      <td>8</td>
      <td>3</td>
      <td>0</td>
      <td>4</td>
      <td>7</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>9</td>
      <td>1</td>
      <td>4</td>
      <td>4</td>
      <td>7</td>
      <td>0</td>
      <td>6</td>
      <td>7</td>
      <td>4</td>
      <td>5</td>
    </tr>
    <tr>
      <th>7</th>
      <td>6</td>
      <td>6</td>
      <td>7</td>
      <td>6</td>
      <td>4</td>
      <td>2</td>
      <td>9</td>
      <td>6</td>
      <td>7</td>
      <td>2</td>
      <td>...</td>
      <td>7</td>
      <td>4</td>
      <td>4</td>
      <td>4</td>
      <td>4</td>
      <td>4</td>
      <td>2</td>
      <td>7</td>
      <td>3</td>
      <td>4</td>
    </tr>
    <tr>
      <th>8</th>
      <td>7</td>
      <td>1</td>
      <td>6</td>
      <td>8</td>
      <td>6</td>
      <td>9</td>
      <td>5</td>
      <td>1</td>
      <td>3</td>
      <td>5</td>
      <td>...</td>
      <td>5</td>
      <td>6</td>
      <td>9</td>
      <td>1</td>
      <td>1</td>
      <td>8</td>
      <td>6</td>
      <td>0</td>
      <td>7</td>
      <td>3</td>
    </tr>
    <tr>
      <th>9</th>
      <td>4</td>
      <td>3</td>
      <td>8</td>
      <td>7</td>
      <td>6</td>
      <td>7</td>
      <td>6</td>
      <td>7</td>
      <td>0</td>
      <td>6</td>
      <td>...</td>
      <td>7</td>
      <td>7</td>
      <td>9</td>
      <td>2</td>
      <td>5</td>
      <td>9</td>
      <td>1</td>
      <td>2</td>
      <td>5</td>
      <td>2</td>
    </tr>
    <tr>
      <th>10</th>
      <td>3</td>
      <td>8</td>
      <td>7</td>
      <td>1</td>
      <td>3</td>
      <td>5</td>
      <td>3</td>
      <td>0</td>
      <td>7</td>
      <td>5</td>
      <td>...</td>
      <td>4</td>
      <td>0</td>
      <td>2</td>
      <td>6</td>
      <td>4</td>
      <td>1</td>
      <td>9</td>
      <td>9</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>7</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>6</td>
      <td>7</td>
      <td>6</td>
      <td>8</td>
      <td>3</td>
      <td>5</td>
      <td>...</td>
      <td>7</td>
      <td>5</td>
      <td>0</td>
      <td>5</td>
      <td>1</td>
      <td>0</td>
      <td>5</td>
      <td>8</td>
      <td>3</td>
      <td>5</td>
    </tr>
    <tr>
      <th>12</th>
      <td>7</td>
      <td>9</td>
      <td>1</td>
      <td>6</td>
      <td>2</td>
      <td>8</td>
      <td>7</td>
      <td>8</td>
      <td>5</td>
      <td>5</td>
      <td>...</td>
      <td>9</td>
      <td>0</td>
      <td>4</td>
      <td>1</td>
      <td>2</td>
      <td>9</td>
      <td>2</td>
      <td>4</td>
      <td>3</td>
      <td>1</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2</td>
      <td>8</td>
      <td>4</td>
      <td>6</td>
      <td>5</td>
      <td>3</td>
      <td>0</td>
      <td>1</td>
      <td>7</td>
      <td>2</td>
      <td>...</td>
      <td>3</td>
      <td>1</td>
      <td>8</td>
      <td>5</td>
      <td>8</td>
      <td>8</td>
      <td>2</td>
      <td>5</td>
      <td>5</td>
      <td>7</td>
    </tr>
    <tr>
      <th>14</th>
      <td>5</td>
      <td>9</td>
      <td>7</td>
      <td>7</td>
      <td>1</td>
      <td>0</td>
      <td>5</td>
      <td>6</td>
      <td>3</td>
      <td>5</td>
      <td>...</td>
      <td>9</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>6</td>
      <td>9</td>
      <td>8</td>
      <td>3</td>
      <td>5</td>
      <td>9</td>
    </tr>
    <tr>
      <th>15</th>
      <td>4</td>
      <td>4</td>
      <td>9</td>
      <td>4</td>
      <td>9</td>
      <td>0</td>
      <td>7</td>
      <td>9</td>
      <td>2</td>
      <td>7</td>
      <td>...</td>
      <td>7</td>
      <td>4</td>
      <td>2</td>
      <td>1</td>
      <td>6</td>
      <td>8</td>
      <td>6</td>
      <td>9</td>
      <td>0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>16</th>
      <td>1</td>
      <td>1</td>
      <td>8</td>
      <td>2</td>
      <td>8</td>
      <td>9</td>
      <td>4</td>
      <td>2</td>
      <td>8</td>
      <td>1</td>
      <td>...</td>
      <td>9</td>
      <td>9</td>
      <td>3</td>
      <td>1</td>
      <td>5</td>
      <td>8</td>
      <td>4</td>
      <td>1</td>
      <td>7</td>
      <td>6</td>
    </tr>
    <tr>
      <th>17</th>
      <td>7</td>
      <td>3</td>
      <td>8</td>
      <td>7</td>
      <td>4</td>
      <td>3</td>
      <td>3</td>
      <td>6</td>
      <td>2</td>
      <td>4</td>
      <td>...</td>
      <td>1</td>
      <td>8</td>
      <td>0</td>
      <td>2</td>
      <td>7</td>
      <td>5</td>
      <td>9</td>
      <td>7</td>
      <td>5</td>
      <td>9</td>
    </tr>
    <tr>
      <th>18</th>
      <td>5</td>
      <td>6</td>
      <td>0</td>
      <td>5</td>
      <td>5</td>
      <td>6</td>
      <td>1</td>
      <td>9</td>
      <td>8</td>
      <td>0</td>
      <td>...</td>
      <td>4</td>
      <td>5</td>
      <td>0</td>
      <td>1</td>
      <td>3</td>
      <td>7</td>
      <td>6</td>
      <td>5</td>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>19</th>
      <td>1</td>
      <td>7</td>
      <td>8</td>
      <td>2</td>
      <td>3</td>
      <td>1</td>
      <td>5</td>
      <td>8</td>
      <td>1</td>
      <td>0</td>
      <td>...</td>
      <td>8</td>
      <td>0</td>
      <td>7</td>
      <td>3</td>
      <td>7</td>
      <td>0</td>
      <td>8</td>
      <td>4</td>
      <td>8</td>
      <td>7</td>
    </tr>
    <tr>
      <th>20</th>
      <td>4</td>
      <td>2</td>
      <td>6</td>
      <td>0</td>
      <td>9</td>
      <td>2</td>
      <td>5</td>
      <td>3</td>
      <td>1</td>
      <td>4</td>
      <td>...</td>
      <td>3</td>
      <td>0</td>
      <td>1</td>
      <td>8</td>
      <td>3</td>
      <td>9</td>
      <td>0</td>
      <td>8</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>21</th>
      <td>0</td>
      <td>0</td>
      <td>8</td>
      <td>2</td>
      <td>6</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>...</td>
      <td>5</td>
      <td>1</td>
      <td>7</td>
      <td>5</td>
      <td>7</td>
      <td>3</td>
      <td>6</td>
      <td>0</td>
      <td>7</td>
      <td>3</td>
    </tr>
    <tr>
      <th>22</th>
      <td>9</td>
      <td>3</td>
      <td>7</td>
      <td>4</td>
      <td>8</td>
      <td>4</td>
      <td>8</td>
      <td>1</td>
      <td>5</td>
      <td>3</td>
      <td>...</td>
      <td>0</td>
      <td>8</td>
      <td>6</td>
      <td>0</td>
      <td>8</td>
      <td>0</td>
      <td>5</td>
      <td>4</td>
      <td>9</td>
      <td>0</td>
    </tr>
    <tr>
      <th>23</th>
      <td>5</td>
      <td>1</td>
      <td>0</td>
      <td>2</td>
      <td>6</td>
      <td>0</td>
      <td>5</td>
      <td>0</td>
      <td>2</td>
      <td>2</td>
      <td>...</td>
      <td>8</td>
      <td>2</td>
      <td>9</td>
      <td>7</td>
      <td>2</td>
      <td>7</td>
      <td>9</td>
      <td>5</td>
      <td>2</td>
      <td>4</td>
    </tr>
    <tr>
      <th>24</th>
      <td>8</td>
      <td>7</td>
      <td>7</td>
      <td>0</td>
      <td>0</td>
      <td>7</td>
      <td>2</td>
      <td>4</td>
      <td>8</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>9</td>
      <td>6</td>
      <td>2</td>
      <td>0</td>
      <td>8</td>
      <td>4</td>
      <td>4</td>
      <td>8</td>
    </tr>
    <tr>
      <th>25</th>
      <td>0</td>
      <td>3</td>
      <td>7</td>
      <td>4</td>
      <td>0</td>
      <td>0</td>
      <td>3</td>
      <td>4</td>
      <td>3</td>
      <td>0</td>
      <td>...</td>
      <td>4</td>
      <td>4</td>
      <td>1</td>
      <td>9</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>5</td>
      <td>5</td>
      <td>0</td>
    </tr>
    <tr>
      <th>26</th>
      <td>9</td>
      <td>1</td>
      <td>2</td>
      <td>9</td>
      <td>8</td>
      <td>0</td>
      <td>3</td>
      <td>6</td>
      <td>0</td>
      <td>4</td>
      <td>...</td>
      <td>3</td>
      <td>6</td>
      <td>5</td>
      <td>2</td>
      <td>9</td>
      <td>3</td>
      <td>3</td>
      <td>5</td>
      <td>9</td>
      <td>8</td>
    </tr>
    <tr>
      <th>27</th>
      <td>2</td>
      <td>5</td>
      <td>0</td>
      <td>6</td>
      <td>8</td>
      <td>1</td>
      <td>2</td>
      <td>8</td>
      <td>3</td>
      <td>5</td>
      <td>...</td>
      <td>2</td>
      <td>5</td>
      <td>5</td>
      <td>0</td>
      <td>2</td>
      <td>7</td>
      <td>8</td>
      <td>6</td>
      <td>5</td>
      <td>7</td>
    </tr>
    <tr>
      <th>28</th>
      <td>6</td>
      <td>5</td>
      <td>7</td>
      <td>6</td>
      <td>3</td>
      <td>1</td>
      <td>9</td>
      <td>8</td>
      <td>0</td>
      <td>2</td>
      <td>...</td>
      <td>5</td>
      <td>0</td>
      <td>2</td>
      <td>4</td>
      <td>2</td>
      <td>5</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>5</td>
    </tr>
    <tr>
      <th>29</th>
      <td>3</td>
      <td>9</td>
      <td>2</td>
      <td>8</td>
      <td>8</td>
      <td>5</td>
      <td>2</td>
      <td>2</td>
      <td>4</td>
      <td>8</td>
      <td>...</td>
      <td>1</td>
      <td>4</td>
      <td>1</td>
      <td>3</td>
      <td>4</td>
      <td>9</td>
      <td>9</td>
      <td>7</td>
      <td>2</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
<p>30 rows × 30 columns</p>
</div>




```python
np.random.seed(1)
y = pd.DataFrame(np.random.randint(10, size=(30, 30)))
y
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
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
      <th>7</th>
      <th>8</th>
      <th>9</th>
      <th>...</th>
      <th>20</th>
      <th>21</th>
      <th>22</th>
      <th>23</th>
      <th>24</th>
      <th>25</th>
      <th>26</th>
      <th>27</th>
      <th>28</th>
      <th>29</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5</td>
      <td>8</td>
      <td>9</td>
      <td>5</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>7</td>
      <td>6</td>
      <td>9</td>
      <td>...</td>
      <td>1</td>
      <td>7</td>
      <td>0</td>
      <td>6</td>
      <td>9</td>
      <td>9</td>
      <td>7</td>
      <td>6</td>
      <td>9</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>1</td>
      <td>8</td>
      <td>8</td>
      <td>3</td>
      <td>9</td>
      <td>8</td>
      <td>7</td>
      <td>3</td>
      <td>6</td>
      <td>...</td>
      <td>9</td>
      <td>2</td>
      <td>0</td>
      <td>4</td>
      <td>9</td>
      <td>2</td>
      <td>7</td>
      <td>7</td>
      <td>9</td>
      <td>8</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6</td>
      <td>9</td>
      <td>3</td>
      <td>7</td>
      <td>7</td>
      <td>4</td>
      <td>5</td>
      <td>9</td>
      <td>3</td>
      <td>6</td>
      <td>...</td>
      <td>7</td>
      <td>7</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>0</td>
      <td>8</td>
      <td>6</td>
      <td>4</td>
      <td>5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>6</td>
      <td>2</td>
      <td>5</td>
      <td>7</td>
      <td>8</td>
      <td>4</td>
      <td>4</td>
      <td>7</td>
      <td>7</td>
      <td>4</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>9</td>
      <td>8</td>
      <td>2</td>
      <td>3</td>
      <td>1</td>
      <td>2</td>
      <td>7</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>6</td>
      <td>0</td>
      <td>9</td>
      <td>2</td>
      <td>6</td>
      <td>6</td>
      <td>2</td>
      <td>7</td>
      <td>7</td>
      <td>0</td>
      <td>...</td>
      <td>1</td>
      <td>5</td>
      <td>4</td>
      <td>0</td>
      <td>7</td>
      <td>8</td>
      <td>9</td>
      <td>5</td>
      <td>7</td>
      <td>0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>9</td>
      <td>3</td>
      <td>9</td>
      <td>1</td>
      <td>4</td>
      <td>4</td>
      <td>6</td>
      <td>8</td>
      <td>8</td>
      <td>9</td>
      <td>...</td>
      <td>1</td>
      <td>8</td>
      <td>7</td>
      <td>0</td>
      <td>3</td>
      <td>4</td>
      <td>2</td>
      <td>0</td>
      <td>3</td>
      <td>5</td>
    </tr>
    <tr>
      <th>6</th>
      <td>1</td>
      <td>2</td>
      <td>4</td>
      <td>3</td>
      <td>0</td>
      <td>6</td>
      <td>0</td>
      <td>7</td>
      <td>2</td>
      <td>8</td>
      <td>...</td>
      <td>4</td>
      <td>3</td>
      <td>3</td>
      <td>6</td>
      <td>7</td>
      <td>3</td>
      <td>5</td>
      <td>3</td>
      <td>2</td>
      <td>4</td>
    </tr>
    <tr>
      <th>7</th>
      <td>4</td>
      <td>0</td>
      <td>3</td>
      <td>3</td>
      <td>8</td>
      <td>3</td>
      <td>5</td>
      <td>6</td>
      <td>7</td>
      <td>5</td>
      <td>...</td>
      <td>1</td>
      <td>7</td>
      <td>3</td>
      <td>1</td>
      <td>6</td>
      <td>6</td>
      <td>9</td>
      <td>6</td>
      <td>9</td>
      <td>6</td>
    </tr>
    <tr>
      <th>8</th>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>9</td>
      <td>6</td>
      <td>0</td>
      <td>6</td>
      <td>7</td>
      <td>0</td>
      <td>3</td>
      <td>...</td>
      <td>6</td>
      <td>7</td>
      <td>9</td>
      <td>5</td>
      <td>4</td>
      <td>9</td>
      <td>5</td>
      <td>2</td>
      <td>5</td>
      <td>6</td>
    </tr>
    <tr>
      <th>9</th>
      <td>6</td>
      <td>8</td>
      <td>7</td>
      <td>7</td>
      <td>7</td>
      <td>2</td>
      <td>6</td>
      <td>0</td>
      <td>5</td>
      <td>2</td>
      <td>...</td>
      <td>7</td>
      <td>0</td>
      <td>6</td>
      <td>2</td>
      <td>4</td>
      <td>3</td>
      <td>6</td>
      <td>7</td>
      <td>6</td>
      <td>3</td>
    </tr>
    <tr>
      <th>10</th>
      <td>0</td>
      <td>6</td>
      <td>4</td>
      <td>7</td>
      <td>6</td>
      <td>2</td>
      <td>9</td>
      <td>5</td>
      <td>9</td>
      <td>9</td>
      <td>...</td>
      <td>4</td>
      <td>9</td>
      <td>3</td>
      <td>9</td>
      <td>1</td>
      <td>2</td>
      <td>5</td>
      <td>4</td>
      <td>0</td>
      <td>8</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2</td>
      <td>3</td>
      <td>9</td>
      <td>9</td>
      <td>4</td>
      <td>4</td>
      <td>8</td>
      <td>2</td>
      <td>1</td>
      <td>6</td>
      <td>...</td>
      <td>0</td>
      <td>5</td>
      <td>9</td>
      <td>8</td>
      <td>6</td>
      <td>6</td>
      <td>0</td>
      <td>4</td>
      <td>7</td>
      <td>3</td>
    </tr>
    <tr>
      <th>12</th>
      <td>0</td>
      <td>1</td>
      <td>6</td>
      <td>0</td>
      <td>6</td>
      <td>1</td>
      <td>6</td>
      <td>4</td>
      <td>2</td>
      <td>5</td>
      <td>...</td>
      <td>8</td>
      <td>8</td>
      <td>0</td>
      <td>7</td>
      <td>2</td>
      <td>0</td>
      <td>7</td>
      <td>1</td>
      <td>1</td>
      <td>9</td>
    </tr>
    <tr>
      <th>13</th>
      <td>5</td>
      <td>1</td>
      <td>5</td>
      <td>9</td>
      <td>6</td>
      <td>4</td>
      <td>9</td>
      <td>8</td>
      <td>7</td>
      <td>5</td>
      <td>...</td>
      <td>2</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>4</td>
      <td>2</td>
      <td>5</td>
      <td>0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>0</td>
      <td>3</td>
      <td>8</td>
      <td>5</td>
      <td>3</td>
      <td>1</td>
      <td>4</td>
      <td>7</td>
      <td>3</td>
      <td>2</td>
      <td>...</td>
      <td>8</td>
      <td>5</td>
      <td>5</td>
      <td>7</td>
      <td>5</td>
      <td>9</td>
      <td>1</td>
      <td>3</td>
      <td>9</td>
      <td>3</td>
    </tr>
    <tr>
      <th>15</th>
      <td>3</td>
      <td>3</td>
      <td>6</td>
      <td>1</td>
      <td>3</td>
      <td>0</td>
      <td>5</td>
      <td>0</td>
      <td>5</td>
      <td>2</td>
      <td>...</td>
      <td>7</td>
      <td>1</td>
      <td>7</td>
      <td>7</td>
      <td>3</td>
      <td>8</td>
      <td>3</td>
      <td>0</td>
      <td>6</td>
      <td>3</td>
    </tr>
    <tr>
      <th>16</th>
      <td>0</td>
      <td>6</td>
      <td>5</td>
      <td>9</td>
      <td>6</td>
      <td>4</td>
      <td>6</td>
      <td>6</td>
      <td>2</td>
      <td>2</td>
      <td>...</td>
      <td>3</td>
      <td>6</td>
      <td>8</td>
      <td>6</td>
      <td>5</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>6</td>
      <td>3</td>
    </tr>
    <tr>
      <th>17</th>
      <td>6</td>
      <td>7</td>
      <td>2</td>
      <td>8</td>
      <td>0</td>
      <td>1</td>
      <td>8</td>
      <td>6</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>5</td>
      <td>6</td>
      <td>2</td>
      <td>5</td>
      <td>4</td>
      <td>3</td>
      <td>0</td>
      <td>6</td>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>18</th>
      <td>9</td>
      <td>4</td>
      <td>4</td>
      <td>0</td>
      <td>9</td>
      <td>8</td>
      <td>7</td>
      <td>7</td>
      <td>6</td>
      <td>1</td>
      <td>...</td>
      <td>7</td>
      <td>9</td>
      <td>9</td>
      <td>7</td>
      <td>1</td>
      <td>1</td>
      <td>4</td>
      <td>6</td>
      <td>5</td>
      <td>6</td>
    </tr>
    <tr>
      <th>19</th>
      <td>4</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>1</td>
      <td>2</td>
      <td>6</td>
      <td>2</td>
      <td>3</td>
      <td>3</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>9</td>
      <td>8</td>
      <td>5</td>
      <td>9</td>
      <td>3</td>
      <td>4</td>
      <td>0</td>
    </tr>
    <tr>
      <th>20</th>
      <td>9</td>
      <td>8</td>
      <td>6</td>
      <td>3</td>
      <td>9</td>
      <td>9</td>
      <td>0</td>
      <td>8</td>
      <td>1</td>
      <td>6</td>
      <td>...</td>
      <td>2</td>
      <td>9</td>
      <td>0</td>
      <td>1</td>
      <td>3</td>
      <td>9</td>
      <td>4</td>
      <td>8</td>
      <td>8</td>
      <td>8</td>
    </tr>
    <tr>
      <th>21</th>
      <td>2</td>
      <td>8</td>
      <td>6</td>
      <td>4</td>
      <td>9</td>
      <td>0</td>
      <td>5</td>
      <td>5</td>
      <td>6</td>
      <td>1</td>
      <td>...</td>
      <td>6</td>
      <td>7</td>
      <td>5</td>
      <td>6</td>
      <td>8</td>
      <td>7</td>
      <td>4</td>
      <td>2</td>
      <td>4</td>
      <td>0</td>
    </tr>
    <tr>
      <th>22</th>
      <td>0</td>
      <td>3</td>
      <td>5</td>
      <td>9</td>
      <td>0</td>
      <td>3</td>
      <td>6</td>
      <td>5</td>
      <td>1</td>
      <td>1</td>
      <td>...</td>
      <td>6</td>
      <td>2</td>
      <td>5</td>
      <td>3</td>
      <td>9</td>
      <td>3</td>
      <td>9</td>
      <td>5</td>
      <td>1</td>
      <td>9</td>
    </tr>
    <tr>
      <th>23</th>
      <td>7</td>
      <td>7</td>
      <td>0</td>
      <td>8</td>
      <td>6</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>4</td>
      <td>4</td>
      <td>...</td>
      <td>1</td>
      <td>9</td>
      <td>6</td>
      <td>0</td>
      <td>2</td>
      <td>8</td>
      <td>3</td>
      <td>7</td>
      <td>2</td>
      <td>5</td>
    </tr>
    <tr>
      <th>24</th>
      <td>6</td>
      <td>0</td>
      <td>4</td>
      <td>2</td>
      <td>3</td>
      <td>1</td>
      <td>0</td>
      <td>5</td>
      <td>7</td>
      <td>0</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>7</td>
      <td>5</td>
      <td>2</td>
      <td>9</td>
      <td>4</td>
      <td>7</td>
      <td>3</td>
    </tr>
    <tr>
      <th>25</th>
      <td>5</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
      <td>4</td>
      <td>9</td>
      <td>4</td>
      <td>6</td>
      <td>9</td>
      <td>3</td>
      <td>...</td>
      <td>5</td>
      <td>5</td>
      <td>3</td>
      <td>5</td>
      <td>9</td>
      <td>2</td>
      <td>7</td>
      <td>4</td>
      <td>1</td>
      <td>6</td>
    </tr>
    <tr>
      <th>26</th>
      <td>9</td>
      <td>8</td>
      <td>1</td>
      <td>8</td>
      <td>1</td>
      <td>6</td>
      <td>2</td>
      <td>6</td>
      <td>1</td>
      <td>8</td>
      <td>...</td>
      <td>2</td>
      <td>5</td>
      <td>1</td>
      <td>2</td>
      <td>5</td>
      <td>3</td>
      <td>3</td>
      <td>6</td>
      <td>1</td>
      <td>8</td>
    </tr>
    <tr>
      <th>27</th>
      <td>1</td>
      <td>8</td>
      <td>6</td>
      <td>4</td>
      <td>6</td>
      <td>9</td>
      <td>5</td>
      <td>4</td>
      <td>7</td>
      <td>2</td>
      <td>...</td>
      <td>9</td>
      <td>3</td>
      <td>1</td>
      <td>5</td>
      <td>1</td>
      <td>1</td>
      <td>7</td>
      <td>1</td>
      <td>2</td>
      <td>6</td>
    </tr>
    <tr>
      <th>28</th>
      <td>0</td>
      <td>7</td>
      <td>7</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
      <td>7</td>
      <td>8</td>
      <td>5</td>
      <td>2</td>
      <td>...</td>
      <td>0</td>
      <td>2</td>
      <td>8</td>
      <td>3</td>
      <td>7</td>
      <td>3</td>
      <td>9</td>
      <td>2</td>
      <td>3</td>
      <td>8</td>
    </tr>
    <tr>
      <th>29</th>
      <td>8</td>
      <td>0</td>
      <td>2</td>
      <td>6</td>
      <td>8</td>
      <td>3</td>
      <td>6</td>
      <td>4</td>
      <td>9</td>
      <td>7</td>
      <td>...</td>
      <td>6</td>
      <td>7</td>
      <td>8</td>
      <td>5</td>
      <td>7</td>
      <td>2</td>
      <td>5</td>
      <td>3</td>
      <td>4</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
<p>30 rows × 30 columns</p>
</div>




```python
x.dot(y)
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
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
      <th>7</th>
      <th>8</th>
      <th>9</th>
      <th>...</th>
      <th>20</th>
      <th>21</th>
      <th>22</th>
      <th>23</th>
      <th>24</th>
      <th>25</th>
      <th>26</th>
      <th>27</th>
      <th>28</th>
      <th>29</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>616</td>
      <td>560</td>
      <td>723</td>
      <td>739</td>
      <td>612</td>
      <td>457</td>
      <td>681</td>
      <td>799</td>
      <td>575</td>
      <td>590</td>
      <td>...</td>
      <td>523</td>
      <td>739</td>
      <td>613</td>
      <td>580</td>
      <td>668</td>
      <td>602</td>
      <td>733</td>
      <td>585</td>
      <td>657</td>
      <td>700</td>
    </tr>
    <tr>
      <th>1</th>
      <td>520</td>
      <td>438</td>
      <td>691</td>
      <td>600</td>
      <td>612</td>
      <td>455</td>
      <td>666</td>
      <td>764</td>
      <td>707</td>
      <td>592</td>
      <td>...</td>
      <td>555</td>
      <td>681</td>
      <td>503</td>
      <td>679</td>
      <td>641</td>
      <td>506</td>
      <td>779</td>
      <td>494</td>
      <td>633</td>
      <td>590</td>
    </tr>
    <tr>
      <th>2</th>
      <td>557</td>
      <td>570</td>
      <td>786</td>
      <td>807</td>
      <td>690</td>
      <td>469</td>
      <td>804</td>
      <td>828</td>
      <td>704</td>
      <td>573</td>
      <td>...</td>
      <td>563</td>
      <td>675</td>
      <td>712</td>
      <td>758</td>
      <td>793</td>
      <td>672</td>
      <td>754</td>
      <td>550</td>
      <td>756</td>
      <td>638</td>
    </tr>
    <tr>
      <th>3</th>
      <td>605</td>
      <td>507</td>
      <td>664</td>
      <td>701</td>
      <td>660</td>
      <td>496</td>
      <td>698</td>
      <td>806</td>
      <td>651</td>
      <td>575</td>
      <td>...</td>
      <td>582</td>
      <td>685</td>
      <td>668</td>
      <td>586</td>
      <td>629</td>
      <td>534</td>
      <td>678</td>
      <td>484</td>
      <td>591</td>
      <td>626</td>
    </tr>
    <tr>
      <th>4</th>
      <td>599</td>
      <td>681</td>
      <td>753</td>
      <td>873</td>
      <td>721</td>
      <td>563</td>
      <td>754</td>
      <td>770</td>
      <td>620</td>
      <td>654</td>
      <td>...</td>
      <td>633</td>
      <td>747</td>
      <td>661</td>
      <td>677</td>
      <td>726</td>
      <td>649</td>
      <td>716</td>
      <td>610</td>
      <td>735</td>
      <td>706</td>
    </tr>
    <tr>
      <th>5</th>
      <td>422</td>
      <td>354</td>
      <td>602</td>
      <td>627</td>
      <td>613</td>
      <td>396</td>
      <td>617</td>
      <td>627</td>
      <td>489</td>
      <td>423</td>
      <td>...</td>
      <td>456</td>
      <td>572</td>
      <td>559</td>
      <td>537</td>
      <td>499</td>
      <td>384</td>
      <td>589</td>
      <td>436</td>
      <td>574</td>
      <td>507</td>
    </tr>
    <tr>
      <th>6</th>
      <td>359</td>
      <td>446</td>
      <td>599</td>
      <td>599</td>
      <td>481</td>
      <td>357</td>
      <td>577</td>
      <td>572</td>
      <td>451</td>
      <td>464</td>
      <td>...</td>
      <td>449</td>
      <td>550</td>
      <td>495</td>
      <td>532</td>
      <td>633</td>
      <td>554</td>
      <td>663</td>
      <td>476</td>
      <td>565</td>
      <td>602</td>
    </tr>
    <tr>
      <th>7</th>
      <td>531</td>
      <td>520</td>
      <td>698</td>
      <td>590</td>
      <td>607</td>
      <td>537</td>
      <td>665</td>
      <td>696</td>
      <td>571</td>
      <td>472</td>
      <td>...</td>
      <td>576</td>
      <td>588</td>
      <td>551</td>
      <td>665</td>
      <td>652</td>
      <td>527</td>
      <td>742</td>
      <td>528</td>
      <td>650</td>
      <td>599</td>
    </tr>
    <tr>
      <th>8</th>
      <td>449</td>
      <td>322</td>
      <td>547</td>
      <td>533</td>
      <td>593</td>
      <td>399</td>
      <td>584</td>
      <td>638</td>
      <td>587</td>
      <td>424</td>
      <td>...</td>
      <td>402</td>
      <td>596</td>
      <td>523</td>
      <td>523</td>
      <td>447</td>
      <td>362</td>
      <td>561</td>
      <td>386</td>
      <td>529</td>
      <td>484</td>
    </tr>
    <tr>
      <th>9</th>
      <td>373</td>
      <td>433</td>
      <td>525</td>
      <td>601</td>
      <td>522</td>
      <td>345</td>
      <td>551</td>
      <td>521</td>
      <td>434</td>
      <td>447</td>
      <td>...</td>
      <td>508</td>
      <td>498</td>
      <td>438</td>
      <td>478</td>
      <td>459</td>
      <td>418</td>
      <td>488</td>
      <td>407</td>
      <td>503</td>
      <td>496</td>
    </tr>
    <tr>
      <th>10</th>
      <td>500</td>
      <td>427</td>
      <td>574</td>
      <td>607</td>
      <td>667</td>
      <td>477</td>
      <td>652</td>
      <td>656</td>
      <td>615</td>
      <td>477</td>
      <td>...</td>
      <td>622</td>
      <td>702</td>
      <td>531</td>
      <td>610</td>
      <td>558</td>
      <td>532</td>
      <td>598</td>
      <td>471</td>
      <td>582</td>
      <td>561</td>
    </tr>
    <tr>
      <th>11</th>
      <td>664</td>
      <td>694</td>
      <td>772</td>
      <td>841</td>
      <td>779</td>
      <td>574</td>
      <td>730</td>
      <td>810</td>
      <td>711</td>
      <td>608</td>
      <td>...</td>
      <td>591</td>
      <td>760</td>
      <td>616</td>
      <td>638</td>
      <td>721</td>
      <td>676</td>
      <td>846</td>
      <td>678</td>
      <td>754</td>
      <td>708</td>
    </tr>
    <tr>
      <th>12</th>
      <td>545</td>
      <td>547</td>
      <td>687</td>
      <td>701</td>
      <td>721</td>
      <td>576</td>
      <td>689</td>
      <td>724</td>
      <td>710</td>
      <td>532</td>
      <td>...</td>
      <td>674</td>
      <td>684</td>
      <td>648</td>
      <td>694</td>
      <td>710</td>
      <td>564</td>
      <td>757</td>
      <td>571</td>
      <td>671</td>
      <td>656</td>
    </tr>
    <tr>
      <th>13</th>
      <td>574</td>
      <td>586</td>
      <td>723</td>
      <td>750</td>
      <td>691</td>
      <td>494</td>
      <td>696</td>
      <td>787</td>
      <td>667</td>
      <td>523</td>
      <td>...</td>
      <td>618</td>
      <td>681</td>
      <td>568</td>
      <td>682</td>
      <td>715</td>
      <td>644</td>
      <td>756</td>
      <td>557</td>
      <td>690</td>
      <td>604</td>
    </tr>
    <tr>
      <th>14</th>
      <td>502</td>
      <td>382</td>
      <td>645</td>
      <td>557</td>
      <td>570</td>
      <td>403</td>
      <td>538</td>
      <td>677</td>
      <td>500</td>
      <td>501</td>
      <td>...</td>
      <td>369</td>
      <td>650</td>
      <td>507</td>
      <td>576</td>
      <td>546</td>
      <td>531</td>
      <td>554</td>
      <td>437</td>
      <td>616</td>
      <td>463</td>
    </tr>
    <tr>
      <th>15</th>
      <td>510</td>
      <td>505</td>
      <td>736</td>
      <td>651</td>
      <td>649</td>
      <td>510</td>
      <td>719</td>
      <td>733</td>
      <td>694</td>
      <td>557</td>
      <td>...</td>
      <td>605</td>
      <td>717</td>
      <td>574</td>
      <td>642</td>
      <td>678</td>
      <td>576</td>
      <td>755</td>
      <td>455</td>
      <td>598</td>
      <td>654</td>
    </tr>
    <tr>
      <th>16</th>
      <td>567</td>
      <td>376</td>
      <td>614</td>
      <td>612</td>
      <td>643</td>
      <td>514</td>
      <td>598</td>
      <td>724</td>
      <td>547</td>
      <td>464</td>
      <td>...</td>
      <td>456</td>
      <td>639</td>
      <td>520</td>
      <td>560</td>
      <td>569</td>
      <td>442</td>
      <td>596</td>
      <td>517</td>
      <td>659</td>
      <td>532</td>
    </tr>
    <tr>
      <th>17</th>
      <td>626</td>
      <td>716</td>
      <td>828</td>
      <td>765</td>
      <td>740</td>
      <td>603</td>
      <td>809</td>
      <td>852</td>
      <td>692</td>
      <td>591</td>
      <td>...</td>
      <td>664</td>
      <td>716</td>
      <td>655</td>
      <td>721</td>
      <td>742</td>
      <td>612</td>
      <td>819</td>
      <td>593</td>
      <td>744</td>
      <td>712</td>
    </tr>
    <tr>
      <th>18</th>
      <td>600</td>
      <td>559</td>
      <td>667</td>
      <td>664</td>
      <td>641</td>
      <td>556</td>
      <td>624</td>
      <td>815</td>
      <td>638</td>
      <td>564</td>
      <td>...</td>
      <td>581</td>
      <td>701</td>
      <td>559</td>
      <td>677</td>
      <td>710</td>
      <td>554</td>
      <td>748</td>
      <td>597</td>
      <td>614</td>
      <td>657</td>
    </tr>
    <tr>
      <th>19</th>
      <td>445</td>
      <td>431</td>
      <td>661</td>
      <td>681</td>
      <td>641</td>
      <td>552</td>
      <td>690</td>
      <td>719</td>
      <td>602</td>
      <td>474</td>
      <td>...</td>
      <td>515</td>
      <td>637</td>
      <td>576</td>
      <td>620</td>
      <td>572</td>
      <td>512</td>
      <td>599</td>
      <td>455</td>
      <td>622</td>
      <td>538</td>
    </tr>
    <tr>
      <th>20</th>
      <td>523</td>
      <td>569</td>
      <td>784</td>
      <td>725</td>
      <td>713</td>
      <td>501</td>
      <td>740</td>
      <td>772</td>
      <td>638</td>
      <td>640</td>
      <td>...</td>
      <td>589</td>
      <td>775</td>
      <td>664</td>
      <td>686</td>
      <td>726</td>
      <td>672</td>
      <td>747</td>
      <td>548</td>
      <td>723</td>
      <td>645</td>
    </tr>
    <tr>
      <th>21</th>
      <td>487</td>
      <td>465</td>
      <td>553</td>
      <td>639</td>
      <td>517</td>
      <td>449</td>
      <td>592</td>
      <td>609</td>
      <td>454</td>
      <td>398</td>
      <td>...</td>
      <td>492</td>
      <td>567</td>
      <td>534</td>
      <td>404</td>
      <td>554</td>
      <td>417</td>
      <td>561</td>
      <td>466</td>
      <td>498</td>
      <td>492</td>
    </tr>
    <tr>
      <th>22</th>
      <td>479</td>
      <td>449</td>
      <td>574</td>
      <td>686</td>
      <td>583</td>
      <td>377</td>
      <td>566</td>
      <td>614</td>
      <td>563</td>
      <td>455</td>
      <td>...</td>
      <td>453</td>
      <td>539</td>
      <td>491</td>
      <td>501</td>
      <td>596</td>
      <td>520</td>
      <td>722</td>
      <td>478</td>
      <td>565</td>
      <td>501</td>
    </tr>
    <tr>
      <th>23</th>
      <td>483</td>
      <td>386</td>
      <td>476</td>
      <td>526</td>
      <td>550</td>
      <td>426</td>
      <td>492</td>
      <td>585</td>
      <td>536</td>
      <td>482</td>
      <td>...</td>
      <td>322</td>
      <td>541</td>
      <td>438</td>
      <td>456</td>
      <td>487</td>
      <td>408</td>
      <td>502</td>
      <td>426</td>
      <td>474</td>
      <td>481</td>
    </tr>
    <tr>
      <th>24</th>
      <td>523</td>
      <td>551</td>
      <td>658</td>
      <td>767</td>
      <td>537</td>
      <td>444</td>
      <td>663</td>
      <td>731</td>
      <td>576</td>
      <td>577</td>
      <td>...</td>
      <td>522</td>
      <td>590</td>
      <td>525</td>
      <td>664</td>
      <td>691</td>
      <td>548</td>
      <td>635</td>
      <td>526</td>
      <td>641</td>
      <td>538</td>
    </tr>
    <tr>
      <th>25</th>
      <td>652</td>
      <td>656</td>
      <td>738</td>
      <td>753</td>
      <td>853</td>
      <td>508</td>
      <td>752</td>
      <td>815</td>
      <td>669</td>
      <td>576</td>
      <td>...</td>
      <td>694</td>
      <td>833</td>
      <td>693</td>
      <td>606</td>
      <td>575</td>
      <td>616</td>
      <td>704</td>
      <td>559</td>
      <td>728</td>
      <td>672</td>
    </tr>
    <tr>
      <th>26</th>
      <td>578</td>
      <td>577</td>
      <td>744</td>
      <td>856</td>
      <td>699</td>
      <td>497</td>
      <td>779</td>
      <td>800</td>
      <td>733</td>
      <td>587</td>
      <td>...</td>
      <td>630</td>
      <td>754</td>
      <td>704</td>
      <td>834</td>
      <td>760</td>
      <td>680</td>
      <td>765</td>
      <td>592</td>
      <td>731</td>
      <td>629</td>
    </tr>
    <tr>
      <th>27</th>
      <td>554</td>
      <td>494</td>
      <td>665</td>
      <td>689</td>
      <td>630</td>
      <td>574</td>
      <td>695</td>
      <td>703</td>
      <td>636</td>
      <td>599</td>
      <td>...</td>
      <td>554</td>
      <td>685</td>
      <td>532</td>
      <td>658</td>
      <td>649</td>
      <td>554</td>
      <td>693</td>
      <td>577</td>
      <td>634</td>
      <td>668</td>
    </tr>
    <tr>
      <th>28</th>
      <td>498</td>
      <td>552</td>
      <td>659</td>
      <td>784</td>
      <td>552</td>
      <td>492</td>
      <td>690</td>
      <td>775</td>
      <td>544</td>
      <td>551</td>
      <td>...</td>
      <td>567</td>
      <td>636</td>
      <td>518</td>
      <td>599</td>
      <td>742</td>
      <td>521</td>
      <td>733</td>
      <td>533</td>
      <td>605</td>
      <td>604</td>
    </tr>
    <tr>
      <th>29</th>
      <td>513</td>
      <td>491</td>
      <td>563</td>
      <td>642</td>
      <td>477</td>
      <td>367</td>
      <td>589</td>
      <td>647</td>
      <td>516</td>
      <td>484</td>
      <td>...</td>
      <td>428</td>
      <td>574</td>
      <td>504</td>
      <td>548</td>
      <td>553</td>
      <td>483</td>
      <td>540</td>
      <td>407</td>
      <td>547</td>
      <td>455</td>
    </tr>
  </tbody>
</table>
<p>30 rows × 30 columns</p>
</div>




```python
%timeit x.dot(y)
```

    218 µs ± 18.7 µs per loop (mean ± std. dev. of 7 runs, 1000 loops each)



```python
%timeit np.dot(x, y)
```

    81.1 µs ± 2.85 µs per loop (mean ± std. dev. of 7 runs, 10000 loops each)


* 执行相同运算，Numpy与Pandas的对比


```python
x1 = np.array(x)
x1
```


```python
y1 = np.array(y)
y1
```


```python
%timeit x1.dot(y1)
```

    22.1 µs ± 992 ns per loop (mean ± std. dev. of 7 runs, 10000 loops each)



```python
%timeit np.dot(x1, y1)
```

    22.6 µs ± 766 ns per loop (mean ± std. dev. of 7 runs, 10000 loops each)



```python
%timeit np.dot(x.values, y.values)
```

    42.9 µs ± 1.24 µs per loop (mean ± std. dev. of 7 runs, 10000 loops each)



```python
x2 = list(x1)
y2 = list(y1)
x3 = []
y3 = []
for i in x2:
    res = []
    for j in i:
        res.append(int(j))
    x3.append(res)
for i in y2:
    res = []
    for j in i:
        res.append(int(j))
    y3.append(res)
```


```python
def f(x, y):
    res = []
    for i in range(len(x)):
        row = []
        for j in range(len(y[0])):
            sum_row = 0
            for k in range(len(x[0])):
                sum_row += x[i][k]*y[k][j]
            row.append(sum_row)
        res.append(row)
    return res          
```


```python
%timeit f(x3, y3)
```

    4.29 ms ± 207 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)


**一般来说，纯粹的计算在Numpy里执行的更快**

Numpy更侧重于计算，Pandas更侧重于数据处理

**（3）广播运算**


```python
np.random.seed(42)
x = pd.DataFrame(np.random.randint(10, size=(3, 3)), columns=list("ABC"))
x
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
      <td>6</td>
      <td>3</td>
      <td>7</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4</td>
      <td>6</td>
      <td>9</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>6</td>
      <td>7</td>
    </tr>
  </tbody>
</table>
</div>



* 按行广播


```python
x.iloc[0]
```




    A    6
    B    3
    C    7
    Name: 0, dtype: int32




```python
x/x.iloc[0]
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
      <td>1.000000</td>
      <td>1.0</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.666667</td>
      <td>2.0</td>
      <td>1.285714</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.333333</td>
      <td>2.0</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>



* 按列广播


```python
x.A
```




    0    6
    1    4
    2    2
    Name: A, dtype: int32




```python
x.div(x.A, axis=0)             # add sub div mul
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
      <td>1.0</td>
      <td>0.5</td>
      <td>1.166667</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.0</td>
      <td>1.5</td>
      <td>2.250000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1.0</td>
      <td>3.0</td>
      <td>3.500000</td>
    </tr>
  </tbody>
</table>
</div>




```python
x.div(x.iloc[0], axis=1)
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
      <td>1.000000</td>
      <td>1.0</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.666667</td>
      <td>2.0</td>
      <td>1.285714</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.333333</td>
      <td>2.0</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>



**3、新的用法**

**（1）索引对齐**


```python
A = pd.DataFrame(np.random.randint(0, 20, size=(2, 2)), columns=list("AB"))
A
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3</td>
      <td>7</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
B = pd.DataFrame(np.random.randint(0, 10, size=(3, 3)), columns=list("ABC"))
B
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
      <td>7</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4</td>
      <td>0</td>
      <td>9</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5</td>
      <td>8</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



* pandas会自动对齐两个对象的索引，没有的值用np.nan表示


```python
A+B
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
      <td>10.0</td>
      <td>12.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>6.0</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



* 缺省值也可用fill_value来填充


```python
A.add(B, fill_value=0)
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
      <td>10.0</td>
      <td>12.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>6.0</td>
      <td>1.0</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5.0</td>
      <td>8.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
A*B
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
      <td>21.0</td>
      <td>35.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>8.0</td>
      <td>0.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



**（2）统计相关**

* 数据种类统计


```python
y = np.random.randint(3, size=20)
y
```




    array([2, 2, 2, 1, 2, 1, 1, 2, 1, 2, 2, 0, 2, 0, 2, 2, 0, 0, 2, 1])




```python
np.unique(y)
```




    array([0, 1, 2])



用Counter方法统计数据


```python
from collections import Counter
Counter(y)
```




    Counter({2: 11, 1: 5, 0: 4})




```python
y1 = pd.DataFrame(y, columns=["A"])
y1
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1</td>
    </tr>
    <tr>
      <th>6</th>
      <td>1</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2</td>
    </tr>
    <tr>
      <th>8</th>
      <td>1</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2</td>
    </tr>
    <tr>
      <th>11</th>
      <td>0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>2</td>
    </tr>
    <tr>
      <th>13</th>
      <td>0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>2</td>
    </tr>
    <tr>
      <th>15</th>
      <td>2</td>
    </tr>
    <tr>
      <th>16</th>
      <td>0</td>
    </tr>
    <tr>
      <th>17</th>
      <td>0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>2</td>
    </tr>
    <tr>
      <th>19</th>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



np.unique(y1)

有value counter的方法


```python
y1["A"].value_counts()
```




    2    11
    1     5
    0     4
    Name: A, dtype: int64



* 产生新的结果，并进行排序


```python
population_dict = {"BeiJing": 2154,
                   "ShangHai": 2424,
                   "ShenZhen": 1303,
                   "HangZhou": 981 }
population = pd.Series(population_dict) 

GDP_dict = {"BeiJing": 30320,
            "ShangHai": 32680,
            "ShenZhen": 24222,
            "HangZhou": 13468 }
GDP = pd.Series(GDP_dict)

city_info = pd.DataFrame({"population": population,"GDP": GDP})
city_info
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
      <th>population</th>
      <th>GDP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>BeiJing</th>
      <td>2154</td>
      <td>30320</td>
    </tr>
    <tr>
      <th>ShangHai</th>
      <td>2424</td>
      <td>32680</td>
    </tr>
    <tr>
      <th>ShenZhen</th>
      <td>1303</td>
      <td>24222</td>
    </tr>
    <tr>
      <th>HangZhou</th>
      <td>981</td>
      <td>13468</td>
    </tr>
  </tbody>
</table>
</div>




```python
city_info["per_GDP"] = city_info["GDP"]/city_info["population"]
city_info
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
      <th>population</th>
      <th>GDP</th>
      <th>per_GDP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>BeiJing</th>
      <td>2154</td>
      <td>30320</td>
      <td>14.076137</td>
    </tr>
    <tr>
      <th>ShangHai</th>
      <td>2424</td>
      <td>32680</td>
      <td>13.481848</td>
    </tr>
    <tr>
      <th>ShenZhen</th>
      <td>1303</td>
      <td>24222</td>
      <td>18.589409</td>
    </tr>
    <tr>
      <th>HangZhou</th>
      <td>981</td>
      <td>13468</td>
      <td>13.728848</td>
    </tr>
  </tbody>
</table>
</div>



递增排序


```python
city_info.sort_values(by="per_GDP")
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
      <th>population</th>
      <th>GDP</th>
      <th>per_GDP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>ShangHai</th>
      <td>2424</td>
      <td>32680</td>
      <td>13.481848</td>
    </tr>
    <tr>
      <th>HangZhou</th>
      <td>981</td>
      <td>13468</td>
      <td>13.728848</td>
    </tr>
    <tr>
      <th>BeiJing</th>
      <td>2154</td>
      <td>30320</td>
      <td>14.076137</td>
    </tr>
    <tr>
      <th>ShenZhen</th>
      <td>1303</td>
      <td>24222</td>
      <td>18.589409</td>
    </tr>
  </tbody>
</table>
</div>



递减排序


```python
city_info.sort_values(by="per_GDP", ascending=False)
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
      <th>population</th>
      <th>GDP</th>
      <th>per_GDP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>ShenZhen</th>
      <td>1303</td>
      <td>24222</td>
      <td>18.589409</td>
    </tr>
    <tr>
      <th>BeiJing</th>
      <td>2154</td>
      <td>30320</td>
      <td>14.076137</td>
    </tr>
    <tr>
      <th>HangZhou</th>
      <td>981</td>
      <td>13468</td>
      <td>13.728848</td>
    </tr>
    <tr>
      <th>ShangHai</th>
      <td>2424</td>
      <td>32680</td>
      <td>13.481848</td>
    </tr>
  </tbody>
</table>
</div>



**按轴进行排序**


```python
data = pd.DataFrame(np.random.randint(20, size=(3, 4)), index=[2, 1, 0], columns=list("CBAD"))
data
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
      <th>C</th>
      <th>B</th>
      <th>A</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>13</td>
      <td>17</td>
      <td>8</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>19</td>
      <td>14</td>
      <td>6</td>
    </tr>
    <tr>
      <th>0</th>
      <td>11</td>
      <td>7</td>
      <td>14</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>



行排序


```python
data.sort_index()
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
      <th>C</th>
      <th>B</th>
      <th>A</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>11</td>
      <td>7</td>
      <td>14</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>19</td>
      <td>14</td>
      <td>6</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>13</td>
      <td>17</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>



列排序


```python
data.sort_index(axis=1)
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
      <th>2</th>
      <td>17</td>
      <td>13</td>
      <td>3</td>
      <td>8</td>
    </tr>
    <tr>
      <th>1</th>
      <td>14</td>
      <td>19</td>
      <td>1</td>
      <td>6</td>
    </tr>
    <tr>
      <th>0</th>
      <td>14</td>
      <td>7</td>
      <td>11</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>




```python
data.sort_index(axis=1, ascending=False)
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
      <th>D</th>
      <th>C</th>
      <th>B</th>
      <th>A</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>8</td>
      <td>3</td>
      <td>13</td>
      <td>17</td>
    </tr>
    <tr>
      <th>1</th>
      <td>6</td>
      <td>1</td>
      <td>19</td>
      <td>14</td>
    </tr>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>11</td>
      <td>7</td>
      <td>14</td>
    </tr>
  </tbody>
</table>
</div>



* 统计方法


```python
df = pd.DataFrame(np.random.normal(2, 4, size=(6, 4)),columns=list("ABCD"))
df
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
      <td>1.082198</td>
      <td>3.557396</td>
      <td>-3.060476</td>
      <td>6.367969</td>
    </tr>
    <tr>
      <th>1</th>
      <td>13.113252</td>
      <td>6.774559</td>
      <td>2.874553</td>
      <td>5.527044</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-2.036341</td>
      <td>-4.333177</td>
      <td>5.094802</td>
      <td>-0.152567</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-3.386712</td>
      <td>-1.522365</td>
      <td>-2.522209</td>
      <td>2.537716</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4.328491</td>
      <td>5.550994</td>
      <td>5.577329</td>
      <td>5.019991</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1.171336</td>
      <td>-0.493910</td>
      <td>-4.032613</td>
      <td>6.398588</td>
    </tr>
  </tbody>
</table>
</div>



非空个数


```python
df.count()
```




    A    6
    B    6
    C    6
    D    6
    dtype: int64



求和


```python
df.sum()
```




    A    14.272224
    B     9.533497
    C     3.931385
    D    25.698741
    dtype: float64




```python
df.sum(axis=1)
```




    0     7.947086
    1    28.289408
    2    -1.427283
    3    -4.893571
    4    20.476806
    5     3.043402
    dtype: float64



最大值 最小值


```python
df.min()
```




    A   -3.386712
    B   -4.333177
    C   -4.032613
    D   -0.152567
    dtype: float64




```python
df.max(axis=1)
```




    0     6.367969
    1    13.113252
    2     5.094802
    3     2.537716
    4     5.577329
    5     6.398588
    dtype: float64




```python
df
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
      <td>1.082198</td>
      <td>3.557396</td>
      <td>-3.060476</td>
      <td>6.367969</td>
    </tr>
    <tr>
      <th>1</th>
      <td>13.113252</td>
      <td>6.774559</td>
      <td>2.874553</td>
      <td>5.527044</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-2.036341</td>
      <td>-4.333177</td>
      <td>5.094802</td>
      <td>-0.152567</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-3.386712</td>
      <td>-1.522365</td>
      <td>-2.522209</td>
      <td>2.537716</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4.328491</td>
      <td>5.550994</td>
      <td>5.577329</td>
      <td>5.019991</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1.171336</td>
      <td>-0.493910</td>
      <td>-4.032613</td>
      <td>6.398588</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.idxmax()
```




    A    1
    B    1
    C    4
    D    5
    dtype: int64



均值


```python
df.mean()
```




    A    2.378704
    B    1.588916
    C    0.655231
    D    4.283124
    dtype: float64



方差


```python
df.var()
```




    A    34.980702
    B    19.110656
    C    18.948144
    D     6.726776
    dtype: float64



标准差


```python
df.std()
```




    A    5.914449
    B    4.371574
    C    4.352947
    D    2.593603
    dtype: float64



中位数


```python
df.median()
```




    A    1.126767
    B    1.531743
    C    0.176172
    D    5.273518
    dtype: float64



众数


```python
data = pd.DataFrame(np.random.randint(5, size=(10, 2)), columns=list("AB"))
data
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>4</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>4</td>
      <td>1</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>8</th>
      <td>3</td>
      <td>4</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
data.mode()
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



75%分位数


```python
df.quantile(0.75)
```




    A    3.539202
    B    5.052594
    C    4.539740
    D    6.157738
    Name: 0.75, dtype: float64



+ 用describe()可以获取所有属性


```python
df.describe()
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
      <th>count</th>
      <td>6.000000</td>
      <td>6.000000</td>
      <td>6.000000</td>
      <td>6.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>2.378704</td>
      <td>1.588916</td>
      <td>0.655231</td>
      <td>4.283124</td>
    </tr>
    <tr>
      <th>std</th>
      <td>5.914449</td>
      <td>4.371574</td>
      <td>4.352947</td>
      <td>2.593603</td>
    </tr>
    <tr>
      <th>min</th>
      <td>-3.386712</td>
      <td>-4.333177</td>
      <td>-4.032613</td>
      <td>-0.152567</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>-1.256706</td>
      <td>-1.265251</td>
      <td>-2.925910</td>
      <td>3.158284</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>1.126767</td>
      <td>1.531743</td>
      <td>0.176172</td>
      <td>5.273518</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>3.539202</td>
      <td>5.052594</td>
      <td>4.539740</td>
      <td>6.157738</td>
    </tr>
    <tr>
      <th>max</th>
      <td>13.113252</td>
      <td>6.774559</td>
      <td>5.577329</td>
      <td>6.398588</td>
    </tr>
  </tbody>
</table>
</div>




```python
data_2 = pd.DataFrame([["a", "a", "c", "d"],
                       ["c", "a", "c", "b"],
                       ["a", "a", "d", "c"]], columns=list("ABCD"))
data_2
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
      <td>a</td>
      <td>a</td>
      <td>c</td>
      <td>d</td>
    </tr>
    <tr>
      <th>1</th>
      <td>c</td>
      <td>a</td>
      <td>c</td>
      <td>b</td>
    </tr>
    <tr>
      <th>2</th>
      <td>a</td>
      <td>a</td>
      <td>d</td>
      <td>c</td>
    </tr>
  </tbody>
</table>
</div>



+ 字符串类型的describe


```python
data_2.describe()
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
      <th>count</th>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>top</th>
      <td>a</td>
      <td>a</td>
      <td>c</td>
      <td>d</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>2</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



相关性系数和协方差


```python
df.corr()
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
      <th>A</th>
      <td>1.000000</td>
      <td>0.831063</td>
      <td>0.331060</td>
      <td>0.510821</td>
    </tr>
    <tr>
      <th>B</th>
      <td>0.831063</td>
      <td>1.000000</td>
      <td>0.179244</td>
      <td>0.719112</td>
    </tr>
    <tr>
      <th>C</th>
      <td>0.331060</td>
      <td>0.179244</td>
      <td>1.000000</td>
      <td>-0.450365</td>
    </tr>
    <tr>
      <th>D</th>
      <td>0.510821</td>
      <td>0.719112</td>
      <td>-0.450365</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.corrwith(df["A"])
```




    A    1.000000
    B    0.831063
    C    0.331060
    D    0.510821
    dtype: float64



自定义输出

apply（method）的用法：使用method方法默认对每一列进行相应的操作


```python
df
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
      <td>1.082198</td>
      <td>3.557396</td>
      <td>-3.060476</td>
      <td>6.367969</td>
    </tr>
    <tr>
      <th>1</th>
      <td>13.113252</td>
      <td>6.774559</td>
      <td>2.874553</td>
      <td>5.527044</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-2.036341</td>
      <td>-4.333177</td>
      <td>5.094802</td>
      <td>-0.152567</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-3.386712</td>
      <td>-1.522365</td>
      <td>-2.522209</td>
      <td>2.537716</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4.328491</td>
      <td>5.550994</td>
      <td>5.577329</td>
      <td>5.019991</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1.171336</td>
      <td>-0.493910</td>
      <td>-4.032613</td>
      <td>6.398588</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.apply(np.cumsum)
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
      <td>1.082198</td>
      <td>3.557396</td>
      <td>-3.060476</td>
      <td>6.367969</td>
    </tr>
    <tr>
      <th>1</th>
      <td>14.195450</td>
      <td>10.331955</td>
      <td>-0.185923</td>
      <td>11.895013</td>
    </tr>
    <tr>
      <th>2</th>
      <td>12.159109</td>
      <td>5.998778</td>
      <td>4.908878</td>
      <td>11.742447</td>
    </tr>
    <tr>
      <th>3</th>
      <td>8.772397</td>
      <td>4.476413</td>
      <td>2.386669</td>
      <td>14.280162</td>
    </tr>
    <tr>
      <th>4</th>
      <td>13.100888</td>
      <td>10.027406</td>
      <td>7.963999</td>
      <td>19.300153</td>
    </tr>
    <tr>
      <th>5</th>
      <td>14.272224</td>
      <td>9.533497</td>
      <td>3.931385</td>
      <td>25.698741</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.apply(np.cumsum, axis=1)
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
      <td>1.082198</td>
      <td>4.639594</td>
      <td>1.579117</td>
      <td>7.947086</td>
    </tr>
    <tr>
      <th>1</th>
      <td>13.113252</td>
      <td>19.887811</td>
      <td>22.762364</td>
      <td>28.289408</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-2.036341</td>
      <td>-6.369518</td>
      <td>-1.274717</td>
      <td>-1.427283</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-3.386712</td>
      <td>-4.909077</td>
      <td>-7.431287</td>
      <td>-4.893571</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4.328491</td>
      <td>9.879485</td>
      <td>15.456814</td>
      <td>20.476806</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1.171336</td>
      <td>0.677427</td>
      <td>-3.355186</td>
      <td>3.043402</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.apply(sum)
```




    A    14.272224
    B     9.533497
    C     3.931385
    D    25.698741
    dtype: float64




```python
df.sum()
```




    A    14.272224
    B     9.533497
    C     3.931385
    D    25.698741
    dtype: float64




```python
df.apply(lambda x: x.max()-x.min())
```




    A    16.499965
    B    11.107736
    C     9.609942
    D     6.551155
    dtype: float64




```python
def my_describe(x):
    return pd.Series([x.count(), x.mean(), x.max(), x.idxmin(), x.std()], \
                     index=["Count", "mean", "max", "idxmin", "std"])
df.apply(my_describe)
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
      <th>Count</th>
      <td>6.000000</td>
      <td>6.000000</td>
      <td>6.000000</td>
      <td>6.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>2.378704</td>
      <td>1.588916</td>
      <td>0.655231</td>
      <td>4.283124</td>
    </tr>
    <tr>
      <th>max</th>
      <td>13.113252</td>
      <td>6.774559</td>
      <td>5.577329</td>
      <td>6.398588</td>
    </tr>
    <tr>
      <th>idxmin</th>
      <td>3.000000</td>
      <td>2.000000</td>
      <td>5.000000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>std</th>
      <td>5.914449</td>
      <td>4.371574</td>
      <td>4.352947</td>
      <td>2.593603</td>
    </tr>
  </tbody>
</table>
</div>



## 11.4 缺失值处理

**1、发现缺失值**


```python
import pandas as pd
import numpy as np

data = pd.DataFrame(np.array([[1, np.nan, 2],
                              [np.nan, 3, 4],
                              [5, 6, None]]), columns=["A", "B", "C"])
data
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
      <td>1</td>
      <td>NaN</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NaN</td>
      <td>3</td>
      <td>4</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5</td>
      <td>6</td>
      <td>None</td>
    </tr>
  </tbody>
</table>
</div>



**注意：有None、字符串等，数据类型全部变为object，它比int和float更消耗资源**

np.nan是一个特殊的浮点数，类型是浮点类型，所以表示缺失值时最好使用NaN。


```python
data.dtypes
```




    A    object
    B    object
    C    object
    dtype: object




```python
data.isnull()
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
      <td>False</td>
      <td>True</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>True</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>False</td>
      <td>False</td>
      <td>True</td>
    </tr>
  </tbody>
</table>
</div>




```python
data.notnull()
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
      <td>True</td>
      <td>False</td>
      <td>True</td>
    </tr>
    <tr>
      <th>1</th>
      <td>False</td>
      <td>True</td>
      <td>True</td>
    </tr>
    <tr>
      <th>2</th>
      <td>True</td>
      <td>True</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>



**2、删除缺失值**


```python
data = pd.DataFrame(np.array([[1, np.nan, 2, 3],
                              [np.nan, 4, 5, 6],
                              [7, 8, np.nan, 9],
                              [10, 11 , 12, 13]]), columns=["A", "B", "C", "D"])
data
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
      <td>1.0</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NaN</td>
      <td>4.0</td>
      <td>5.0</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>7.0</td>
      <td>8.0</td>
      <td>NaN</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10.0</td>
      <td>11.0</td>
      <td>12.0</td>
      <td>13.0</td>
    </tr>
  </tbody>
</table>
</div>



**注意：np.nan是一种特殊的浮点数**


```python
data.dtypes
```




    A    float64
    B    float64
    C    float64
    D    float64
    dtype: object



**（1）删除整行**


```python
data.dropna()
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
      <th>3</th>
      <td>10.0</td>
      <td>11.0</td>
      <td>12.0</td>
      <td>13.0</td>
    </tr>
  </tbody>
</table>
</div>



**（2）删除整列**


```python
data.dropna(axis="columns")
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
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>6.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>9.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>13.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
data["D"] = np.nan
data
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
      <td>1.0</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NaN</td>
      <td>4.0</td>
      <td>5.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>7.0</td>
      <td>8.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10.0</td>
      <td>11.0</td>
      <td>12.0</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
data.dropna(axis="columns", how="all")
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
      <td>1.0</td>
      <td>NaN</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NaN</td>
      <td>4.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>7.0</td>
      <td>8.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10.0</td>
      <td>11.0</td>
      <td>12.0</td>
    </tr>
  </tbody>
</table>
</div>



all表示都是缺失值时才删除。


```python
data.dropna(axis="columns", how="any")
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
    </tr>
    <tr>
      <th>1</th>
    </tr>
    <tr>
      <th>2</th>
    </tr>
    <tr>
      <th>3</th>
    </tr>
  </tbody>
</table>
</div>




```python
data.loc[3] = np.nan
data
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
      <td>1.0</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NaN</td>
      <td>4.0</td>
      <td>5.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>7.0</td>
      <td>8.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
data.dropna(how="all")
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
      <td>1.0</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NaN</td>
      <td>4.0</td>
      <td>5.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>7.0</td>
      <td>8.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



**3、填充缺失值**


```python
data = pd.DataFrame(np.array([[1, np.nan, 2, 3],
                              [np.nan, 4, 5, 6],
                              [7, 8, np.nan, 9],
                              [10, 11 , 12, 13]]), columns=["A", "B", "C", "D"])
data
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
      <td>1.0</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NaN</td>
      <td>4.0</td>
      <td>5.0</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>7.0</td>
      <td>8.0</td>
      <td>NaN</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10.0</td>
      <td>11.0</td>
      <td>12.0</td>
      <td>13.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
data.fillna(value=5)
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
      <td>1.0</td>
      <td>5.0</td>
      <td>2.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>5.0</td>
      <td>4.0</td>
      <td>5.0</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>7.0</td>
      <td>8.0</td>
      <td>5.0</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10.0</td>
      <td>11.0</td>
      <td>12.0</td>
      <td>13.0</td>
    </tr>
  </tbody>
</table>
</div>



* 用均值进行替换


```python
fill = data.mean()
fill
```




    A    6.000000
    B    7.666667
    C    6.333333
    D    7.750000
    dtype: float64




```python
data.fillna(value=fill)
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
      <td>1.0</td>
      <td>7.666667</td>
      <td>2.000000</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>6.0</td>
      <td>4.000000</td>
      <td>5.000000</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>7.0</td>
      <td>8.000000</td>
      <td>6.333333</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10.0</td>
      <td>11.000000</td>
      <td>12.000000</td>
      <td>13.0</td>
    </tr>
  </tbody>
</table>
</div>



全部数据的平均值，先进行摊平，再进行填充即可。


```python
fill = data.stack().mean()
fill
```




    7.0




```python
data.fillna(value=fill)
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
      <td>1.0</td>
      <td>7.0</td>
      <td>2.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>7.0</td>
      <td>4.0</td>
      <td>5.0</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>7.0</td>
      <td>8.0</td>
      <td>7.0</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10.0</td>
      <td>11.0</td>
      <td>12.0</td>
      <td>13.0</td>
    </tr>
  </tbody>
</table>
</div>



## 11.5 合并数据

* 构造一个生产DataFrame的函数


```python
import pandas as pd
import numpy as np

def make_df(cols, ind):
    "一个简单的DataFrame"
    data = {c: [str(c)+str(i) for i in ind]  for c in cols}
    return pd.DataFrame(data, ind)

make_df("ABC", range(3))
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
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
      <td>C1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
    </tr>
  </tbody>
</table>
</div>



* 垂直合并


```python
df_1 = make_df("AB", [1, 2])
df_2 = make_df("AB", [3, 4])
print(df_1)
print(df_2)
```

        A   B
    1  A1  B1
    2  A2  B2
        A   B
    3  A3  B3
    4  A4  B4



```python
pd.concat([df_1, df_2])
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A3</td>
      <td>B3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>A4</td>
      <td>B4</td>
    </tr>
  </tbody>
</table>
</div>



* 水平合并


```python
df_3 = make_df("AB", [0, 1])
df_4 = make_df("CD", [0, 1])
print(df_3)
print(df_4)
```

        A   B
    0  A0  B0
    1  A1  B1
        C   D
    0  C0  D0
    1  C1  D1



```python
pd.concat([df_3, df_4], axis=1)
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
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
      <td>D0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
      <td>C1</td>
      <td>D1</td>
    </tr>
  </tbody>
</table>
</div>



* 索引重叠

行重叠


```python
df_5 = make_df("AB", [1, 2])
df_6 = make_df("AB", [1, 2])
print(df_5)
print(df_6)
```

        A   B
    1  A1  B1
    2  A2  B2
        A   B
    1  A1  B1
    2  A2  B2



```python
pd.concat([df_5, df_6])
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.concat([df_5, df_6],ignore_index=True)
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A1</td>
      <td>B1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A2</td>
      <td>B2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A1</td>
      <td>B1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A2</td>
      <td>B2</td>
    </tr>
  </tbody>
</table>
</div>



列重叠


```python
df_7 = make_df("ABC", [1, 2])
df_8 = make_df("BCD", [1, 2])
print(df_7)
print(df_8)
```

        A   B   C
    1  A1  B1  C1
    2  A2  B2  C2
        B   C   D
    1  B1  C1  D1
    2  B2  C2  D2



```python
pd.concat([df_7, df_8], axis=1)
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
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
      <td>C1</td>
      <td>B1</td>
      <td>C1</td>
      <td>D1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
      <td>B2</td>
      <td>C2</td>
      <td>D2</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.concat([df_7, df_8],axis=1, ignore_index=True)
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
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
      <td>C1</td>
      <td>B1</td>
      <td>C1</td>
      <td>D1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
      <td>B2</td>
      <td>C2</td>
      <td>D2</td>
    </tr>
  </tbody>
</table>
</div>



* 对齐合并merge()


```python
df_9 = make_df("AB", [1, 2])
df_10 = make_df("BC", [1, 2])
print(df_9)
print(df_10)
```

        A   B
    1  A1  B1
    2  A2  B2
        B   C
    1  B1  C1
    2  B2  C2



```python
pd.merge(df_9, df_10)
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
      <td>A1</td>
      <td>B1</td>
      <td>C1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_9 = make_df("AB", [1, 2])
df_10 = make_df("CB", [2, 1])
print(df_9)
print(df_10)
```

        A   B
    1  A1  B1
    2  A2  B2
        C   B
    2  C2  B2
    1  C1  B1



```python
pd.merge(df_9, df_10)
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
      <td>A1</td>
      <td>B1</td>
      <td>C1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
    </tr>
  </tbody>
</table>
</div>



【例】 合并城市信息


```python
population_dict = {"city": ("BeiJing", "HangZhou", "ShenZhen"),
                   "pop": (2154, 981, 1303)}
population = pd.DataFrame(population_dict)
population
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
      <th>city</th>
      <th>pop</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>BeiJing</td>
      <td>2154</td>
    </tr>
    <tr>
      <th>1</th>
      <td>HangZhou</td>
      <td>981</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ShenZhen</td>
      <td>1303</td>
    </tr>
  </tbody>
</table>
</div>




```python
GDP_dict = {"city": ("BeiJing", "ShangHai", "HangZhou"),
            "GDP": (30320, 32680, 13468)}
GDP = pd.DataFrame(GDP_dict)
GDP
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
      <th>city</th>
      <th>GDP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>BeiJing</td>
      <td>30320</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ShangHai</td>
      <td>32680</td>
    </tr>
    <tr>
      <th>2</th>
      <td>HangZhou</td>
      <td>13468</td>
    </tr>
  </tbody>
</table>
</div>




```python
city_info = pd.merge(population, GDP)
city_info
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
      <th>city</th>
      <th>pop</th>
      <th>GDP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>BeiJing</td>
      <td>2154</td>
      <td>30320</td>
    </tr>
    <tr>
      <th>1</th>
      <td>HangZhou</td>
      <td>981</td>
      <td>13468</td>
    </tr>
  </tbody>
</table>
</div>



这里outer是求并集


```python
city_info = pd.merge(population, GDP, how="outer")
city_info
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
      <th>city</th>
      <th>pop</th>
      <th>GDP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>BeiJing</td>
      <td>2154.0</td>
      <td>30320.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>HangZhou</td>
      <td>981.0</td>
      <td>13468.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ShenZhen</td>
      <td>1303.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ShangHai</td>
      <td>NaN</td>
      <td>32680.0</td>
    </tr>
  </tbody>
</table>
</div>



## 11.6 分组和数据透视表

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221002211059551.png)


```python
df = pd.DataFrame({"key":["A", "B", "C", "C", "B", "A"],
                  "data1": range(6),
                  "data2": np.random.randint(0, 10, size=6)})
df
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
      <th>key</th>
      <th>data1</th>
      <th>data2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>B</td>
      <td>1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>2</th>
      <td>C</td>
      <td>2</td>
      <td>9</td>
    </tr>
    <tr>
      <th>3</th>
      <td>C</td>
      <td>3</td>
      <td>9</td>
    </tr>
    <tr>
      <th>4</th>
      <td>B</td>
      <td>4</td>
      <td>1</td>
    </tr>
    <tr>
      <th>5</th>
      <td>A</td>
      <td>5</td>
      <td>9</td>
    </tr>
  </tbody>
</table>
</div>



**（1）分组**

* 延迟计算


```python
df.groupby("key")
```




    <pandas.core.groupby.generic.DataFrameGroupBy object at 0x000002276795A240>



这说明已经分好了，等待我们用什么样的方法进行处理后，再显示。


```python
df.groupby("key").sum()
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
      <th>data1</th>
      <th>data2</th>
    </tr>
    <tr>
      <th>key</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>A</th>
      <td>5</td>
      <td>10</td>
    </tr>
    <tr>
      <th>B</th>
      <td>5</td>
      <td>6</td>
    </tr>
    <tr>
      <th>C</th>
      <td>5</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.groupby("key").mean()
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
      <th>data1</th>
      <th>data2</th>
    </tr>
    <tr>
      <th>key</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>A</th>
      <td>2.5</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>B</th>
      <td>2.5</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>C</th>
      <td>2.5</td>
      <td>5.5</td>
    </tr>
  </tbody>
</table>
</div>



可以打印看看这是什么东西：


```python
for i in df.groupby("key"):
    print(str(i))
```

    ('A',   key  data1  data2
    0   A      0      2
    5   A      5      8)
    ('B',   key  data1  data2
    1   B      1      2
    4   B      4      4)
    ('C',   key  data1  data2
    2   C      2      8
    3   C      3      3)


* 按列取值


```python
df.groupby("key")["data2"].sum()
```




    key
    A    10
    B     6
    C    11
    Name: data2, dtype: int32



* 按组迭代


```python
for data, group in df.groupby("key"):
    print("{0:5} shape={1}".format(data, group.shape))
```

    A     shape=(2, 3)
    B     shape=(2, 3)
    C     shape=(2, 3)


* 调用方法


```python
df.groupby("key")["data1"].describe()
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
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
    <tr>
      <th>key</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>A</th>
      <td>2.0</td>
      <td>2.5</td>
      <td>3.535534</td>
      <td>0.0</td>
      <td>1.25</td>
      <td>2.5</td>
      <td>3.75</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>B</th>
      <td>2.0</td>
      <td>2.5</td>
      <td>2.121320</td>
      <td>1.0</td>
      <td>1.75</td>
      <td>2.5</td>
      <td>3.25</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>C</th>
      <td>2.0</td>
      <td>2.5</td>
      <td>0.707107</td>
      <td>2.0</td>
      <td>2.25</td>
      <td>2.5</td>
      <td>2.75</td>
      <td>3.0</td>
    </tr>
  </tbody>
</table>
</div>



* 支持更复杂的操作


```python
df.groupby("key").aggregate(["min", "median", "max"])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead tr th {
        text-align: left;
    }
    
    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="3" halign="left">data1</th>
      <th colspan="3" halign="left">data2</th>
    </tr>
    <tr>
      <th></th>
      <th>min</th>
      <th>median</th>
      <th>max</th>
      <th>min</th>
      <th>median</th>
      <th>max</th>
    </tr>
    <tr>
      <th>key</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>A</th>
      <td>0</td>
      <td>2.5</td>
      <td>5</td>
      <td>2</td>
      <td>5.0</td>
      <td>8</td>
    </tr>
    <tr>
      <th>B</th>
      <td>1</td>
      <td>2.5</td>
      <td>4</td>
      <td>2</td>
      <td>3.0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>C</th>
      <td>2</td>
      <td>2.5</td>
      <td>3</td>
      <td>3</td>
      <td>5.5</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>



* 过滤


```python
def filter_func(x):
    return x["data2"].std() > 3
df.groupby("key")["data2"].std()
```




    key
    A    4.242641
    B    1.414214
    C    3.535534
    Name: data2, dtype: float64




```python
df.groupby("key").filter(filter_func)
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
      <th>key</th>
      <th>data1</th>
      <th>data2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>C</td>
      <td>2</td>
      <td>8</td>
    </tr>
    <tr>
      <th>3</th>
      <td>C</td>
      <td>3</td>
      <td>3</td>
    </tr>
    <tr>
      <th>5</th>
      <td>A</td>
      <td>5</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>



* 转换


```python
df
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
      <th>key</th>
      <th>data1</th>
      <th>data2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>B</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>C</td>
      <td>2</td>
      <td>8</td>
    </tr>
    <tr>
      <th>3</th>
      <td>C</td>
      <td>3</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>B</td>
      <td>4</td>
      <td>4</td>
    </tr>
    <tr>
      <th>5</th>
      <td>A</td>
      <td>5</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.groupby("key").transform(lambda x: x-x.mean())
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
      <th>data1</th>
      <th>data2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-2.5</td>
      <td>-3.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-1.5</td>
      <td>-1.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-0.5</td>
      <td>2.5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.5</td>
      <td>-2.5</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1.5</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2.5</td>
      <td>3.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df
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
      <th>key</th>
      <th>data1</th>
      <th>data2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>B</td>
      <td>1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>2</th>
      <td>C</td>
      <td>2</td>
      <td>9</td>
    </tr>
    <tr>
      <th>3</th>
      <td>C</td>
      <td>3</td>
      <td>9</td>
    </tr>
    <tr>
      <th>4</th>
      <td>B</td>
      <td>4</td>
      <td>1</td>
    </tr>
    <tr>
      <th>5</th>
      <td>A</td>
      <td>5</td>
      <td>9</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.groupby("key").apply(lambda x: x-x.mean())
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
      <th>data1</th>
      <th>data2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-2.5</td>
      <td>-4.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-1.5</td>
      <td>1.5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-0.5</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.5</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1.5</td>
      <td>-1.5</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2.5</td>
      <td>4.0</td>
    </tr>
  </tbody>
</table>
</div>



* apply（）方法


```python
df
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
      <th>key</th>
      <th>data1</th>
      <th>data2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>B</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>C</td>
      <td>2</td>
      <td>8</td>
    </tr>
    <tr>
      <th>3</th>
      <td>C</td>
      <td>3</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>B</td>
      <td>4</td>
      <td>4</td>
    </tr>
    <tr>
      <th>5</th>
      <td>A</td>
      <td>5</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
def norm_by_data2(x):
    x["data1"] /= x["data2"].sum()
    return x
```


```python
df.groupby("key").apply(norm_by_data2)
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
      <th>key</th>
      <th>data1</th>
      <th>data2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A</td>
      <td>0.000000</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>B</td>
      <td>0.166667</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>C</td>
      <td>0.181818</td>
      <td>8</td>
    </tr>
    <tr>
      <th>3</th>
      <td>C</td>
      <td>0.272727</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>B</td>
      <td>0.666667</td>
      <td>4</td>
    </tr>
    <tr>
      <th>5</th>
      <td>A</td>
      <td>0.500000</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>



* 将列表、数组设为分组键

这里的L相当于一个新的标签替代原来的行标签。


```python
L = [0, 1, 0, 1, 2, 0]
df
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
      <th>key</th>
      <th>data1</th>
      <th>data2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>B</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>C</td>
      <td>2</td>
      <td>8</td>
    </tr>
    <tr>
      <th>3</th>
      <td>C</td>
      <td>3</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>B</td>
      <td>4</td>
      <td>4</td>
    </tr>
    <tr>
      <th>5</th>
      <td>A</td>
      <td>5</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.groupby(L).sum()
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
      <th>data1</th>
      <th>data2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>7</td>
      <td>18</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4</td>
      <td>5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>



* 用字典将索引映射到分组


```python
df2 = df.set_index("key")
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
      <th>data1</th>
      <th>data2</th>
    </tr>
    <tr>
      <th>key</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>A</th>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>B</th>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>C</th>
      <td>2</td>
      <td>8</td>
    </tr>
    <tr>
      <th>C</th>
      <td>3</td>
      <td>3</td>
    </tr>
    <tr>
      <th>B</th>
      <td>4</td>
      <td>4</td>
    </tr>
    <tr>
      <th>A</th>
      <td>5</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
mapping = {"A": "first", "B": "constant", "C": "constant"}
df2.groupby(mapping).sum()
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
      <th>data1</th>
      <th>data2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>constant</th>
      <td>10</td>
      <td>17</td>
    </tr>
    <tr>
      <th>first</th>
      <td>5</td>
      <td>10</td>
    </tr>
  </tbody>
</table>
</div>



* 任意Python函数


```python
df2.groupby(str.lower).mean()
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
      <th>data1</th>
      <th>data2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>2.5</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>b</th>
      <td>2.5</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>c</th>
      <td>2.5</td>
      <td>5.5</td>
    </tr>
  </tbody>
</table>
</div>



* 多个有效值组成的列表

只有这两个数都相等，才会分到同一个组。


```python
df2.groupby([str.lower, mapping]).mean()
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
      <th></th>
      <th>data1</th>
      <th>data2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <th>first</th>
      <td>2.5</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>b</th>
      <th>constant</th>
      <td>2.5</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>c</th>
      <th>constant</th>
      <td>2.5</td>
      <td>5.5</td>
    </tr>
  </tbody>
</table>
</div>



【例1】 行星观测数据处理


```python
import seaborn as sns

planets = sns.load_dataset("planets")
```


```python
planets.shape
```




    (1035, 6)




```python
planets.head()
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
      <th>method</th>
      <th>number</th>
      <th>orbital_period</th>
      <th>mass</th>
      <th>distance</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Radial Velocity</td>
      <td>1</td>
      <td>269.300</td>
      <td>7.10</td>
      <td>77.40</td>
      <td>2006</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Radial Velocity</td>
      <td>1</td>
      <td>874.774</td>
      <td>2.21</td>
      <td>56.95</td>
      <td>2008</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Radial Velocity</td>
      <td>1</td>
      <td>763.000</td>
      <td>2.60</td>
      <td>19.84</td>
      <td>2011</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Radial Velocity</td>
      <td>1</td>
      <td>326.030</td>
      <td>19.40</td>
      <td>110.62</td>
      <td>2007</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Radial Velocity</td>
      <td>1</td>
      <td>516.220</td>
      <td>10.50</td>
      <td>119.47</td>
      <td>2009</td>
    </tr>
  </tbody>
</table>
</div>




```python
planets.describe()
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
      <th>number</th>
      <th>orbital_period</th>
      <th>mass</th>
      <th>distance</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>1035.000000</td>
      <td>992.000000</td>
      <td>513.000000</td>
      <td>808.000000</td>
      <td>1035.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>1.785507</td>
      <td>2002.917596</td>
      <td>2.638161</td>
      <td>264.069282</td>
      <td>2009.070531</td>
    </tr>
    <tr>
      <th>std</th>
      <td>1.240976</td>
      <td>26014.728304</td>
      <td>3.818617</td>
      <td>733.116493</td>
      <td>3.972567</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.000000</td>
      <td>0.090706</td>
      <td>0.003600</td>
      <td>1.350000</td>
      <td>1989.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>1.000000</td>
      <td>5.442540</td>
      <td>0.229000</td>
      <td>32.560000</td>
      <td>2007.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>1.000000</td>
      <td>39.979500</td>
      <td>1.260000</td>
      <td>55.250000</td>
      <td>2010.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>2.000000</td>
      <td>526.005000</td>
      <td>3.040000</td>
      <td>178.500000</td>
      <td>2012.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>7.000000</td>
      <td>730000.000000</td>
      <td>25.000000</td>
      <td>8500.000000</td>
      <td>2014.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
planets.head()
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
      <th>method</th>
      <th>number</th>
      <th>orbital_period</th>
      <th>mass</th>
      <th>distance</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Radial Velocity</td>
      <td>1</td>
      <td>269.300</td>
      <td>7.10</td>
      <td>77.40</td>
      <td>2006</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Radial Velocity</td>
      <td>1</td>
      <td>874.774</td>
      <td>2.21</td>
      <td>56.95</td>
      <td>2008</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Radial Velocity</td>
      <td>1</td>
      <td>763.000</td>
      <td>2.60</td>
      <td>19.84</td>
      <td>2011</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Radial Velocity</td>
      <td>1</td>
      <td>326.030</td>
      <td>19.40</td>
      <td>110.62</td>
      <td>2007</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Radial Velocity</td>
      <td>1</td>
      <td>516.220</td>
      <td>10.50</td>
      <td>119.47</td>
      <td>2009</td>
    </tr>
  </tbody>
</table>
</div>




```python
decade = 10 * (planets["year"] // 10)
decade.head()
```




    0    2000
    1    2000
    2    2010
    3    2000
    4    2000
    Name: year, dtype: int64




```python
decade = decade.astype(str) + "s"
decade.name = "decade"
decade.head()
```




    0    2000s
    1    2000s
    2    2010s
    3    2000s
    4    2000s
    Name: decade, dtype: object




```python
planets.head()
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
      <th>method</th>
      <th>number</th>
      <th>orbital_period</th>
      <th>mass</th>
      <th>distance</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Radial Velocity</td>
      <td>1</td>
      <td>269.300</td>
      <td>7.10</td>
      <td>77.40</td>
      <td>2006</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Radial Velocity</td>
      <td>1</td>
      <td>874.774</td>
      <td>2.21</td>
      <td>56.95</td>
      <td>2008</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Radial Velocity</td>
      <td>1</td>
      <td>763.000</td>
      <td>2.60</td>
      <td>19.84</td>
      <td>2011</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Radial Velocity</td>
      <td>1</td>
      <td>326.030</td>
      <td>19.40</td>
      <td>110.62</td>
      <td>2007</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Radial Velocity</td>
      <td>1</td>
      <td>516.220</td>
      <td>10.50</td>
      <td>119.47</td>
      <td>2009</td>
    </tr>
  </tbody>
</table>
</div>




```python
planets.groupby(["method", decade]).sum()
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
      <th></th>
      <th>number</th>
      <th>orbital_period</th>
      <th>mass</th>
      <th>distance</th>
      <th>year</th>
    </tr>
    <tr>
      <th>method</th>
      <th>decade</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Astrometry</th>
      <th>2010s</th>
      <td>2</td>
      <td>1.262360e+03</td>
      <td>0.00000</td>
      <td>35.75</td>
      <td>4023</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Eclipse Timing Variations</th>
      <th>2000s</th>
      <td>5</td>
      <td>1.930800e+04</td>
      <td>6.05000</td>
      <td>261.44</td>
      <td>6025</td>
    </tr>
    <tr>
      <th>2010s</th>
      <td>10</td>
      <td>2.345680e+04</td>
      <td>4.20000</td>
      <td>1000.00</td>
      <td>12065</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Imaging</th>
      <th>2000s</th>
      <td>29</td>
      <td>1.350935e+06</td>
      <td>0.00000</td>
      <td>956.83</td>
      <td>40139</td>
    </tr>
    <tr>
      <th>2010s</th>
      <td>21</td>
      <td>6.803750e+04</td>
      <td>0.00000</td>
      <td>1210.08</td>
      <td>36208</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Microlensing</th>
      <th>2000s</th>
      <td>12</td>
      <td>1.732500e+04</td>
      <td>0.00000</td>
      <td>0.00</td>
      <td>20070</td>
    </tr>
    <tr>
      <th>2010s</th>
      <td>15</td>
      <td>4.750000e+03</td>
      <td>0.00000</td>
      <td>41440.00</td>
      <td>26155</td>
    </tr>
    <tr>
      <th>Orbital Brightness Modulation</th>
      <th>2010s</th>
      <td>5</td>
      <td>2.127920e+00</td>
      <td>0.00000</td>
      <td>2360.00</td>
      <td>6035</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">Pulsar Timing</th>
      <th>1990s</th>
      <td>9</td>
      <td>1.900153e+02</td>
      <td>0.00000</td>
      <td>0.00</td>
      <td>5978</td>
    </tr>
    <tr>
      <th>2000s</th>
      <td>1</td>
      <td>3.652500e+04</td>
      <td>0.00000</td>
      <td>0.00</td>
      <td>2003</td>
    </tr>
    <tr>
      <th>2010s</th>
      <td>1</td>
      <td>9.070629e-02</td>
      <td>0.00000</td>
      <td>1200.00</td>
      <td>2011</td>
    </tr>
    <tr>
      <th>Pulsation Timing Variations</th>
      <th>2000s</th>
      <td>1</td>
      <td>1.170000e+03</td>
      <td>0.00000</td>
      <td>0.00</td>
      <td>2007</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Radial Velocity</th>
      <th>1980s</th>
      <td>1</td>
      <td>8.388800e+01</td>
      <td>11.68000</td>
      <td>40.57</td>
      <td>1989</td>
    </tr>
    <tr>
      <th>1990s</th>
      <td>52</td>
      <td>1.091561e+04</td>
      <td>68.17820</td>
      <td>723.71</td>
      <td>55943</td>
    </tr>
    <tr>
      <th>2000s</th>
      <td>475</td>
      <td>2.633526e+05</td>
      <td>945.31928</td>
      <td>15201.16</td>
      <td>619775</td>
    </tr>
    <tr>
      <th>2010s</th>
      <td>424</td>
      <td>1.809630e+05</td>
      <td>316.47890</td>
      <td>11382.67</td>
      <td>432451</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Transit</th>
      <th>2000s</th>
      <td>64</td>
      <td>2.897102e+02</td>
      <td>0.00000</td>
      <td>31823.31</td>
      <td>124462</td>
    </tr>
    <tr>
      <th>2010s</th>
      <td>712</td>
      <td>8.087813e+03</td>
      <td>1.47000</td>
      <td>102419.46</td>
      <td>673999</td>
    </tr>
    <tr>
      <th>Transit Timing Variations</th>
      <th>2010s</th>
      <td>9</td>
      <td>2.393505e+02</td>
      <td>0.00000</td>
      <td>3313.00</td>
      <td>8050</td>
    </tr>
  </tbody>
</table>
</div>



这里使用两个中括号[[]]，取出来是DF类型的数据，而一个中括号[]取出来是Serios的数据，前者更美观一点。


```python
planets.groupby(["method", decade])[["number"]].sum().unstack().fillna(0)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead tr th {
        text-align: left;
    }
    
    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="4" halign="left">number</th>
    </tr>
    <tr>
      <th>decade</th>
      <th>1980s</th>
      <th>1990s</th>
      <th>2000s</th>
      <th>2010s</th>
    </tr>
    <tr>
      <th>method</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Astrometry</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>Eclipse Timing Variations</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>5.0</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>Imaging</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>29.0</td>
      <td>21.0</td>
    </tr>
    <tr>
      <th>Microlensing</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>12.0</td>
      <td>15.0</td>
    </tr>
    <tr>
      <th>Orbital Brightness Modulation</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>Pulsar Timing</th>
      <td>0.0</td>
      <td>9.0</td>
      <td>1.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>Pulsation Timing Variations</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>Radial Velocity</th>
      <td>1.0</td>
      <td>52.0</td>
      <td>475.0</td>
      <td>424.0</td>
    </tr>
    <tr>
      <th>Transit</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>64.0</td>
      <td>712.0</td>
    </tr>
    <tr>
      <th>Transit Timing Variations</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>9.0</td>
    </tr>
  </tbody>
</table>
</div>



**（2）数据透视表**

【例2】泰坦尼克号乘客数据分析


```python
import seaborn as sns

titanic = sns.load_dataset("titanic")
```


```python
titanic.head()
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
      <th>survived</th>
      <th>pclass</th>
      <th>sex</th>
      <th>age</th>
      <th>sibsp</th>
      <th>parch</th>
      <th>fare</th>
      <th>embarked</th>
      <th>class</th>
      <th>who</th>
      <th>adult_male</th>
      <th>deck</th>
      <th>embark_town</th>
      <th>alive</th>
      <th>alone</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>3</td>
      <td>male</td>
      <td>22.0</td>
      <td>1</td>
      <td>0</td>
      <td>7.2500</td>
      <td>S</td>
      <td>Third</td>
      <td>man</td>
      <td>True</td>
      <td>NaN</td>
      <td>Southampton</td>
      <td>no</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>1</td>
      <td>female</td>
      <td>38.0</td>
      <td>1</td>
      <td>0</td>
      <td>71.2833</td>
      <td>C</td>
      <td>First</td>
      <td>woman</td>
      <td>False</td>
      <td>C</td>
      <td>Cherbourg</td>
      <td>yes</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>3</td>
      <td>female</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>7.9250</td>
      <td>S</td>
      <td>Third</td>
      <td>woman</td>
      <td>False</td>
      <td>NaN</td>
      <td>Southampton</td>
      <td>yes</td>
      <td>True</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>1</td>
      <td>female</td>
      <td>35.0</td>
      <td>1</td>
      <td>0</td>
      <td>53.1000</td>
      <td>S</td>
      <td>First</td>
      <td>woman</td>
      <td>False</td>
      <td>C</td>
      <td>Southampton</td>
      <td>yes</td>
      <td>False</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>3</td>
      <td>male</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>8.0500</td>
      <td>S</td>
      <td>Third</td>
      <td>man</td>
      <td>True</td>
      <td>NaN</td>
      <td>Southampton</td>
      <td>no</td>
      <td>True</td>
    </tr>
  </tbody>
</table>
</div>




```python
T = titanic[titanic.age.notnull()].copy()
```


```python
T.age.apply(lambda x: 60 if x>=60 else x)
T.age.value_counts()
```




    24.00    30
    22.00    27
    60.00    26
    18.00    26
    28.00    25
    30.00    25
    19.00    25
    21.00    24
    25.00    23
    36.00    22
    29.00    20
    35.00    18
    32.00    18
    27.00    18
    26.00    18
    31.00    17
    16.00    17
    34.00    15
    20.00    15
    33.00    15
    23.00    15
    39.00    14
    40.00    13
    17.00    13
    42.00    13
    45.00    12
    38.00    11
    4.00     10
    50.00    10
    2.00     10
             ..
    8.00      4
    5.00      4
    11.00     4
    6.00      3
    7.00      3
    46.00     3
    30.50     2
    57.00     2
    0.83      2
    55.00     2
    10.00     2
    59.00     2
    13.00     2
    28.50     2
    40.50     2
    45.50     2
    0.75      2
    32.50     2
    34.50     1
    55.50     1
    0.92      1
    36.50     1
    12.00     1
    53.00     1
    14.50     1
    0.67      1
    20.50     1
    23.50     1
    24.50     1
    0.42      1
    Name: age, Length: 77, dtype: int64




```python
Age = 10*(T["age"]//10)
Age = Age.astype(int)
Age.head()
Age.value_counts()
```




    20    220
    30    167
    10    102
    40     89
    0      62
    50     48
    60     26
    Name: age, dtype: int64




```python
Age.astype(str)+"s"
```




    0      20s
    1      30s
    2      20s
    3      30s
    4      30s
    6      50s
    7       0s
    8      20s
    9      10s
    10      0s
    11     50s
    12     20s
    13     30s
    14     10s
    15     50s
    16      0s
    18     30s
    20     30s
    21     30s
    22     10s
    23     20s
    24      0s
    25     30s
    27     10s
    30     40s
    33     60s
    34     20s
    35     40s
    37     20s
    38     10s
          ... 
    856    40s
    857    50s
    858    20s
    860    40s
    861    20s
    862    40s
    864    20s
    865    40s
    866    20s
    867    30s
    869     0s
    870    20s
    871    40s
    872    30s
    873    40s
    874    20s
    875    10s
    876    20s
    877    10s
    879    50s
    880    20s
    881    30s
    882    20s
    883    20s
    884    20s
    885    30s
    886    20s
    887    10s
    889    20s
    890    30s
    Name: age, Length: 714, dtype: object




```python
T.groupby(["sex", Age])["survived"].mean().unstack()
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
      <th>age</th>
      <th>0</th>
      <th>10</th>
      <th>20</th>
      <th>30</th>
      <th>40</th>
      <th>50</th>
      <th>60</th>
    </tr>
    <tr>
      <th>sex</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>female</th>
      <td>0.633333</td>
      <td>0.755556</td>
      <td>0.722222</td>
      <td>0.833333</td>
      <td>0.687500</td>
      <td>0.888889</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>male</th>
      <td>0.593750</td>
      <td>0.122807</td>
      <td>0.168919</td>
      <td>0.214953</td>
      <td>0.210526</td>
      <td>0.133333</td>
      <td>0.136364</td>
    </tr>
  </tbody>
</table>
</div>




```python
T.age = Age
T.pivot_table("survived", index="sex", columns="age")
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
      <th>age</th>
      <th>0</th>
      <th>10</th>
      <th>20</th>
      <th>30</th>
      <th>40</th>
      <th>50</th>
      <th>60</th>
    </tr>
    <tr>
      <th>sex</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>female</th>
      <td>0.633333</td>
      <td>0.755556</td>
      <td>0.722222</td>
      <td>0.833333</td>
      <td>0.687500</td>
      <td>0.888889</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>male</th>
      <td>0.593750</td>
      <td>0.122807</td>
      <td>0.168919</td>
      <td>0.214953</td>
      <td>0.210526</td>
      <td>0.133333</td>
      <td>0.136364</td>
    </tr>
  </tbody>
</table>
</div>




```python
titanic.describe()
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
      <th>survived</th>
      <th>pclass</th>
      <th>age</th>
      <th>sibsp</th>
      <th>parch</th>
      <th>fare</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>891.000000</td>
      <td>891.000000</td>
      <td>714.000000</td>
      <td>891.000000</td>
      <td>891.000000</td>
      <td>891.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>0.383838</td>
      <td>2.308642</td>
      <td>29.699118</td>
      <td>0.523008</td>
      <td>0.381594</td>
      <td>32.204208</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.486592</td>
      <td>0.836071</td>
      <td>14.526497</td>
      <td>1.102743</td>
      <td>0.806057</td>
      <td>49.693429</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>0.420000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>0.000000</td>
      <td>2.000000</td>
      <td>20.125000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>7.910400</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>0.000000</td>
      <td>3.000000</td>
      <td>28.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>14.454200</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>1.000000</td>
      <td>3.000000</td>
      <td>38.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>31.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>1.000000</td>
      <td>3.000000</td>
      <td>80.000000</td>
      <td>8.000000</td>
      <td>6.000000</td>
      <td>512.329200</td>
    </tr>
  </tbody>
</table>
</div>




```python
titanic.groupby("sex")[["survived"]].mean()
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
      <th>survived</th>
    </tr>
    <tr>
      <th>sex</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>female</th>
      <td>0.742038</td>
    </tr>
    <tr>
      <th>male</th>
      <td>0.188908</td>
    </tr>
  </tbody>
</table>
</div>




```python
titanic.groupby("sex")["survived"].mean()
```




    sex
    female    0.742038
    male      0.188908
    Name: survived, dtype: float64




```python
titanic.groupby(["sex", "class"])["survived"].aggregate("mean").unstack()
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
      <th>class</th>
      <th>First</th>
      <th>Second</th>
      <th>Third</th>
    </tr>
    <tr>
      <th>sex</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>female</th>
      <td>0.968085</td>
      <td>0.921053</td>
      <td>0.500000</td>
    </tr>
    <tr>
      <th>male</th>
      <td>0.368852</td>
      <td>0.157407</td>
      <td>0.135447</td>
    </tr>
  </tbody>
</table>
</div>



* 数据透视表：用更直观的方式实现上面的功能。


```python
titanic.pivot_table("survived", index="sex", columns="class") # 默认返回平均值
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
      <th>class</th>
      <th>First</th>
      <th>Second</th>
      <th>Third</th>
    </tr>
    <tr>
      <th>sex</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>female</th>
      <td>0.968085</td>
      <td>0.921053</td>
      <td>0.500000</td>
    </tr>
    <tr>
      <th>male</th>
      <td>0.368852</td>
      <td>0.157407</td>
      <td>0.135447</td>
    </tr>
  </tbody>
</table>
</div>




```python
titanic.pivot_table("survived", index="sex", columns="class", aggfunc="mean", margins=True) # aggfunc="mean"即为默认值 margins=True 会加一个总的列和总的行。
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
      <th>class</th>
      <th>First</th>
      <th>Second</th>
      <th>Third</th>
      <th>All</th>
    </tr>
    <tr>
      <th>sex</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>female</th>
      <td>0.968085</td>
      <td>0.921053</td>
      <td>0.500000</td>
      <td>0.742038</td>
    </tr>
    <tr>
      <th>male</th>
      <td>0.368852</td>
      <td>0.157407</td>
      <td>0.135447</td>
      <td>0.188908</td>
    </tr>
    <tr>
      <th>All</th>
      <td>0.629630</td>
      <td>0.472826</td>
      <td>0.242363</td>
      <td>0.383838</td>
    </tr>
  </tbody>
</table>
</div>




```python
titanic.pivot_table(index="sex", columns="class", aggfunc={"survived": "sum", "fare": "mean"}) # 要处理的那一列和要处理的方法组成一个键值对。
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead tr th {
        text-align: left;
    }
    
    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="3" halign="left">fare</th>
      <th colspan="3" halign="left">survived</th>
    </tr>
    <tr>
      <th>class</th>
      <th>First</th>
      <th>Second</th>
      <th>Third</th>
      <th>First</th>
      <th>Second</th>
      <th>Third</th>
    </tr>
    <tr>
      <th>sex</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>female</th>
      <td>106.125798</td>
      <td>21.970121</td>
      <td>16.118810</td>
      <td>91</td>
      <td>70</td>
      <td>72</td>
    </tr>
    <tr>
      <th>male</th>
      <td>67.226127</td>
      <td>19.741782</td>
      <td>12.661633</td>
      <td>45</td>
      <td>17</td>
      <td>47</td>
    </tr>
  </tbody>
</table>
</div>



## 11.7 其他

**（1）向量化字符串操作**

**（2） 处理时间序列**

**（3） 多级索引：用于多维数据**


```python
base_data = np.array([[1771, 11115 ],
                      [2154, 30320],
                      [2141, 14070],
                      [2424, 32680],
                      [1077, 7806],
                      [1303, 24222],
                      [798, 4789],
                      [981, 13468]]) 
data = pd.DataFrame(base_data, index=[["BeiJing","BeiJing","ShangHai","ShangHai","ShenZhen","ShenZhen","HangZhou","HangZhou"]\
                                     , [2008, 2018]*4], columns=["population", "GDP"])
data
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
      <th></th>
      <th>population</th>
      <th>GDP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">BeiJing</th>
      <th>2008</th>
      <td>1771</td>
      <td>11115</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>2154</td>
      <td>30320</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">ShangHai</th>
      <th>2008</th>
      <td>2141</td>
      <td>14070</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>2424</td>
      <td>32680</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">ShenZhen</th>
      <th>2008</th>
      <td>1077</td>
      <td>7806</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>1303</td>
      <td>24222</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">HangZhou</th>
      <th>2008</th>
      <td>798</td>
      <td>4789</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>981</td>
      <td>13468</td>
    </tr>
  </tbody>
</table>
</div>




```python
data.index.names = ["city", "year"]
data
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
      <th></th>
      <th>population</th>
      <th>GDP</th>
    </tr>
    <tr>
      <th>city</th>
      <th>year</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">BeiJing</th>
      <th>2008</th>
      <td>1771</td>
      <td>11115</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>2154</td>
      <td>30320</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">ShangHai</th>
      <th>2008</th>
      <td>2141</td>
      <td>14070</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>2424</td>
      <td>32680</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">ShenZhen</th>
      <th>2008</th>
      <td>1077</td>
      <td>7806</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>1303</td>
      <td>24222</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">HangZhou</th>
      <th>2008</th>
      <td>798</td>
      <td>4789</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>981</td>
      <td>13468</td>
    </tr>
  </tbody>
</table>
</div>




```python
data["GDP"]
```




    city      year
    BeiJing   2008    11115
              2018    30320
    ShangHai  2008    14070
              2018    32680
    ShenZhen  2008     7806
              2018    24222
    HangZhou  2008     4789
              2018    13468
    Name: GDP, dtype: int32




```python
data.loc["ShangHai", "GDP"]
```




    year
    2008    14070
    2018    32680
    Name: GDP, dtype: int32




```python
data.loc["ShangHai", 2018]["GDP"]
```




    32680



**（4） 高性能的Pandas：eval（）**


```python
df1, df2, df3, df4 = (pd.DataFrame(np.random.random((10000,100))) for i in range(4))
```


```python
%timeit (df1+df2)/(df3+df4)
```

    17.6 ms ± 120 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)


* 减少了复合代数式计算中间过程的内存分配


```python
%timeit pd.eval("(df1+df2)/(df3+df4)")
```

    10.5 ms ± 153 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)



```python
np.allclose((df1+df2)/(df3+df4), pd.eval("(df1+df2)/(df3+df4)"))
```




    True



* 实现列间运算


```python
df = pd.DataFrame(np.random.random((1000, 3)), columns=list("ABC"))
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.418071</td>
      <td>0.381836</td>
      <td>0.500556</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.059432</td>
      <td>0.749066</td>
      <td>0.302429</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.489147</td>
      <td>0.739153</td>
      <td>0.777161</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.175441</td>
      <td>0.016556</td>
      <td>0.348979</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.766534</td>
      <td>0.559252</td>
      <td>0.310635</td>
    </tr>
  </tbody>
</table>
</div>




```python
res_1 = pd.eval("(df.A+df.B)/(df.C-1)")
```


```python
res_2 = df.eval("(A+B)/(C-1)")
```


```python
np.allclose(res_1, res_2)
```




    True




```python
df["D"] = pd.eval("(df.A+df.B)/(df.C-1)")
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
      <td>0.418071</td>
      <td>0.381836</td>
      <td>0.500556</td>
      <td>-1.601593</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.059432</td>
      <td>0.749066</td>
      <td>0.302429</td>
      <td>-1.159019</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.489147</td>
      <td>0.739153</td>
      <td>0.777161</td>
      <td>-5.512052</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.175441</td>
      <td>0.016556</td>
      <td>0.348979</td>
      <td>-0.294917</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.766534</td>
      <td>0.559252</td>
      <td>0.310635</td>
      <td>-1.923199</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.eval("D=(A+B)/(C-1)", inplace=True)
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
      <td>0.418071</td>
      <td>0.381836</td>
      <td>0.500556</td>
      <td>-1.601593</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.059432</td>
      <td>0.749066</td>
      <td>0.302429</td>
      <td>-1.159019</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.489147</td>
      <td>0.739153</td>
      <td>0.777161</td>
      <td>-5.512052</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.175441</td>
      <td>0.016556</td>
      <td>0.348979</td>
      <td>-0.294917</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.766534</td>
      <td>0.559252</td>
      <td>0.310635</td>
      <td>-1.923199</td>
    </tr>
  </tbody>
</table>
</div>



* 使用局部变量


```python
column_mean = df.mean(axis=1)
res = df.eval("A+@column_mean")
res.head()
```




    0    0.342788
    1    0.047409
    2   -0.387501
    3    0.236956
    4    0.694839
    dtype: float64



**（4） 高性能的Pandas：query（）**


```python
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
      <td>0.418071</td>
      <td>0.381836</td>
      <td>0.500556</td>
      <td>-1.601593</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.059432</td>
      <td>0.749066</td>
      <td>0.302429</td>
      <td>-1.159019</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.489147</td>
      <td>0.739153</td>
      <td>0.777161</td>
      <td>-5.512052</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.175441</td>
      <td>0.016556</td>
      <td>0.348979</td>
      <td>-0.294917</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.766534</td>
      <td>0.559252</td>
      <td>0.310635</td>
      <td>-1.923199</td>
    </tr>
  </tbody>
</table>
</div>




```python
%timeit df[(df.A < 0.5) & (df.B > 0.5)]
```

    1.11 ms ± 9.38 µs per loop (mean ± std. dev. of 7 runs, 1000 loops each)



```python
%timeit df.query("(A < 0.5)&(B > 0.5)")
```

    2.55 ms ± 199 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)



```python
df.query("(A < 0.5)&(B > 0.5)").head()
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
      <th>1</th>
      <td>0.059432</td>
      <td>0.749066</td>
      <td>0.302429</td>
      <td>-1.159019</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.489147</td>
      <td>0.739153</td>
      <td>0.777161</td>
      <td>-5.512052</td>
    </tr>
    <tr>
      <th>7</th>
      <td>0.073950</td>
      <td>0.730144</td>
      <td>0.646190</td>
      <td>-2.272672</td>
    </tr>
    <tr>
      <th>10</th>
      <td>0.393200</td>
      <td>0.610467</td>
      <td>0.697096</td>
      <td>-3.313485</td>
    </tr>
    <tr>
      <th>11</th>
      <td>0.065734</td>
      <td>0.764699</td>
      <td>0.179380</td>
      <td>-1.011958</td>
    </tr>
  </tbody>
</table>
</div>




```python
np.allclose(df[(df.A < 0.5) & (df.B > 0.5)], df.query("(A < 0.5)&(B > 0.5)"))
```




    True



**（5）eval（）和query（）的使用时机**

小数组时，普通方法反而更快


```python
df.values.nbytes
```




    32000




```python
df1.values.nbytes
```




    8000000





[返回首页](https://github.com/timerring/pytorch-for-beginners)
