- [CH5 函数](#ch5-函数)
  - [函数的参数](#函数的参数)
    - [参数传递](#参数传递)
      - [形参与实参](#形参与实参)
      - [位置参数](#位置参数)
      - [关键字参数](#关键字参数)
      - [默认参数](#默认参数)
      - [可变长参数 `*args`](#可变长参数-args)
      - [可变长参数 `**kwargs`](#可变长参数-kwargs)
      - [可变长参数的组合使用](#可变长参数的组合使用)
    - [函数体与变量作用域](#函数体与变量作用域)
    - [返回值](#返回值)
      - [单个返回值](#单个返回值)
      - [多个返回值——以元组的形式](#多个返回值以元组的形式)
      - [没有return语句，默认返回值为None](#没有return语句默认返回值为none)
    - [建议](#建议)
  - [进行单元测试 用 `assert` ——断言](#进行单元测试-用-assert-断言)
  - [匿名函数](#匿名函数)


# CH5 函数

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220925215610422.png)

## 函数的参数

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220925215624577.png)


### 参数传递

#### 形参与实参

* 形参（形式参数）：函数定义时的参数，实际上就是变量名
* 实参（实际参数）：函数调用时的参数，实际上就是变量的值

#### 位置参数

* 严格按照位置顺序，用实参对形参进行赋值
* 实参与形参个数必须一一对应，**一个不能多，一个不能少**

```python
def function(x, y, z):
    print(x, y, z)

function(1, 2, 3) # x = 1; y = 2; z = 3
```

#### 关键字参数

* 打破位置限制，直呼其名的进行值的传递（形参=实参）

```python
def function(x, y, z):
    print(x, y, z)

function(y=1, z=2, x=3) # x = 1; y = 2; z = 3
```

* 位置参数可以与关键字参数混合使用，**位置参数必须放在关键字参数前面**

```python
function(1, z=2, y=3)
```

#### 默认参数

* 在定义阶段就给形参赋值——该形参的常用值
* **默认参数必须放在非默认参数后面**
* **调用函数时，可以不对该形参传值**

```python
def register(name, age, sex="male"):
    print(name, age, sex)

register("timerring", 18)
```


* 默认参数应该设置为不可变类型（数字、字符串、元组）


```python
def function(ls=[]):
    print(id(ls))
    ls.append(1)
    print(id(ls))
    print(ls)


function()
# 1759752744328
# 1759752744328
# [1]
function()
# 1759752744328
# 1759752744328
# [1, 1]
```

**由上可见，列表的地址并没有发生变化。每次操作就是在原地址列表上进行操作，内容发生了变化，因此就好像有了记忆功能一样。因为默认参数被设置为列表这种可变类型。**


```python
def function(ls="Python"):
    print(id(ls))
    ls += "3.7"
    print(id(ls))
    print(ls)
    
    
function()
# 1759701700656
# 1759754352240
# Python3.7
function()
# 1759701700656
# 1759754353328
# Python3.7
```

**不会产生“记忆功能”，每次增量后到新的地址**

#### 可变长参数 `*args`

* 不知道会传过来多少参数  `*args`
* **该形参必须放在参数列表的最后**

```python
def foo(x, y, z, *args):
    print(x, y ,z)
    print(args)

foo(1, 2, 3, 4, 5, 6)    # 多余的参数，打包传递给args
# 1 2 3
# (4, 5, 6)
```

* 实参打散

```python
def foo(x, y, z, *args):
    print(x, y ,z)
    print(args)

foo(1, 2, 3, [4, 5, 6])    #列表被打包成元组赋值给args
# 1 2 3
# ([4, 5, 6],)
foo(1, 2, 3, *[4, 5, 6])   # *将这些列表打散，打散的是列表、字符串、元组或集合
# 1 2 3
# (4, 5, 6)
```

#### 可变长参数 `**kwargs`


```python
def foo(x, y, z, **kwargs):
    print(x, y ,z)
    print(kwargs)
        
foo(1, 2, 3, a=4, b=5, c=6)    #  多余的参数，以字典的形式打包传递给kwargs
# 1 2 3
# {'a': 4, 'b': 5, 'c': 6}
```

* 字典实参打散

```python
def foo(x, y, z, **kwargs):
    print(x, y ,z)
    print(kwargs)
    
foo(1, 2, 3, **{"a": 4, "b": 5, "c":6})
# 1 2 3
# {'a': 4, 'b': 5, 'c': 6}
```

#### 可变长参数的组合使用


```python
def foo(*args, **kwargs):
    print(args)
    print(kwargs)
    
    
foo(1, 2, 3, a=4, b=5, c=6)
# (1, 2, 3)
# {'a': 4, 'b': 5, 'c': 6}
```

### 函数体与变量作用域

* 函数体就是一段只在函数被调用时，才会执行的代码，代码构成与其他代码并无不同  
* **局部变量**——仅在函数体内定义和发挥作用
* **全局变量**——外部定义的都是全局变量，全局变量可以在函数体内直接被使用
* 通过 `global` 在函数体内定义全局变量

```python
def multipy(x, y):
    global z
    z = x*y
    return z 

print(multipy(2, 9))
# 18
print(z)
# 18
```

### 返回值

#### 单个返回值

```python
def foo(x):
    return x**2
```

#### 多个返回值——以元组的形式

```python
def foo(x):
    return 1, x, x**2, x**3    # 逗号分开，打包返回

print(foo(3))
# (1, 3, 9, 27)

a, b , c, d = foo(3)       # 解包赋值
print(a) # 1
print(b) # 3
print(c) # 9
print(d) # 27
```

#### 没有return语句，默认返回值为None

### 建议

1. 函数及其参数的命名参照变量的命名：字母小写及下划线组合
2. 应包含简要阐述函数功能的注释，注释紧跟函数定义后面

```python
def foo():
    # 这个函数的作用是为了。
    pass
```

3. 函数定义前后各空两行

```python
def f1():
    pass

                 # 空出两行，以示清白
def f2():
    pass
```

4. 默认参数赋值等号两侧不需加空格

## 进行单元测试 用 `assert` ——断言

* assert expression
* 表达式结果为 false 的时候触发异常

```python
assert game_over(21, 8) == False

---------------------------------------------------------------------------
AssertionError                            Traceback (most recent call last)

<ipython-input-42-88b651626036> in <module>
----> 4 assert game_over(21, 8) == False

AssertionError: 
```

## 匿名函数

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220925215654668.png)

`lambda 变量: 函数体`

在参数列表中最适合使用匿名函数，尤其是与 `key = lambda 入参: 函数体` 搭配

* 排序 `sort()` `sorted()`


```python
ls = [(93, 88), (79, 100), (86, 71), (85, 85), (76, 94)]
ls.sort(key = lambda x: x[1])# 根据每个元组的第二个数据进行排序
ls
# [(86, 71), (85, 85), (93, 88), (76, 94), (79, 100)]
ls = [(93, 88), (79, 100), (86, 71), (85, 85), (76, 94)]
temp = sorted(ls, key = lambda x: x[0]+x[1], reverse=True)# 得到降序的排序
temp
# [(93, 88), (79, 100), (85, 85), (76, 94), (86, 71)]
```

* `max()` `min()`


```python
ls = [(93, 88), (79, 100), (86, 71), (85, 85), (76, 94)]
n = max(ls, key = lambda x: x[1])
n
# (79, 100)
```

[返回首页](https://github.com/timerring/dive-into-AI)