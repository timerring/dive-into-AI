# PyTorch简介与安装

## PyTorch 简介

2017 年 1 月， FAIR （Facebook AI Research ）发布 PyTorch。

PyTorch是在 Torch 基础上用python 语言重新打造的一款深度学习框架。

Torch是采用 Lua 语言为接口的机器学习框架，但因 Lua 语言较为小众，导致 Torch 知名度不高

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221003210030883.png)

## PyTorch发展

+ 2017 年 1 月正式发布 PyTorch
+ 2018 年 4 月更新 0.4.0 版，支持 Windows 系统，caffe2 正式并入 PyTorch
+ 2018 年 11 月更新 1.0 稳定版，已 GitHub 增长第二快的开源项目
+ 2019 年 5 月更新 1.1.0 版，支持 TensorBoard ，增强可视化功能
+ 2019 年 8 月更新 1.2.0 版，更新 torchvision torchaudio 和 torchtext ，增加更多功能

2014 年 10 月至 2018 年 02 月 arXiv 论文中深度学习框架提及次数统计。

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221003210213917.png)

PyTorch的增长速度与 TensorFlow一致。

2019 年 3 月各深度学习框架在 GitHub 上的Start Forks Watchers 和Contributors 数量对比

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221003210314598.png)

## PyTorch优点

+ 上手快 ：掌握 Numpy 和基本深度学习概念即可上手
+ 代码简洁灵活 ：用 nn.module 封装使网络搭建更方便；基于动态图机制，更灵活
+ Debug 方便 ：调试 PyTorch 就像调试 Python 代码一样简单
+ 文档规范： https://pytorch.org/docs/ 可查各版本文档
+ 资源多：arXiv 中的新算法大多有 PyTorch 实现
+ 开发者多：GitHub 上贡献者 ，已超过 1100+
+ 背靠大树：FaceBook 维护开发

适合人群：

+ 深度学习初学者 ：模型算法实现容易，加深深度学习概念认识

+ 机器学习爱好者 ：数十行代码便可实现人脸识别，目标检测，图像生成等有趣实验

+ 算法研究员 ：最新 arXiv 论文算法快速复现

## 软件安装

Python包管理器

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221003210531227.png)

Python集成开发环境

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221003210542501.png)

PyTorch

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221003210553275.png)

### 解释器与工具包

#### 解释器

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221003211625527.png)

#### 工具包

工具包又称为**依赖包 、 模块 、 库 、 包**。

python之所以强大是因为拥有大量工具包

+ 内置包：os 、 sys 、 glob 、 re 、 math等
+ 第三方包：pytorch tensorflow numpy等

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221003211754605.png)

#### 虚拟环境

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221003211924054.png)

### Anaconda 安装

Anaconda 是为方便使用 python 而建立的一个软件包，其包含常用的 250 多个工具包。

多版本 python 解释器 和强大的虚拟环境管理工具，所以 Anaconda 得名 python 全家桶。

Anaconda可以使安装、运行和升级环境变得更简单，因此推荐安装使用。

##### 安装步骤

1.官网下载安装包 https://www.anaconda.com/distribution/#download-section

2.运行 Anaconda3 Windows x86_64.exe

3.选择路径，勾选 Add Anaconda to the system PATH environment variable ，等待安装完成

4.验证安装成功，打开 cmd ，输入 conda ，回车

5.添加中科大镜像

### Pycharm 安装

Pycharm——强大的python IDE ，拥有**调试 、语法高亮** 、 Project 管理、**代码跳转 、 智能提示** 、版本控制等功能

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221003212236356.png)

安装步骤：

1.官网下载安装包 https://www.jetbrains.com/pycharm

2.运行 pycharm professional 20xx.2.exe

3.选择路径，勾选 Add launchers dir to the PATH ，等待安装完成

激活步骤：

1.下载破解文件

2.将 jetbrains agent.jar 放到 pycharm 安装目录中 bin 文件夹

3.创建空项目，在 pycharm64.exe.vmoptions 中添加命令 javaagent 安装目录 jetbrains agent.jar

4.重启，完成激活

### PyTorch 安装

安装步骤：
1.检查是否有合适 GPU ，若有，需安装 CUDA 与 CuDNN

2.CUDA与 CuDNN 安装（详情见 ）

3.下载 whl 文件，登陆 https://download.pytorch.org/whl/torch_stable.html

命名解释：
![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221003213522325.png)

下载 pytorch 与 torchvision 的 whl 文件，进入相应虚拟环境，通过 pip 安装

4.在 pycharm 中创建 hello pytorch 项目，运行脚本，查看 pytorch 版本

