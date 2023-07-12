# 模型创建与nn.Module

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221127180131983.png)

创建网络模型通常有2个要素：

+ **构建子模块**
+ **拼接子模块**

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221127194306080.png)

+ 

```python
class LeNet(nn.Module):
	# 子模块创建
    def __init__(self, classes):
        super(LeNet, self).__init__()
        self.conv1 = nn.Conv2d(3, 6, 5)
        self.conv2 = nn.Conv2d(6, 16, 5)
        self.fc1 = nn.Linear(16*5*5, 120)
        self.fc2 = nn.Linear(120, 84)
        self.fc3 = nn.Linear(84, classes)
	# 子模块拼接
    def forward(self, x):
        out = F.relu(self.conv1(x))
        out = F.max_pool2d(out, 2)
        out = F.relu(self.conv2(out))
        out = F.max_pool2d(out, 2)
        out = out.view(out.size(0), -1)
        out = F.relu(self.fc1(out))
        out = F.relu(self.fc2(out))
        out = self.fc3(out)
        return out
```

调用`net = LeNet(classes=2)`创建模型时，会调用`__init__()`方法创建模型的子模块。

训练调用`outputs = net(inputs)`时，会进入`module.py`的`call()`函数中：

```python
    def __call__(self, *input, **kwargs):
        for hook in self._forward_pre_hooks.values():
            result = hook(self, input)
            if result is not None:
                if not isinstance(result, tuple):
                    result = (result,)
                input = result
        if torch._C._get_tracing_state():
            result = self._slow_forward(*input, **kwargs)
        else:
            result = self.forward(*input, **kwargs)
        ...
        ...
        ...
```

最终会调用`result = self.forward(*input, **kwargs)`函数，该函数会进入模型的`forward()`函数中，进行前向传播。

在 `torch.nn`中包含 4 个模块，如下图所示。

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221127194825717.png)

本次重点就在于nn.Model的解析：

### nn.Module

`nn.Module` 有 8 个属性，都是`OrderDict`(有序字典)的结构。在 LeNet 的`__init__()`方法中会调用父类`nn.Module`的`__init__()`方法，创建这 8 个属性。

```python
    def __init__(self):
        """
        Initializes internal Module state, shared by both nn.Module and ScriptModule.
        """
        torch._C._log_api_usage_once("python.nn_module")

        self.training = True
        self._parameters = OrderedDict()
        self._buffers = OrderedDict()
        self._backward_hooks = OrderedDict()
        self._forward_hooks = OrderedDict()
        self._forward_pre_hooks = OrderedDict()
        self._state_dict_hooks = OrderedDict()
        self._load_state_dict_pre_hooks = OrderedDict()
        self._modules = OrderedDict()
```

- **_parameters 属性**：存储管理 nn.Parameter 类型的参数
- **_modules 属性**：存储管理 nn.Module 类型的参数
- _buffers 属性：存储管理缓冲属性，如 BN 层中的 running_mean
- 5 个 ***_hooks 属性：存储管理钩子函数

LeNet 的`__init__()`中创建了 5 个子模块，`nn.Conv2d()`和`nn.Linear()`都继承于`nn.module`，即一个 module 都是包含多个子 module 的。

```ruby
class LeNet(nn.Module):
	# 子模块创建
    def __init__(self, classes):
        super(LeNet, self).__init__()
        self.conv1 = nn.Conv2d(3, 6, 5)
        self.conv2 = nn.Conv2d(6, 16, 5)
        self.fc1 = nn.Linear(16*5*5, 120)
        self.fc2 = nn.Linear(120, 84)
        self.fc3 = nn.Linear(84, classes)
        ...
        ...
        ...
```

当调用`net = LeNet(classes=2)`创建模型后，`net`对象的 modules 属性就包含了这 5 个子网络模块。

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221127195338431.png)

下面看下每个子模块是如何添加到 LeNet 的`_modules` 属性中的。以`self.conv1 = nn.Conv2d(3, 6, 5)`为例，当我们运行到这一行时，首先 Step Into 进入 `Conv2d`的构造，然后 Step Out。右键`Evaluate Expression`查看`nn.Conv2d(3, 6, 5)`的属性。

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221127200100016.png)

上面说了`Conv2d`也是一个 module，里面的`_modules`属性为空，`_parameters`属性里包含了该卷积层的可学习参数，这些参数的类型是 Parameter，继承自 Tensor。

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221127200205812.png)

此时只是完成了`nn.Conv2d(3, 6, 5)` module 的创建。还没有赋值给`self.conv1 `。**在`nn.Module`里有一个机制，会拦截所有的类属性赋值操作(`self.conv1`是类属性)**，进入到`__setattr__()`函数中。我们再次 Step Into 就可以进入`__setattr__()`。

```python
   def __setattr__(self, name, value):
        def remove_from(*dicts):
            for d in dicts:
                if name in d:
                    del d[name]

        params = self.__dict__.get('_parameters')
        if isinstance(value, Parameter):
            if params is None:
                raise AttributeError(
                    "cannot assign parameters before Module.__init__() call")
            remove_from(self.__dict__, self._buffers, self._modules)
            self.register_parameter(name, value)
        elif params is not None and name in params:
            if value is not None:
                raise TypeError("cannot assign '{}' as parameter '{}' "
                                "(torch.nn.Parameter or None expected)"
                                .format(torch.typename(value), name))
            self.register_parameter(name, value)
        else:
            modules = self.__dict__.get('_modules')
            if isinstance(value, Module):
                if modules is None:
                    raise AttributeError(
                        "cannot assign module before Module.__init__() call")
                remove_from(self.__dict__, self._parameters, self._buffers)
                modules[name] = value
            elif modules is not None and name in modules:
                if value is not None:
                    raise TypeError("cannot assign '{}' as child module '{}' "
                                    "(torch.nn.Module or None expected)"
                                    .format(torch.typename(value), name))
                modules[name] = value
            ...
            ...
            ...
```

在这里判断 value 的类型是`Parameter`还是`Module`，存储到对应的有序字典中。

这里`nn.Conv2d(3, 6, 5)`的类型是`Module`，因此会执行`modules[name] = value`，key 是类属性的名字`conv1`，value 就是`nn.Conv2d(3, 6, 5)`。

## 总结

- 一个 module 里可包含多个子 module。比如 LeNet 是一个 Module，里面包括多个卷积层、池化层、全连接层等子 module
- 一个 module 相当于一个运算，必须实现 forward() 函数
- 每个 module 都有 8 个字典管理自己的属性

参考文章：

https://www.cnblogs.com/zhangxiann/p/13579624.html