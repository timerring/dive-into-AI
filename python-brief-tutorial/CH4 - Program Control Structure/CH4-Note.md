- [CH4 - 程序控制结构](#ch4---程序控制结构)
  - [4.1 条件测试](#41-条件测试)
    - [4.1.1 比较运算](#411-比较运算)
    - [4.1.2 逻辑运算](#412-逻辑运算)
    - [4.1.3 存在运算](#413-存在运算)
  - [4.2 分支结构——if语句](#42-分支结构if语句)
    - [4.2.1 单分支](#421-单分支)
    - [4.2.2 二分支](#422-二分支)
    - [4.2.3  多分支](#423--多分支)
    - [4.2.4 嵌套语句](#424-嵌套语句)
  - [4.3 遍历循环——for 循环](#43-遍历循环for-循环)
    - [4.3.1 主要形式](#431-主要形式)
    - [4.3.2 执行过程](#432-执行过程)
    - [4.3.3 循环控制：break 和 continue](#433-循环控制break-和-continue)
    - [4.3.4 for 与 else的配合](#434-for-与-else的配合)
  - [4.4 无限循环——while 循环](#44-无限循环while-循环)
    - [4.4.1 为什么要用while 循环](#441-为什么要用while-循环)
    - [4.4.2 while循环的一般形式](#442-while循环的一般形式)
    - [主要形式：](#主要形式)
    - [4.4.3 while与风向标](#443-while与风向标)
    - [4.4.4 while 与循环控制 break、continue](#444-while-与循环控制-breakcontinue)
    - [4.4.5 while与else](#445-while与else)
    - [4.4.6 再看两个例子](#446-再看两个例子)
- [4.5 控制语句注意问题](#45-控制语句注意问题)
    - [4.5.1 尽可能减少多层嵌套](#451-尽可能减少多层嵌套)
    - [4.5.2 避免死循环](#452-避免死循环)
    - [4.5.3 封装过于复杂的判断条件](#453-封装过于复杂的判断条件)


# CH4 - 程序控制结构

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220912223804979.png)

## 4.1 条件测试

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220912223823056.png)

### 4.1.1 比较运算


```python
a = 10
b = 8
print(a > b)     # 大于
print(a < b)     # 小于
print(a >= b)    # 大于等于
print(a <= b)    # 小于等于
print(a == b)    # 等于
print(a != b)    # 不等于
```

    True
    False
    True
    False
    False
    True


* 非空


```python
ls = [1]
if ls:            # 数据结构不为空、变量不为0、None、False 则条件成立
    print("非空")
else:
    print("空的")
```

    非空


### 4.1.2 逻辑运算

* 与、或、非


```python
a = 10
b = 8
c = 12
print((a > b) and (b > c))    # 与
print((a > b) or (b > c))     # 或
print(not(a > b))             # 非
```

    False
    True
    False


* 复合逻辑运算的优先级  
非 > 与 > 或


```python
print(True or True and False)
```

    True



```python
print((True or True) and False)
```

    False


### 4.1.3 存在运算

元素 in 列表/字符串   


```python
cars = ["BYD", "BMW", "AUDI", "TOYOTA"]
```


```python
print("BMW" in cars)
print("BENZ" in cars)
```

    True
    False


元素 not in 列表/字符串   


```python
print("BMW" not in cars)
print("BENZ" not in cars)
```

    False
    True

## 4.2 分支结构——if语句

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220912223836014.png)

### 4.2.1 单分支

**模板**  
**注意有“：”**
    
if 条件：  
&emsp;&ensp;缩进的代码块


```python
age = 8
if age > 7:
    print("孩子，你该上学啦！")
```

    孩子，你该上学啦！


### 4.2.2 二分支

**模板**  
    
if 条件：  
&emsp;&ensp;缩进的代码块  
else：  
&emsp;&ensp;缩进的代码块  


```python
age = 6
if age > 7:
    print("孩子，你该上学啦！")
else:
    print("再玩两年泥巴！")
```

    再玩两年泥巴！


### 4.2.3  多分支

**模板**  
    
if 条件：  
&emsp;&ensp;缩进的代码块  
elif 条件：  
&emsp;&ensp;缩进的代码块  
elif 条件：  
&emsp;&ensp;缩进的代码块  
...  
else：  
&emsp;&ensp;缩进的代码块  


```python
age = 38
if age < 7:
    print("再玩两年泥巴")
elif age < 13:
    print("孩子，你该上小学啦")
elif age < 16:
    print("孩子，你该上初中了")
elif age < 19:
    print("孩子，你该上高中了")
elif age < 23:
    print("大学生活快乐")
elif age < 60:
    print("辛苦了，各行各业的工作者们")
else:     # 有时为了清楚，也可以写成elif age >= 60:
    print("享受退休生活吧") 
```

    辛苦了，各行各业的工作者们


**不管多少分支，最后只执行一个分支**

### 4.2.4 嵌套语句

**题目：年满18周岁，在非公共场合方可抽烟，判断某种情形下是否可以抽烟**


```python
age = eval(input("请输入年龄"))
if age > 18:
    is_public_place = bool(eval(input("公共场合请输入1，非公共场合请输入0")))
    print(is_public_place)
    if not is_public_place:
        print("可以抽烟")
    else:
        print("禁止抽烟")        
else:
    print("禁止抽烟")            
```

    请输入年龄19
    公共场合请输入1，非公共场合请输入01
    True
    禁止抽烟

## 4.3 遍历循环——for 循环

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220912223859509.png)

### 4.3.1 主要形式
* **for** 元素 **in** 可迭代对象：  
&emsp;&emsp;执行语句

### 4.3.2 执行过程
* 从可迭代对象中，依次取出每一个元素，并进行相应的操作

1、直接迭代——列表[ ]、元组( )、集合{ }、字符串" "


```python
graduates = ("李雷", "韩梅梅", "Jim")
for graduate in graduates:
    print("Congratulations, "+graduate)
```

    Congratulations, 李雷
    Congratulations, 韩梅梅
    Congratulations, Jim


2、变换迭代——字典


```python
students = {201901: '小明', 201902: '小红', 201903: '小强'}
for k, v in students.items():
    print(k, v)
for student in students.keys():
#for student in students 同样是迭代键key
    print(student)  
```

    201901 小明
    201902 小红
    201903 小强
    201901
    201902
    201903


3、range()对象


```python
res=[]
for i in range(10000):
    res.append(i**2)
print(res[:5])
print(res[-1])
```

    [0, 1, 4, 9, 16]
    99980001



```python
res = []
for i in range(1, 10, 2):
    res.append(i ** 2)
print(res)
```

    [1, 9, 25, 49, 81]


### 4.3.3 循环控制：break 和 continue

* break 结束整个循环


```python
product_scores = [89, 90, 99, 70, 67, 78, 85, 92, 77, 82] # 1组10个产品的性能评分
 # 如果低于75分的超过1个，则该组产品不合格
i = 0
for score in product_scores:
    if score < 75:
        i += 1 
    if i == 2:
        print("产品抽检不合格")
        break
```

    产品抽检不合格


* continue 结束本次循环


```python
product_scores = [89, 90, 99, 70, 67, 78, 85, 92, 77, 82] # 1组10个产品的性能评分
 # 如果低于75分,输出警示
print(len(product_scores))
for i in range(len(product_scores)):
    if product_scores[i] >= 75:
        continue 
    print("第{0}个产品，分数为{1}，不合格".format(i, product_scores[i]))
```

    10
    第3个产品，分数为70，不合格
    第4个产品，分数为67，不合格


### 4.3.4 for 与 else的配合

如果for 循环全部执行完毕，没有被break中止，则运行else块


```python
product_scores = [89, 90, 99, 70, 67, 78, 85, 92, 77, 82] # 1组10个产品的性能评分
 # 如果低于75分的超过1个，则该组产品不合格
i = 0
for score in product_scores:
    if score < 75:
        i+=1 
    if i == 2:
        print("产品抽检不合格")
        break
else:
    print("产品抽检合格")
```

    产品抽检不合格

## 4.4 无限循环——while 循环

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220912223924395.png)

###  4.4.1 为什么要用while 循环

* 经典题目：猜数字


```python
albert_age = 18
#第1次
guess = int(input(">>:"))
if guess > albert_age :
    print("猜的太大了")
elif guess < albert_age :
    print("猜的太小了")
else:
    print("猜对了")
#第2次
guess = int(input(">>:"))
if guess > albert_age :
    print("猜的太大了")
elif guess < albert_age :
    print("猜的太小了")
else:
    print("猜对了")
```

**代码可能需要重复执行，可是又不知道具体要执行多少次**

### 4.4.2 while循环的一般形式

### 主要形式：
* **while** 判断条件：  
&emsp;&emsp;执行语句   
**条件为真，执行语句**   
**条件为假，while 循环结束**


```python
albert_age = 18 
guess = int(input(">>:"))
while guess != albert_age: 
    if guess > albert_age :
        print("猜的太大了.")
    elif guess < albert_age :
        print("猜的太小了")
    guess = int(input(">>:"))
print("猜对了")
```

    >>:20
    猜的太大了.
    >>:18
    猜对了


### 4.4.3 while与风向标


```python
albert_age = 18 
flag = True     # 布尔类型
while flag:
    guess = int(input(">>:"))
    if guess > albert_age :
        print("猜的太大了")
    elif guess < albert_age :
        print("猜的太小了")
    else:
        print("猜对了")
        flag = False
```


```python
flag=True  
while flag:  
    pass  
    while flag:  
        pass  
        while flag:     
            flag=False     # 循环逐层判断，当flag为false时，循环会逐层退出
```

### 4.4.4 while 与循环控制 break、continue 


```python
albert_age = 18 
while True:
    guess = int(input(">>:"))
    if guess > albert_age :
        print("猜的太大了")
    elif guess < albert_age :
        print("猜的太小了")
    else:
        print("猜对了...")
        break    # 当诉求得到满足，就跳出循环
```

输出10以内的奇数


```python
i = 0
while i < 10:
    i += 1
    if i % 2 == 0:
        continue       # 跳出本次循环，进入下一次循环
    print(i)
```

    1
    3
    5
    7
    9


### 4.4.5 while与else

如果while 循环全部执行完毕，没有被break中止，而是条件不再满足了而中止，则运行else块


```python
count = 0
while count <= 5 :
    count += 1
    print("Loop",count)
else:
    print("循环正常执行完啦")
```

    Loop 1
    Loop 2
    Loop 3
    Loop 4
    Loop 5
    Loop 6
    循环正常执行完啦


### 4.4.6 再看两个例子

应用：删除列表中的特定值


```python
pets = ["dog", "cat", "dog", "pig", "goldfish", "rabbit", "cat"]
```


```python
while "cat" in pets:
    pets.remove("cat")
pets
```




    ['dog', 'dog', 'pig', 'goldfish', 'rabbit']



应用：将未读书籍列表中书名分别输出后，存入已读书籍列表


```python
not_read = ["红楼梦", "水浒传", "三国演义", "西游记"]
have_read = []
while not_read:     # not_read非空，循环继续，否则中止
    book = not_read.pop()
    have_read.append(book)
    print("我已经读过《{}》了".format(book))
print(not_read)
print(have_read)
```

    我已经读过《西游记》了
    我已经读过《三国演义》了
    我已经读过《水浒传》了
    我已经读过《红楼梦》了
    []
    ['西游记', '三国演义', '水浒传', '红楼梦']

# 4.5 控制语句注意问题

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220912223934332.png)

### 4.5.1 尽可能减少多层嵌套

* 可读性差，容易把人搞疯掉


```python
if 条件:
    执行语句
    if 条件：
        执行语句
        if...
```

### 4.5.2 避免死循环

条件一直成立，循环永无止境


```python
# while True:
    # print("欢迎订阅专栏")
```

### 4.5.3 封装过于复杂的判断条件

如果条件判断里的表达式过于复杂   

出现了太多的 not/and/or等   

导致可读性大打折扣

**考虑将条件封装为函数**


```python
a, b, c, d, e = 10, 8, 6, 2, 0
if (a > b) and (c >d) and (not e):
    print("过于复杂")
```

```python
numbers = (10, 8, 6, 2, 0)


def judge(num):
    a, b, c, d, e = num
    x = a > b
    y = c > d
    z = not e
    return x and y and z


if judge(numbers):
    print("简洁明了")
```



[返回首页](https://github.com/timerring/dive-into-AI)
