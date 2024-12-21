- [CH2 基本数据类型](#ch2-基本数据类型)
  - [1.数字类型](#1数字类型)
    - [数字类型的组成](#数字类型的组成)
      - [整数——不同进制的转换](#整数不同进制的转换)
      - [浮点数——不确定性](#浮点数不确定性)
      - [复数——a+bj](#复数abj)
    - [数字运算操作符（a 操作符 b）](#数字运算操作符a-操作符-b)
    - [数字运算操作函数](#数字运算操作函数)
  - [2.字符串类型](#2字符串类型)
    - [字符串的表达](#字符串的表达)
    - [字符串的性质](#字符串的性质)
      - [字符串的索引 单个字符](#字符串的索引-单个字符)
      - [字符串的切片 多个字符](#字符串的切片-多个字符)
    - [字符串操作符](#字符串操作符)
      - [字符串的拼接](#字符串的拼接)
      - [字符串的成倍复制](#字符串的成倍复制)
      - [成员运算](#成员运算)
    - [字符串处理函数](#字符串处理函数)
      - [字符串的长度](#字符串的长度)
      - [字符编码](#字符编码)
    - [字符串的处理方法](#字符串的处理方法)
      - [字符串的分割 `split(" ")`](#字符串的分割-split-)
      - [字符串的聚合 `",".join(" ")`](#字符串的聚合-join-)
      - [删除两端特定字符 `strip(删除字符)`](#删除两端特定字符-strip删除字符)
      - [字符串的替换 `replace("被替换"，"替换成")`](#字符串的替换-replace被替换替换成)
      - [字符串统计 `count("待统计字符串")`](#字符串统计-count待统计字符串)
      - [字符串字母大小写及首字母大写 `.upper() .lower() .title()`](#字符串字母大小写及首字母大写-upper-lower-title)
  - [3.布尔类型](#3布尔类型)
    - [逻辑运算的结果](#逻辑运算的结果)
    - [作为numpy数组的掩码](#作为numpy数组的掩码)
  - [4.类型判别及类型转换](#4类型判别及类型转换)
    - [类型判别](#类型判别)
    - [字符串检查方法](#字符串检查方法)
    - [类型转换](#类型转换)


# CH2 基本数据类型

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220908151914154.png)

## 1.数字类型

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220908152001501.png)

### 数字类型的组成

#### 整数——不同进制的转换

* 默认输入十进制 
* 二进制0b、八进制0o、十六进制0x

```python
a = bin(16)   # 转二进制
b = oct(16)   # 转八进制
c = hex(16)   # 转十六进制
print(a, b, c)
# 0b10000 0o20 0x10
# Attention: str type
```

* 其他进制转十进制

```python
d = int(a, 2)      # 二进制转十进制
e = int(b, 8)      # 八进制转十进制
f = int(c, 16)     # 十六进制转十进制
print(d, e, f)
# 16 16 16
```

#### 浮点数——不确定性

* 不确定小数问题


```python
(0.1+0.2) == 0.3
# 0.30000000000000004
# False
```

**计算机采用二进制小数来表示浮点数的小数部分**
* 原因：部分小数不能用二进制小数完全表示

       二进制                  十进制
0.00011001100110011001   0.09999942779541016
0.0011001100110011      0.1999969482421875
0.01001100110011001     0.29999542236328125
0.01100110011001101     0.40000152587890625
0.1     ===    $1*2^{-1}$   ===         0.5

* 通常情况下不会影响计算精度
* 可以采用四舍五入的方式解决：round(parameter, 保留小数位数)


```python
a = 3*0.1
print(a)
# 0.30000000000000004
b = round(a, 1)
print(b)
# 0.3
b == 0.3
# True
```

#### 复数——a+bj

```python
# 大写J或小写j均可
3+4j
2+5J
# 虚部系数为1时，需要显式写出
2+1j
```

### 数字运算操作符（a 操作符 b）

* 加减乘除运算
* 取反 `-`
* 乘方运算 `**`
* 整数商 `//`: x/y 向下取整数
* 模运算 `%`: x/y 余数

**几点说明**
* 整数与浮点数运算结果是浮点数
* 除法运算的结果是浮点数 8/4 = 2.0

### 数字运算操作函数

* 求绝对值 abs()

```python
abs(3+4j)  # 对复数a+bj 执行的是求模运算(a^2+b^2)=0.5
# 5.0
```

* 幂次方 `pow(x,n)` 等价于 `x**n`
* 幂次方取模 `pow(x,n,m)` 等价于 `x**n % m`
* 四舍五入 `round(x,n)` n 为小数位数，默认无 n 时四舍五入为整数
* 整数商和模运算 `divmod(x,y)` 等价于返回二元元组 `(x//y,x % y)`
* 序列最大/最小值 `max( )` `min( )`

```python
a = [3, 2, 3, 6, 9, 4, 5]
print("max:", max(a))
print("min:", min(a))
# max: 9
# min: 2
```

* 求和 `sum(x)` 注意：sum里面需要填入一个序列数据


```python
sum((1, 2, 3, 4, 5))
# 15
```

* 借助科学计算库 `math\scipy\numpy`


```python
import math   # 导入库
print(math.exp(1))   # 指数运算 e^x
print(math.log2(2))  # 对数运算
print(math.sqrt(4))  # 开平方运算  等价于4^0.5
```

```python
import numpy as np
a = [1, 2, 3, 4, 5]
print(np.mean(a))    # 求均值
print(np.median(a))  # 求中位数
print(np.std(a))     # 求标准差
```

## 2.字符串类型

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220908152013247.png)

### 字符串的表达

* 用""或''括起来的任意字符，具体使用参考字符串中是否有双引号或单引号的情况。
* 若只想使用某一种，可以使用转义符 `\` 来实现。
```python
# print(""Python" is good") # False
print("\"Python\" is good")  # \ 字符
# "Python" is good
```

* 转义符可以用来换行继续输入

```python
s = "py\
thon"                    
print(s)
# python
```

### 字符串的性质

#### 字符串的索引 单个字符

**变量名[位置编号]**

* 正向索引——从 **0** 开始递增，空格也是一个位置
* 反向索引——从-1开始递减
* 位置编号不能超过字符串的长度

```python
s = "My name is Peppa Pig"
print(s[0])
# M 
print(s[2])
# 
print(s[5])
# m
print(s[-1])
# g
print(s[-3])
# P
print(s[-5])
# a
```

#### 字符串的切片 多个字符

**变量名[开始位置：结束位置：切片间隔]**

* 切片间隔如不设置默认为1，可省略
* 范围：前闭后开


```python
s = "Python"
print(s[0:3:1]) == print(s[0:3])
# Pyt
print(s[0:3:2])
# Pt
```

* 起始位置是0 可以省略
* 结束位置省略，代表可以取到最后一个字符

```python
s = "Python"
print(s[0:6]) == print(s[:6]) == print(s[:])
# Python
```

反向切片
* 起始位置是-1也可以省略
* 结束位置省略，代表可以取到第一个字符
* **关键点在于-1**，代表**前一个位置比后一个位置大-1**

```python
s = "123456789"
print(s[-1:-10:-1])
# 987654321
print(s[:-10:-1])
# 987654321
print(s[::-1])
# 987654321
```

### 字符串操作符

#### 字符串的拼接

* 字符串1+字符串2

#### 字符串的成倍复制

* 字符串*n

```python
c = a+b
print(c*3)
print(3*c)
```

#### 成员运算

* **子集in全集**：任何一个**连续的切片**都是原字符串的子集


```python
folk_singers = "Peter, Paul and Mary"    
"Peter" in folk_singers
# True
```

* **遍历字符串字符**：for 字符 in 字符串


```python
for s in "Python":
    print(s)
# P
# y
# t
# h
# o
# n
```

### 字符串处理函数

#### 字符串的长度

* 所含字符的个数 len(字符串)

#### 字符编码

将中文字库，英文字母、数字、特殊字符等转化成计算机可识别的二进制数

* 每个单一字符对应一个唯一的互不重复的二进制编码
* Python 中使用的是**Unicode编码**

`ord(字符)`：**将字符转化为Unicode码**


```python
print(ord("1"))
# 49
print(ord("a"))
# 97
```

`chr(Unicode码)`：**将Unicode码转化为字符**


```python
print(chr(1010))
# ϲ
print(chr(23456))
# 宠
```

### 字符串的处理方法

返回一个**列表**，原字符串不变

#### 字符串的分割 `split(" ")`

```python
languages = "Python C C++ Java PHP R"
languages_list = languages.split(" ")#括号里的参数就是我们希望对目标字符串进行分割的标记
print(languages_list)
# ['Python', 'C', 'C++', 'Java', 'PHP', 'R']
print(languages)
# Python C C++ Java PHP R
```

#### 字符串的聚合 `",".join(" ")`

* 可迭代类型 如：字符串、列表

```python
s = "12345"
s_join = ",".join(s) #把可迭代的对象每一个都取出来，相邻两个之间加上聚合字符
s_join
# '1,2,3,4,5'
```

* 序列类型的元素必须是字符类型

```python
# s = [1, 2, 3, 4, 5] 无法使用聚合
s = ["1", "2", "3", "4", "5"]
"*".join(s) 
# '1*2*3*4*5'
```

#### 删除两端特定字符 `strip(删除字符)`

* strip从两侧开始搜索，遇到指定字符执行删除，遇到非指定字符，搜索停止
* 类似的还有左删除`lstrip`和右删除`rstrip` 


```python
s = "      I have many blanks     "
print(s.strip(" ")) #从两端进行搜索，遇到指定字符后删除空格，然后停止
# I have many blanks
print(s.lstrip(" "))
# I have many blanks
print(s.rstrip(" "))
#       I have many blanks
```

#### 字符串的替换 `replace("被替换"，"替换成")`


```python
s = "Python is coming"
s1 = s.replace("Python","Py")
print(s1)
# Py is coming
```

#### 字符串统计 `count("待统计字符串")`


```python
s = "Python is an excellent language"
print("an:", s.count("an"))
# an: 2
```

#### 字符串字母大小写及首字母大写 `.upper() .lower() .title()`

```python
s = "Python"
print(s.upper())
# PYTHON
print(s.lower())
# python
print(s.title())
# Python
```

## 3.布尔类型

### 逻辑运算的结果

* any() 数据有一个是非零就为True
* all() 数据有一个是零就为False （全部非零）

```python
print(any([False,1,0,None]))   # 0 False None 都是无
# True
print(all([False,1,0,None]))
# False
```

###  作为numpy数组的掩码

```python
import numpy as np
x = np.array([[1, 3, 2, 5, 7]])   # 定义 numpy数组
print(x > 3)
# [[False False False  True  True]]
x[x > 3]
# array([5, 7])
```

## 4.类型判别及类型转换

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220908152036300.png)

### 类型判别

`type()`

```python
age = 20
name = "Ada"
print(type(age))
# <class 'int'>
print(type(name))
# <class 'str'>
```

`isinstance(变量, 预判类型)` **承认继承**
* 变量类型是预判类型的子类型，则为真，否则为假


```python
print(isinstance(age, int))        # 承认继承 这里的int就相当于是一个类
# True
print(isinstance(age, object)) # object 是所有类的老祖宗
# True
```

### 字符串检查方法

* `字符串.isdigit()` 字符是否只有数字组成
* `字符串.isalpha()` 字符是否只有字母组成
* `字符串.isalnum()` 字符是否只有数字和字母组成

### 类型转换

* 数字类型转字符串 `str(数字类型)`
* 仅有数字组成的字符串转数字 `int()` `float()` `eval()`


```python
s1 = "20"
int(s1)  # 仅整型 20
s2 = "10.1"
# int(s2) 会错误

float(s1)
# 20.0

eval(s2) # 无限制
# 10.1
```

[返回首页](https://github.com/timerring/dive-into-AI)