- [CH1 基本语法元素](#ch1-基本语法元素)
- [1 数据类型](#1-数据类型)
  - [基本类型：数字、字符串、布尔](#基本类型数字字符串布尔)
  - [组合类型：列表、元组、字典、集合](#组合类型列表元组字典集合)
- [2 变量](#2-变量)
  - [变量的命名](#变量的命名)
  - [变量的赋值](#变量的赋值)
- [3 控制流程](#3-控制流程)
  - [循环（for)](#循环for)
  - [循环（while）](#循环while)
  - [分支（if）](#分支if)
- [4 输入输出](#4-输入输出)
  - [数据的输入](#数据的输入)
  - [数据的输出](#数据的输出)
    - [打印输出 print](#打印输出-print)
    - [换行控制 end=](#换行控制-end)
    - [组合输出](#组合输出)
    - [格式化输出方法 format](#格式化输出方法-format)
    - [修饰性输出](#修饰性输出)
  - [总结](#总结)
- [5 程序格式（PEP8格式）](#5-程序格式pep8格式)
  - [行最大长度](#行最大长度)
  - [缩进](#缩进)
  - [使用空格](#使用空格)
  - [避免使用空格](#避免使用空格)
  - [注释](#注释)


# CH1 基本语法元素

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220829104924361.png)

# 1 数据类型

## 基本类型：数字、字符串、布尔

数字类型

* int 整型
* float 浮点型
* complex 复数 a+bj

字符串类型

* str 字符串: 用" " 或' '

布尔类型

* bool 布尔类型

```python
y = 2 < 1
y
# False
```

## 组合类型：列表、元组、字典、集合

list 列表，**有序**

```python
a = [1, 2, 3, 4, 5]
a[4]
# 5
```
tuple 元组，**有序**，元素不支持修改

```python
b = (1, 2, 3, 4, 5)
b[0]
# 1
```

dict 字典，通过“键”-“值”的映射实现数据存储和查找，内部是无序的。

```python
student = {201901: "小明", 201902: "小红", 201903: "小强"}
student[201902]
# '小红'
```

set 集合，**一系列互不相等元素的集合，无序的**

```python
s = {"小明", "小红", "小强", "小明"}
s
# {'小强', '小明', '小红'}
```

# 2 变量

## 变量的命名

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220829174853368.png)

哪些可以用来做变量名？

* 大写字母、小写字母、数字、下划线、汉字及其组合。
* 严格区分大小写

哪些情况不被允许？

* 首字符不允许为数字
* 变量名中间不能有空格
* 不能与33个Pyhton保留字相同
![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220829175006026.png)

变量名定义技巧

* 下划线（变量和函数名）
**变量名由多个单词组成：用_连接多个单词**

```python
age_of_students = [17, 18, 19]
```

* 驼峰体（类名） **变量名由多个单词组成：单词首字母大写**

```python
AgeOfStudents
```

* 常量（如$\pi$、e） **变量名所有字母均为大写**


```python
MAX_ITERATION = 1000
```

## 变量的赋值

```python
x, y = 1, 2 # 通过","隔开
print(x, y)
x, y = y, x
print(x, y)
```

# 3 控制流程

## 循环（for)

```python
res = 0
for i in [1,2,3,4,5]:
    res += i
res
# 15
```

## 循环（while）

```python
i = 1
res = 0
while i <= 5:
    res += i
    i += 1
res
# 15
```

## 分支（if）

```python
if 判断条件：
    条件为真，执行语句
else:
    条件为假，执行语句
```

# 4 输入输出

## 数据的输入

外部文件导入

* 从本地硬盘、网络端读入等，见《文件、异常和模块》。

动态交互输入 input

```python
x = input("please input: ")
type(x)
# str 因此直接相加是字符串拼接
```

可以采用 `eval()` 去掉引号

```python
x = eval(input("please input: "))
type(x)
# int
```

## 数据的输出

### 打印输出 print

每个 print() 默认换行

```python
print("timerring")
# timerring
print(1)
# 1
```

### 换行控制 end=

```python
print(123,end=" ")#也可以自定义end的内容
print(456)
# 123 456
```

### 组合输出

```python
PI = 3.1415926
E = 2.71828
print("PI = ", PI, "E = ", E)
```

### 格式化输出方法 format

```python
# 一一对应
print("PI = {0},E = {1}".format(PI, E))
# PI = 3.1415926,E = 2.71828
print("PI = {0},E = {0}".format(PI, E))
# PI = 3.1415926,E = 3.1415926
# 默认按照顺序
print("PI = {},E = {}".format(PI, E))
# PI = 3.1415926,E = 2.71828
```

### 修饰性输出

填充输出

```python
print("{0:_^20}".format(PI))#这里的0也是后面PI变量 :表示修饰输出 _表示修饰字符 ^表示居中 20表示输出的宽度
# ____3.1415926_____  进行填充
print("{0:*<30}".format(PI))# <代表左对齐
# 3.1415926*********************
```

数字千分位分隔符

```python
print("{0:,}".format(10000000))
# 10,000,000
```


浮点数简化输出

* 留2位小数

```python
print("{0:.2f}".format(PI))
# 3.14
```

* 按百分数输出

```python
print("{0:.1%}".format(0.818727))
# 81.9%
```

* 科学计数法输出

```python
print("{0:.2e}".format(0.818727))
# 8.19e-01
```

整数的进制转换输出

* 十进制整数转二进制、unicode码、十进制、八进制、十六进制输出


```python
"二进制{0:b},Unicode码{0:c},十进制{0:d},八进制{0:o},十六进制{0:x}".format(450)
# 二进制11100010,Unicode码\u1b6,十进制450,八进制702,十六进制1c2
```

## 总结

* 格式化输出：`"字符{0:修饰}字符{1:修饰}字符".format(v0 ,v1)`

* 修饰输出方法：必须严格按照顺序来。

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220829175908182.png)

# 5 程序格式（PEP8格式）

## 行最大长度

**所有行限制的最大字符数为79**

## 缩进

* 用缩进来**表示语句间的逻辑**，缩进量：4字符

## 使用空格

* 二元运算符两边加一个空格
* 使用不同优先级的运算符，考虑在最低优先级的运算符周围添加空格

```python
x = x*2 - 1
c = (a+b) * (a-b)
```

* 在逗号后使用空格

## 避免使用空格

* 在制定关键字参数或者默认参数值的时候，不要在=附近加上空格

```python
def fun(n=1, m=2):
    print(n, m)
```

## 注释

* 单行注释 `# 注释内容`

* 多行注释 `"""注释内容，可分行"""`

[返回首页](https://github.com/timerring/dive-into-AI)
