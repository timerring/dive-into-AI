# Pytorch、CUDA和cuDNN的安装图文详解win11（解决版本匹配问题）

### 可能出现的问题

+ CUDA和cuDNN版本不匹配
+ CUDA和Pytorch版本不匹配
+ cuDNN和Pytorch版本不匹配
+ 显卡不支持CUDA该版本
+ 已经装完部分，发现版本不匹配准备卸载。

### 说在前面的话

1. 在ubuntu系统下，可以尝试装多个cuda版本，然后通过conda安装对应的Pytorch版本。通过软连接的方式来实现cuda版本的切换。**但是，在win系统下，最好是用相同的支持版本，以免不匹配。**不用纠结是否向下兼容等等问题，最优的方法就是安装相同的版本。
2. 对于CUDA的版本，我推荐用以往的稳定版本，就是指目前还在一直维护的比较旧的版本，原因有很多：
   + 比较旧的版本有强大的社区支持，可以方便地找到前人总结地bug解决方案，而不是遇到最新问题时能力不够导致的一筹莫展。
   + 较旧的版本至今仍在维护，说明其仍有很大的价值，用户基数很多，能确保开发的流畅与稳定。
   + 由于学术界和工业界都喜欢用比较稳定的版本来搭建模型，因此如果想要复现论文，或是pull别人的代码修改，较新的版本很有可能会出现错误。

### CUDA的安装

#### 1.查询支持的最高版本

首先安装之前要先检查我们显卡所支持的最高的CUDA版本：

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221004171838784.png)



![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221004171904219.png)

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221004172119500.png)

目前是11.6的驱动，因此我的显卡最高是可以支持到CUDA11.6版本的。

知道了我们的最高支持版本之后，我们就可以在小于等于该版本的CUDA中选择了。

#### 2.查询Pytoch与cuDNN版本

首先不用着急挑选CUDA的版本。我们先看下[pytorch](https://pytorch.org/)以及[cuDNN](https://developer.nvidia.com/rdp/cudnn-archive)的版本支持情况。

Pytorch：https://pytorch.org/

cuDNN：https://developer.nvidia.com/rdp/cudnn-archive

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221005205103236.png)

可以看到对于win系统，Pytorch支持的版本有10.2，11.3，11.6等。

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221005205417938.png)

cuDNN支持的版本有10.2，11.5，11.4等等。

**这里我推荐装10.2或者11.3，原因见开头，不多做赘述。这里以11.3为例。**

#### 3.下载CUDA

在CUDAhttps://developer.nvidia.com/cuda-toolkit-archive中，寻找[CUDA Toolkit 11.3](https://developer.nvidia.com/cuda-11.3.0-download-archive)版本，然后寻找相应的版本下载即可。

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221005211449241.png)

#### 4.安装CUDA

安装CUDA，首先需要选择CUDA的临时解压路径，这个临时解压文件夹会在安装完成后自动删除，这里建议默认。

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221005220025160.png)

解压完成后会有安装程序，同意即可。接下来的安装选项选择自定义：

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221005220553919.png)

在安装CUDA中取消这个VS有关的组件：

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221005220639170.png)

底下这三个也没必要，可安可不安，看个人选择：

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221005220730477.png)

安装路径仍然建议默认，在Program Files中，方便以后寻找。建议记住这里的CUDA路径。

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221005220819979.png)

然后一直确认最后关闭即可，这里不多赘述。

#### 5.验证CUDA是否安装成功

我们在`cmd`中使用cd命令切换到刚刚CUDA的安装路径下的bin(二进制)文件夹下，再执行nvcc -V命令。

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221005221631804.png)

可见CUDA是正确安装的。







### cuDNN的安装

在cuDNN的版本中，选择支持该版本的CUDA即可，这里我们看到v8.5.0的cuDNN支持CUDA 11.X，说明兼容cuda11.x全系列。点击下载即可。

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221005212213927.png)

接下来，解压该压缩包，然后复制其中的文件夹

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221005221835067.png)

粘贴到CUDA的安装目录下，即完成了cuDNN的安装。

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221005222043585.png)

#### 验证是否安装成功

在cmd中进入到demo文件夹：路径为`C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.3\extras\demo_suite`

执行`bandwidthTest.exe`，如果运行结果出现了PASS即代表安装成功。

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221005222523389.png)

再输入命令`deviceQuery.exe`查询设备。

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221005222811243.png)

这里会显示你的GPU型号，以及PASS，表示CUDA和cuDNN都安装成功了。

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221005222856213.png)

### Pytorch安装

进入Pytorch官网https://pytorch.org/，选择需要安装的pytorch版本。这里安装方式可以选择pip。可以看到有生成的command，里面有个网站，只需要进入该网址下载即可。

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221005223222069.png)

#### 下载torch

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221005223341575.png)

由于我们安装的torch版本为Stable1.12.1，因此我们需要查找前缀为：`torch-1.12.1`的文件。注意找的是GPU版本，cuxxx代表CUDA版本xxx。这里我们找到对应的cu113，然后点击下载。cp代表python版本，这里我们选择cp37版本的win下载。

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221005223819969.png)

然后返回，进入torchvision。

#### 下载torchvision

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221005224125728.png)

torchvision的版本选择最新就好，但是要与cuda及python匹配，这里直接搜索`cu113-cp37`

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221005224427066.png)

下载对应的版本即可。





新建pycharm创建项目，新建项目名称为PytorchTest， 文件名为HelloPytorch。输入以下代码测试

```python
import torch

print("Hello Pytorch{}".format(torch.__version__))
```

结果报错，`ModuleNotFoundError: No module named 'torch'`，因为当前环境没有torch。

我们可以用conda创建虚拟环境并安装torch。

首先再Terminal中输入`conda create -n pytorch-gpu python=3.7`，这里python版本与我们要安装的版本相同。

出现Proceed ([y]/n)? 直接输入y即可。

接着输入`conda activate pytorch_gpu`激活环境。

> 注意：进入conda虚拟环境后venv前面的提示会变成你的环境名称，如下：
>
> ![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221006093929724.png)
>
> 如果没有显示，则可能因为pycharm终端采用的是PowerShell。需要在设置中切换。
>
> ![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221006094048053.png)
>
> 换成如图所示的cmd终端即可。
>
> ![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221006094121856.png)

进入终端后切换到下载刚刚torch和torchvision的文件夹中

```
cd D:\Develop\pytorch_install //因人而异，cd到你的下载torch和torchvision的文件夹中即可。
```

用pip安装torch

```
pip install "torch-1.12.1+cu113-cp37-cp37m-win_amd64.whl"
```

用pip安装torchvision

```
pip install "torchvision-0.13.1+cu113-cp37-cp37m-win_amd64.whl"
```

完成安装

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221006094726646.png)

安装完成后，需要绑定该项目的解释器为这个虚拟环境。因此需要设置：

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221006094823670.png)

找到该项目的Python解释器，然后点击齿轮，选择add：

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221006094937368.png)

选择conda，找到已经存在的环境：

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221006095030919.png)

查找你的anaconda安装的目录

这里可以搜索anaconda并打开文件位置。

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221006124152338.png)

然后继续右键打开文件位置即可。

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221006124319493.png)

打开后找到envs（环境）文件夹。

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221006124554532.png)

找到刚刚创建的环境，复制文件路径到pycharm。

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221006124706598.png)

在pycharm中选择该文件路径下的python.exe解释器即可

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221006124842235.png)

然后一直ok，等待python解释器的重载即可，可能需要等一分钟。

然后重新尝试测试代码并运行。

```python
import torch

print("Hello Pytorch{}".format(torch.__version__))

print(torch.cuda.is_available())
```

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221006125306212.png)

返回true说明安装成功。



### CUDA的卸载

首先，搜索控制面板并打开

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221004165243861.png)

找到程序卸载

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221004165329186.png)

可以在列表中找到有关于NVIDIA的相关组件，找到有关于CUDA的组件并卸载即可，其他的可以保留，因为高于该版本的CUDA会更新其他组件的。（**本质上临近时间安装的都能卸载**）

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221004172459627.png)

右键 -> 卸载 然后在卸载程序中卸载即可。剩下的方法类似。

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221004172743410.png)

最后不放心的话可以用火绒等软件清理一下注册表。