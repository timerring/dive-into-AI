- [CH4 - 程序控制结构](#ch4---程序控制结构)
  - [条件测试](#条件测试)
    - [逻辑运算](#逻辑运算)
    - [存在运算](#存在运算)
  - [分支结构——if语句](#分支结构if语句)
    - [简单版本](#简单版本)
    - [多分支](#多分支)
  - [遍历循环——for 循环](#遍历循环for-循环)
    - [执行过程](#执行过程)
    - [循环控制：break 和 continue](#循环控制break-和-continue)
    - [for 与 else 的配合](#for-与-else-的配合)
  - [while 循环](#while-循环)
    - [while与else](#while与else)


# CH4 - 程序控制结构

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220912223804979.png)

## 条件测试

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220912223823056.png)

### 逻辑运算

* 复合逻辑运算的优先级  
非 > 与 > 或

```python
print((a > b) and (b > c))    # 与
print((a > b) or (b > c))     # 或
print(not(a > b))             # 非
```

### 存在运算

元素 in 列表/字符串

## 分支结构——if语句

### 简单版本

```python
if age > 7:
    print("if")
else:
    print("else")
```

### 多分支

```python
if age < 7:
    print("7")
elif age < 13:
    print("13")
elif age < 60:
    print("60")
else: # 有时为了清楚，也可以写成elif age >= 60:
    print("60") 
```

**不管多少分支，最后只执行一个分支**

## 遍历循环——for 循环

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220912223859509.png)

### 执行过程
* 从可迭代对象中，依次取出每一个元素，并进行相应的操作

列表[ ]、元组( )、集合{ }、字符串" "

```python
graduates = ("apple", "google", "timerring")
for graduate in graduates:
    print("Congratulations, "+graduate)
```

字典

```python
students = {201901: 'apple', 201902: 'google', 201903: 'timerring'}
for k, v in students.items():
    print(k, v)
for student in students.keys():
#for student in students 同样是迭代键key
    print(student)  
```

range()对象

```python
res = []
for i in range(1, 10, 2):
    res.append(i ** 2)
print(res)
```

### 循环控制：break 和 continue

* break 结束整个循环
* continue 结束本次循环

### for 与 else 的配合

如果 for 循环全部执行完毕，没有被 break 中止，则运行 else 块


```python
product_scores = [89, 90, 99, 70, 67, 78, 85, 92, 77, 82]
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

## while 循环

### while与else

如果while 循环全部执行完毕，没有被break中止，而是条件不再满足了而中止，则运行else块


```python
count = 0
while count <= 2 :
    count += 1
    print("Loop",count)
else:
    print("over")
#     Loop 1
#     Loop 2
#     Loop 3
#     over
```

[返回首页](https://github.com/timerring/dive-into-AI)
