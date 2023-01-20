- [CH2 - 基本数据类型](#ch2---基本数据类型)
- [第一部分 数字类型](#第一部分-数字类型)
  - [1.1 数字类型的组成](#11-数字类型的组成)
    - [1.1.1 整数——不同进制的转换](#111-整数不同进制的转换)
    - [1.1.2 浮点数——不确定性](#112-浮点数不确定性)
    - [1.1.3 复数——a+bj](#113-复数abj)
  - [1.2 数字运算操作符（a 操作符 b）](#12-数字运算操作符a-操作符-b)
  - [1.3 数字运算操作函数   function(x, ...)](#13-数字运算操作函数--functionx-)
- [第二部分 字符串类型](#第二部分-字符串类型)
  - [2.1 字符串的表达](#21-字符串的表达)
  - [2.2 字符串的性质](#22-字符串的性质)
    - [2.2.1 字符串的索引](#221-字符串的索引)
    - [2.2.2 字符串的切片](#222-字符串的切片)
  - [2.3 字符串操作符](#23-字符串操作符)
    - [2.3.1 字符串的拼接](#231-字符串的拼接)
    - [2.3.2 字符串的成倍复制](#232-字符串的成倍复制)
    - [2.2.3 成员运算](#223-成员运算)
  - [2.4 字符串处理函数](#24-字符串处理函数)
    - [2.4.1 字符串的长度](#241-字符串的长度)
    - [2.4.2 字符编码](#242-字符编码)
  - [2.5 字符串的处理方法](#25-字符串的处理方法)
    - [2.5.1 字符串的分割——字符串.split(分割字符)](#251-字符串的分割字符串split分割字符)
    - [2.5.2  字符串的聚合——“聚合字符”.join(可迭代数据类型)](#252--字符串的聚合聚合字符join可迭代数据类型)
    - [3.5.3 删除两端特定字符——字符串.strip(删除字符)](#353-删除两端特定字符字符串strip删除字符)
    - [3.5.4 字符串的替换——字符串.replace("被替换"，"替换成")](#354-字符串的替换字符串replace被替换替换成)
    - [3.5.5 字符串统计——字符串.count("待统计字符串")](#355-字符串统计字符串count待统计字符串)
    - [3.3.6 字符串字母大小写](#336-字符串字母大小写)
- [第三部分 布尔类型 TRUE or False](#第三部分-布尔类型trueorfalse)
  - [3.1 逻辑运算的结果](#31-逻辑运算的结果)
  - [3.2 指示条件](#32-指示条件)
  - [3.3 作为numpy数组的掩码](#33-作为numpy数组的掩码)
- [第四部分 类型判别及类型转换](#第四部分-类型判别及类型转换)
  - [4.1 类型判别](#41-类型判别)
  - [4.2 类型转换](#42-类型转换)


# CH2 - 基本数据类型

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220908151914154.png)

# 第一部分 数字类型

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220908152001501.png)

## 1.1 数字类型的组成

### 1.1.1 整数——不同进制的转换

* 默认输入十进制 
* 二进制0b、八进制0o、十六进制0x


```python
16 == 0b10000 == 0o20 == 0x10
```


    True



* 十进制与其他进制的转换


```python
a = bin(16)   # 转二进制
b = oct(16)   # 转八进制
c = hex(16)   # 转十六进制
print(a, b, c)
```

    0b10000 0o20 0x10


**注意：上述转换后结果为字符串类型，因此如果进行相等比较的话，输出的是False结果**


```python
a == b == c
```


    False




```python
type(a)
```


    str



* 其他进制转十进制


```python
d = int(a, 2)      # 二进制转十进制
e = int(b, 8)      # 八进制转十进制
f = int(c, 16)     # 十六进制转十进制
print(d, e, f)
```

    16 16 16


### 1.1.2 浮点数——不确定性

* 不确定小数问题


```python
(0.1+0.2) == 0.3
```


    False




```python
0.1+0.2
```


    0.30000000000000004



**计算机采用二进制小数来表示浮点数的小数部分**
* 原因：部分小数不能用二进制小数完全表示

       二进制                  十进制
0.00011001100110011001   0.09999942779541016
0.0011001100110011      0.1999969482421875
0.01001100110011001     0.29999542236328125
0.01100110011001101     0.40000152587890625
0.1     ===    $1*2^{-1}$   ===         0.5

* 通常情况下不会影响计算精度


```python
0.1 + 0.7
```


    0.7999999999999999



* 四舍五入获得精确解
* 可以采用四舍五入的方式解决：round(parameter, 保留小数位数)


```python
a = 3*0.1
print(a)
```

    0.30000000000000004



```python
b = round(a, 1)
print(b)
b == 0.3
```

    0.3



    True



### 1.1.3 复数——a+bj

* 大写J或小写j均可


```python
3+4j
2+5J
```


    (2+5j)



* 虚部系数为1时，需要显式写出


```python
2+1j
```

## 1.2 数字运算操作符（a 操作符 b）

* 加减乘除运算 &ensp;  +&ensp;  -&ensp;  /&ensp;  *


```python
(1+3-4*2)/5
```


    -0.8



* 取反&ensp;  -


```python
x = 1
-x
```


    -1



* 乘方运算&ensp;  **


```python
2**3
```


    8



* 整数商//&ensp;  和&ensp;  模运算%


```python
13//5    # 整数商    x/y 向下取整数
```


    2




```python
13 % 5   # 模运算    余数 13=2*5+3
```

**几点说明**
* 整数与浮点数运算结果是浮点数
* 除法运算的结果是浮点数


```python
1+1.5
```


    2.5




```python
2/5
```


    0.4




```python
8/4
```


    2.0



## 1.3 数字运算操作函数&ensp;  function(x, ...)

* 求绝对值 abs()


```python
abs(-5)
```


    5




```python
abs(3+4j)  # 对复数a+bj 执行的是求模运算(a^2+b^2)^0.5
```


    5.0



* 幂次方 pow(x,n)
* 幂次方取模 pow(x,n,m)


```python
pow(2, 5)  # pow(x,n) x的n次方  等价于x**n
```


    32




```python
pow(2, 5, 3) # 2^5 % 3  更快速
```


    2



* 四舍五入 round(x,n)


```python
a = 1.618
print(round(a))      # 默认四舍五入为整数
```

    2



```python
print(round(a, 2))   # 参数2表示四舍五入后保留2位小数
```

    1.62



```python
print(round(a, 5))   # 位数不足，无需补齐
```

    1.618


* 整数商和模运算 divmod(x,y)

 等价于返回二元元组（x//y,x % y）


```python
divmod(13, 5)   # 较（x//y,x % y）更快，只执行了一次x/y
```


    (2, 3)



* 序列最大/最小值 max( )&ensp;  min( )


```python
max(3, 2, 3, 6, 9, 4, 5)
```


    9




```python
a = [3, 2, 3, 6, 9, 4, 5]
print("max:", max(a))
print("min:", min(a))
```

    max: 9
    min: 2


* 求和sum(x)

 注意：sum里面需要填入一个序列数据


```python
sum((1, 2, 3, 4, 5))
```


    15



* 借助科学计算库 math\scipy\numpy


```python
import math   # 导入库
print(math.exp(1))   # 指数运算 e^x
print(math.log2(2))  # 对数运算
print(math.sqrt(4))  # 开平方运算  等价于4^0.5
```

    2.718281828459045
    1.0
    2.0



```python
import numpy as np
a = [1, 2, 3, 4, 5]
print(np.mean(a))    # 求均值
print(np.median(a))  # 求中位数
print(np.std(a))     # 求标准差
```

    3.0
    3.0
    1.4142135623730951

# 第二部分 字符串类型

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220908152013247.png)

## 2.1 字符串的表达

* 用""或''括起来的任意字符


```python
print("Python")
print('Python')
```

    Python
    Python


* 字符串中有双引号或单引号的情况

**双中有单**


```python
print("I'm 18 years old")
```

    I'm 18 years old


**单中有双**


```python
print('"Python" is good')
```

    "Python" is good


**双中有双，单中有单**——转义符&ensp;  \


```python
# print(""Python" is good")
print("\"Python\" is good")  # \ 字符
```

    "Python" is good


**转义符可以用来换行继续输入**


```python
s = "py\
thon"                    
print(s)
```

    python


## 2.2 字符串的性质

### 2.2.1 字符串的索引


```python
s = "My name is Peppa Pig"
```

**变量名[位置编号]**
* 正向索引——从零开始递增
* 位置编号不能超过字符串的长度
* 空格也是一个位置


```python
print(s[0])   
print(s[2])
print(s[5])
```

    M
     
    m



```python
s = "My name is Peppa Pig"
```

* 反向索引——从-1开始递减


```python
print(s[-1])
print(s[-3])
print(s[-5])
```

    g
    P
    a


**索引只能获得一个字符，如何获得多个字符？**

### 2.2.2 字符串的切片

**变量名[开始位置：结束位置：切片间隔]**

* 切片间隔如不设置默认为1，可省略
* 切片范围不包含结束位置（前闭后开）


```python
s = "Python"
print(s[0:3:1])
```

    Pyt



```python
print(s[0:3])
```

    Pyt



```python
print(s[0:3:2])
```

    Pt


* 起始位置是0 可以省略
* 结束位置省略，代表可以取到最后一个字符
* 可以使用反向索引


```python
s = "Python"
print(s[0:6])
```

    Python



```python
print(s[:6])
```

    Python



```python
print(s[:])
```

    Python



```python
print(s[-6:])
```

    Python


**反向切片**
* 起始位置是-1也可以省略
* 结束位置省略，代表可以取到第一个字符
* **关键点在于-1**，代表前一个位置比后一个位置大-1


```python
s = "123456789"
print(s[-1:-10:-1])
```

    987654321



```python
print(s[:-10:-1])
```

    987654321



```python
print(s[::-1])
```

    987654321


## 2.3 字符串操作符

### 2.3.1 字符串的拼接

* 字符串1+字符串2


```python
a = "I love "
b = "my wife "
a+b        
```


    'I love my wife '



### 2.3.2 字符串的成倍复制

* 字符串&nbsp;&#42;&nbsp;n &ensp; n&nbsp;&#42;&nbsp;字符串


```python
c = a+b
print(c*3)
print(3*c)
```

    I love my wife I love my wife I love my wife 
    I love my wife I love my wife I love my wife 


### 2.2.3 成员运算

* **子集in全集**  &ensp;  任何一个**连续的切片**都是原字符串的子集


```python
folk_singers = "Peter, Paul and Mary"    
"Peter" in folk_singers
```


    True




```python
"PPM" in folk_singers
```


    False



* **遍历字符串字符** &ensp;  for 字符 in 字符串


```python
for s in "Python":
    print(s)
```

    P
    y
    t
    h
    o
    n


## 2.4 字符串处理函数

### 2.4.1 字符串的长度

* 所含字符的个数


```python
s = "python"
len(s)
```


    6



### 2.4.2 字符编码

**将中文字库，英文字母、数字、特殊字符等转化成计算机可识别的二进制数**

* 每个单一字符对应一个唯一的互不重复的二进制编码
* Python 中使用的是Unicode编码

**将字符转化为Unicode码**——ord(字符)


```python
print(ord("1"))
print(ord("a"))
print(ord("*"))
print(ord("中"))
print(ord("国"))
```

    49
    97
    42
    20013
    22269


**将Unicode码转化为字符**——chr(Unicode码)


```python
print(chr(1010))
print(chr(10000))
print(chr(12345))
print(chr(23456))
```

    ϲ
    ✐
    〹
    宠


## 2.5 字符串的处理方法

### 2.5.1 字符串的分割——字符串.split(分割字符)

* 返回一个**列表**
* 原字符串不变

**上述特性适合以下所有字符串处理方法**


```python
languages = "Python C C++ Java PHP R"
languages_list = languages.split(" ")#括号里的参数就是我们希望对目标字符串进行分割的标记
print(languages_list)
print(languages)
```

    ['Python', 'C', 'C++', 'Java', 'PHP', 'R']
    Python C C++ Java PHP R


### 2.5.2  字符串的聚合——“聚合字符”.join(可迭代数据类型)

* 可迭代类型 如：字符串、列表


```python
s = "12345"
s_join = ",".join(s) #把可迭代的对象每一个都取出来，相邻两个之间加上聚合字符
s_join
```


    '1,2,3,4,5'



* 序列类型的元素必须是字符类型


```python
# s = [1, 2, 3, 4, 5] 无法使用聚合
s = ["1", "2", "3", "4", "5"]
"*".join(s) 
```


    '1*2*3*4*5'



### 3.5.3 删除两端特定字符——字符串.strip(删除字符)

* strip从两侧开始搜索，遇到指定字符执行删除，遇到非指定字符，搜索停止
* 类似的还有左删除lstrip和右删除rstrip 


```python
s = "      I have many blanks     "
print(s.strip(" "))                        #从两端进行搜索，遇到指定字符后删除空格，然后停止
print(s.lstrip(" "))
print(s.rstrip(" "))
print(s)
```

    I have many blanks
    I have many blanks     
          I have many blanks
          I have many blanks     


### 3.5.4 字符串的替换——字符串.replace("被替换"，"替换成")


```python
s = "Python is coming"
s1 = s.replace("Python","Py")
print(s1)
```

    Py is coming


### 3.5.5 字符串统计——字符串.count("待统计字符串")


```python
s = "Python is an excellent language"
print("an:", s.count("an"))
print("e:", s.count("e"))
```

    an: 2
    e: 4


### 3.3.6 字符串字母大小写

* 字符串.upper() 字母全部大写


```python
s = "Python"
s.upper()
```


    'PYTHON'



* 字符串.lower() 字母全部小写


```python
print(s.lower())
print(s)
```

    python
    Python


* 字符串.title()首字母大写


```python
s.title()
```


    'Python'



# 第三部分 布尔类型&ensp;TRUE&ensp;or&ensp;False

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220908152026507.png)

## 3.1 逻辑运算的结果


```python
a = 10
print(a > 8)
print(a == 12)
print(a < 5)
```

    True
    False
    False


* any() 数据有一个是非零就为True
* all() 数据有一个是零就为False （元素都是非零的）


```python
print(any([False,1,0,None]))   # 0 False None 都是无
print(all([False,1,0,None]))
```

    True
    False


## 3.2 指示条件


```python
n = 2800
while True:
    m = eval(input("请输入一个正整数："))
    if m == n:
        print("正确")
        break
    elif m > n:
        print("太大了")
    else:
        print("太小了")  
```

    请输入一个正整数：280
    太小了
    请输入一个正整数：2800
    正确


## 3.3 作为numpy数组的掩码


```python
import numpy as np
x = np.array([[1, 3, 2, 5, 7]])   # 定义 numpy数组
print(x > 3)
x[x > 3]
```

    [[False False False  True  True]]
    
    array([5, 7])



# 第四部分 类型判别及类型转换

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220908152036300.png)

## 4.1 类型判别

* type(变量)


```python
age = 20
name = "Ada"
print(type(age))
print(type(name))
```

    <class 'int'>
    <class 'str'>


* isinstance(变量,预判类型)&ensp;**承认继承**
* 变量类型是预判类型的子类型，则为真，否则为假


```python
print(isinstance(age, int))        # 承认继承 这里的int就相当于是一个类
```

    True



```python
print(isinstance(age, object))
print(isinstance(name, object))    # object 是所有类的老祖宗
```

    True
    True


* 字符串检查方法

**字符串.isdigit()字符是否只有数字组成**


```python
age = "20"
name = "Ada"
```


```python
age.isdigit()
```


    True




```python
name.isdigit()
```


    False



**字符串.isalpha()字符是否只有字母组成**


```python
name.isalpha()
```


    True




```python
age.isalpha()
```


    False



**字符串.isalnum()字符是否只有数字和字母组成**


```python
"Ada20".isalnum()    # 比如可用于判断用户名是否合法
```


    True



## 4.2 类型转换

* 数字类型转字符串&ensp; str(数字类型)


```python
age = 20
print("My age is "+str(age))
```

    My age is 20


* 仅有数字组成的字符串转数字&ensp; int()&ensp; float()&ensp; eval()


```python
s1 = "20"
s2 = "10.1"
```


```python
int(s1)     # 仅整型
# int(s2) 会错误
```


    20




```python
float(s1)
```


    20.0




```python
float(s2)
```


    10.1




```python
eval(s1)
```


    20




```python
eval(s2)
```


    10.1

