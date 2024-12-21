- [CH6 - 类——面向对象的编程](#ch6---类面向对象的编程)
  - [引子](#引子)
  - [类的定义](#类的定义)
    - [类的命名](#类的命名)
    - [类的属性](#类的属性)
    - [类的方法](#类的方法)
  - [创建实例](#创建实例)
    - [实例的创建](#实例的创建)
    - [访问属性](#访问属性)
    - [调用方法](#调用方法)
    - [修改属性](#修改属性)
      - [直接修改](#直接修改)
      - [通过方法修改属性](#通过方法修改属性)
  - [类的继承](#类的继承)
    - [简单的继承](#简单的继承)
    - [给子类添加属性和方法](#给子类添加属性和方法)
    - [重写父类的方法——多态](#重写父类的方法多态)
    - [用在类中的实例](#用在类中的实例)


# CH6 - 类——面向对象的编程

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220926185401409.png)

## 引子

* **一切对象，都有自己内在的属性**  
* **一切行为，皆是对象的行为**  

**How：类是对象的载体**

每一只猫都是一个对象，可以把一类对象的公共特征抽象出来，创建通用的类。

```python
# 创建类
class Cat():
    """模拟猫"""

    def __init__(self, name):
        """初始化属性"""
        self.name = name
    
    def jump(self):
        """模拟猫跳跃"""
        print(self.name + " is jumping") 

my_cat = Cat("Loser")
your_cat = Cat("Lucky")

print(my_cat.name) # Loser
print(your_cat.name) # Lucky
# Call method
my_cat.jump() # Loser is jumping
your_cat.jump() # Lucky is jumping
```

## 类的定义

**三要素：类名、属性、方法**

### 类的命名

* 驼峰命名法——组成的单词首字母大写  

```python
# class 类名：
"""类前空两行"""


class Car():
    """对该类的简单介绍"""
    pass

"""类后空两行"""
```

### 类的属性

```python
class Car():
    """模拟汽车"""
    # def __init__(self,要传递的参数)  初始化类的属性
    def __init__(self, brand, model, year):
        """初始化汽车属性"""               # 相当于类内部的变量
        self.brand = brand                 # 汽车的品牌
        self.model = model                 # 汽车的型号
        self.year = year                   # 汽车出厂年份
        self.mileage = 0                   # 新车总里程初始化为0        
```

### 类的方法

相对于类内部定义的函数

```python
class Car():
    """模拟汽车"""
    
    def __init__(self, brand, model, year):
        """初始化汽车属性"""        # 相当于类内部的变量
        self.brand = brand        # 汽车的品牌
        self.model = model        # 汽车的型号
        self.year = year          # 汽车出厂年份
        self.mileage = 0          # 新车总里程初始化为0  
        
    def get_main_information(self):  # self不能省
        """获取汽车主要信息"""
        print("品牌：{}   型号：{}   出厂年份：{}".format(self.brand, self.model, self.year))
    
    def get_mileage(self):
        """获取总里程"""
        return "行车总里程：{}公里".format(self.mileage)
```

## 创建实例

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220926185441340.png)

### 实例的创建

将实例赋值给对象，实例化过程中，传入相应的参数  

```python
my_new_car = Car("Audi", "A6", 2018)
```

### 访问属性

实例名.属性名

```python
print(my_new_car.brand) # Audi
print(my_new_car.model) # A6
print(my_new_car.year) # 2018
```

### 调用方法

实例名.方法名(必要的参数)


```python
my_new_car = Car("Audi", "A6", 2018)
my_new_car.get_main_information() # 品牌：Audi   型号：A6   出厂年份：2018
```

### 修改属性

#### 直接修改

先访问，后修改

```python
my_old_car.mileage = 12000
print(my_old_car.mileage) # 12000
```

#### 通过方法修改属性


```python
class Car():
    """模拟汽车"""
    
    def __init__(self, brand, model, year):
        """初始化汽车属性"""               # 相当于类内部的变量
        self.brand = brand                 # 汽车的品牌
        self.model = model                 # 汽车的型号
        self.year = year                   # 汽车出厂年份
        self.mileage = 0                   # 新车总里程初始化为0  
        
    def get_main_information(self):        # self不能省
        """获取汽车主要信息"""
        print("品牌：{}   型号：{}   出厂年份：{}".format(self.brand, self.model, self.year))
    
    def set_mileage(self, distance):
        """设置总里程数"""
        self.mileage = distance

my_old_car.set_mileage(8000)
```

可以创建无穷多的实例

```python
my_new_car = Car("Audi", "A6", 2018)
my_cars = [my_new_car, my_old_car]
```

## 类的继承

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220926185449121.png)

**所谓继承，就是低层抽象继承高层抽象的过程**

### 简单的继承

**父类**


```python
class Car():
    """模拟汽车"""
    
    def __init__(self, brand, model, year):
        """初始化汽车属性"""               # 相当于类内部的变量
        self.brand = brand                 # 汽车的品牌
        self.model = model                 # 汽车的型号
        self.year = year                   # 汽车出厂年份
        self.mileage = 0                   # 新车总里程初始化为0
        
        
    def get_main_information(self):        # self不能省
        """获取汽车主要信息"""
        print("品牌：{}   型号：{}   出厂年份：{}".format(self.brand, self.model, self.year))
    
    def get_mileage(self):
        """获取总里程数"""
        print("行车总里程：{}公里".format(self.mileage)) 
    
    def set_mileage(self, distance):
        """设置总里程数"""
        if distance >= 0:
            self.mileage = distance
        else:
            print("里程数不能为负！")
    
    def increment_mileage(self, distance):
        """总里程数累计"""
        if distance >= 0:
            self.mileage += distance
        else:
            print("新增里程数不能为负！")
```

**子类**  

class 子类名（父类名）：

* 新建一个电动汽车的类

```python
class ElectricCar(Car):
    """模拟电动汽车"""
    
    def __init__(self, brand, model, year):
        """初始化电动汽车属性"""
        super().__init__(brand, model, year)  # 声明继承父类的属性, 这里的super就是超类（父类）
```

* 自动继承父类的所有方法

```python
my_electric_car = ElectricCar("NextWeek", "FF91", 2046)
my_electric_car.get_main_information() # 品牌：NextWeek   型号：FF91   出厂年份：2046
```

### 给子类添加属性和方法

```python
class ElectricCar(Car):
    """模拟电动汽车"""
    
    def __init__(self, brand, model, year, bettery_size):# 新传入的参数bettery_size
        """初始化电动汽车属性"""
        super().__init__(brand, model, year)    # 声明继承父类的属性
        self.bettery_size = bettery_size        # 电池容量
        self.electric_quantity = bettery_size   # 电池剩余电量
        self.electric2distance_ratio = 5        # 电量距离换算系数 5公里/kW.h
        self.remainder_range = self.electric_quantity*self.electric2distance_ratio # 剩余可行驶里程
    
    def get_electric_quantit(self):
        """查看当前电池电量"""
        print("当前电池剩余电量：{} kW.h".format(self.electric_quantity))
        
    def set_electric_quantity(self, electric_quantity):
        """设置电池剩余电量，重新计算电量可支撑行驶里程"""
        if electric_quantity >= 0 and electric_quantity <= self.bettery_size:
            self.electric_quantity = electric_quantity
            self.remainder_range = self.electric_quantity*self.electric2distance_ratio
        else:
            print("电量未设置在合理范围！")
    
    def get_remainder_range(self):
        """查看剩余可行驶里程"""
        print("当前电量还可以继续驾驶 {} 公里".format(self.remainder_range))              
```


```python
my_electric_car = ElectricCar("NextWeek", "FF91", 2046, 70)
my_electric_car.get_electric_quantit() # 当前电池剩余电量：70 kW.h
my_electric_car.get_remainder_range()  # 当前电量还可以继续驾驶 350 公里
my_electric_car.set_electric_quantity(50)  # 重设电池电量
my_electric_car.get_electric_quantit() # 当前电池剩余电量：50 kW.h
my_electric_car.get_remainder_range() # 当前电量还可以继续驾驶 250 公里
```

### 重写父类的方法——多态

首先在子类的方法中找，如果找不到再去父类的方法中寻找。因此子类可以重写父类方法。

```python
class ElectricCar(Car):
    """模拟电动汽车"""
    
    def __init__(self, brand, model, year, bettery_size):
        """初始化电动汽车属性"""
        super().__init__(brand, model, year)    # 声明继承父类的属性
        self.bettery_size = bettery_size        # 电池容量
        self.electric_quantity = bettery_size   # 电池剩余电量
        self.electric2distance_ratio = 5        # 电量距离换算系数 5公里/kW.h
        self.remainder_range = self.electric_quantity*self.electric2distance_ratio # 剩余可行驶里程
    
    def get_main_information(self):        # 重写父类方法
        """获取汽车主要信息"""
        print("品牌：{}   型号：{}   出厂年份：{}   续航里程：{} 公里"
              .format(self.brand, self.model, self.year, self.bettery_size*self.electric2distance_ratio))


my_electric_car = ElectricCar("NextWeek", "FF91", 2046, 70)
my_electric_car.get_main_information() # 品牌：NextWeek   型号：FF91   出厂年份：2046   续航里程：350 公里
```


### 用在类中的实例

把电池抽象成一个对象。

```python
class Bettery():
    """模拟电动汽车的电池"""
    
    def __init__(self, bettery_size = 70):
        self.bettery_size = bettery_size        # 电池容量
        self.electric_quantity = bettery_size   # 电池剩余电量
        self.electric2distance_ratio = 5        # 电量距离换算系数 5公里/kW.h
        self.remainder_range = self.electric_quantity*self.electric2distance_ratio # 剩余可行驶里程

    def get_electric_quantit(self):
        """查看当前电池电量"""
        print("当前电池剩余电量：{} kW.h".format(self.electric_quantity))
        
    def set_electric_quantity(self, electric_quantity):
        """设置电池剩余电量，计重新算电量可支撑行驶里程"""
        if electric_quantity >= 0 and electric_quantity <= self.bettery_size:
            self.electric_quantity = electric_quantity
            self.remainder_range = self.electric_quantity*self.electric2distance_ratio
        else:
            print("电量未设置在合理范围！")
    
    def get_remainder_range(self):
        """查看剩余可行驶里程"""
        print("当前电量还可以继续驾驶 {} 公里".format(self.remainder_range))
```


```python
class ElectricCar(Car):
    """模拟电动汽车"""
    
    def __init__(self, brand, model, year, bettery_size):
        """初始化电动汽车属性"""
        super().__init__(brand, model, year)    # 声明继承父类的属性
        self.bettery = Bettery(bettery_size)    # 电池
    
    def get_main_information(self):        # 重写父类方法
        """获取汽车主要信息"""
        print("品牌：{}   型号：{}   出厂年份：{}   续航里程：{} 公里"
              .format(self.brand, self.model, self.year, 
              self.bettery.bettery_size*self.bettery.electric2distance_ratio))
```


```python
my_electric_car = ElectricCar("NextWeek", "FF91", 2046, 70)
my_electric_car.get_main_information() # 品牌：NextWeek   型号：FF91   出厂年份：2046   续航里程：350 公里

my_electric_car.bettery.get_electric_quantit() # 当前电池剩余电量：70 kW.h

my_electric_car.bettery.set_electric_quantity(50)       # 重设电池电量
my_electric_car.bettery.get_electric_quantit()          # 当前电池剩余电量：70 kW.h

my_electric_car.bettery.get_remainder_range()  # 当前电量还可以继续驾驶 250 公里
```

[返回首页](https://github.com/timerring/dive-into-AI)
