- [AI开发硬件环境综述](#ai开发硬件环境综述)
  - [笔记本选配](#笔记本选配)
  - [主机八大件选购](#主机八大件选购)
    - [关于CPU](#关于cpu)
      - [intel-酷容12代系列](#intel-酷容12代系列)
      - [AMD-锐龙5000系列](#amd-锐龙5000系列)
    - [主板介绍](#主板介绍)
    - [CPU散热器](#cpu散热器)
    - [硬盘 \& 内存](#硬盘--内存)
    - [GPU \&电源](#gpu-电源)
      - [N卡进化历程](#n卡进化历程)
      - [其他的Process Unit](#其他的process-unit)
    - [机箱](#机箱)
  - [多GPU情况](#多gpu情况)
  - [云服务器的推荐](#云服务器的推荐)


# AI开发硬件环境综述

## 笔记本选配

出于通勤等因素，建议型号

+ MacBook Air M1 16+ 256

+ MacBook Pro M1 16 + 256

M1芯片的mbp非常强大，发热不严重，甚至在Air版没风扇.

## 主机八大件选购

主要介绍主机GPUx1/GPUx2的配置，GPUx4/GPUx8的配置后面介绍，建议结论如下，3080版本总计13000左右，3090版本总计21000左右(显卡加6000左右换成3090，电源换成1000w，其他不需要变即可)。

CPU/主板:5900x＋微星MAG B550M MORTAR WIFI迫击炮主板.==> 3200元

CPU散热:利民Frozen Magic EX 240水冷. ==> 390元

硬盘:三星PM9A1 1T 809 + WD西数sn570 2T.==>1300元

内存:海盗船复仇者内存条32G x2. ==>900x2=1800元

GPU:耕升3080 12G ==>5200元

电源:长城850w金牌全模组==> 560元

机箱:300元左右支持240水冷的机箱即可=>200元

### 关于CPU

这里参考CPU天梯图 https://zhuanlan.zhihu.com/p/109042798 常看常新

CPU天梯图是按照CPU的跑分进行排序，进行综合性能对比、反映CPU性能优劣的一种量化标准。CPU主要有两家品牌: Intel(触点式接口）和AMD（针脚式接口)，此部分主要介绍Intel/AMD主流系列，其他系列会在后介绍。

1. intel酷睿系列:i3(入门办公). i5(主流).i7(高端级), i9(发烧级).eg: 12700k,12900k...

2. AMD锐龙系列:R3(入门办公).R5(主流).R7(高端).R9(发烧级).eg: 5700x,5900x...

CPU接口不同搭配的主板也不同，不同级别的CPU搭配不同级别的主板芯片组。eg:

1. 入门办公 : Intel主板(H开头)、AMD主板(A开头).eg:H610，A520

2. 主流∶Intel主板/AMD主板(B开头).eg: B660，B550

3. 高端/发烧: Intel主板(Z开头)、AMD主板(开头).eg: z690，x570

#### intel-酷容12代系列

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/03/14-17-55-58-1678787757.png)

https://zhuanlan.zhihu.com/p/429099752

intel 12代酷睿系列CPU，需要搭配不同后缀标代表不同意思，

1. 后缀k:具备核显，可以超频

2. 后缀KF:不具备核显，可以超频

3. 后缀F:不具备核先，不可超频需要搭配的主板型号

一般主机要搭配GPU，因此不需要考虑带K的系列。且长时间运行不建议超频。

**需要搭配的主板型号**

1）B660

2）Z690

#### AMD-锐龙5000系列

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/03/14-18-02-47-1678788164.png)

https://www.gamersky.com/news/202204/1479779.shtml

**AMD锐龙5000系列，后缀含义**

1）X: 高端处理器

2）G: 带核显..

**需要搭配的主板型号**

1）B550: 华硕TUF重炮手、微星迫击炮..

2）X570：

### 主板介绍

主板中比较好的牌子：华硕、技嘉、微星. 中高端都是可以选择的. 不同的主板的版型，即大小:

1) EATX/ATX: 需要搭配大机箱，散热最好

2) mATX：比较合适

3) ITX：扩展性、散热有问题

在AI训练、测试用途中，**CPU部分主要考虑的是核心&线程数量**。建议大家选购AMD 5900x型号，散片/盒装都可，CPU出故障率极低。（不推荐12代酷睿的原因是 在Ubuntu系统中版本适配做的不好，比如大小核小分配任务的故障等） 

+ 学校可以配置一个ATX的大机箱放到工位。

+ 公司可以配置mATX加一个小机箱。

另外，**主板一定要选择带蓝牙/wifi的配置**，这样可以剩下一个PCIE插口，后续扩展硬盘非常方便的。

### CPU散热器

+ 风冷: CPU的热量传到到热管鳍片的表面，通过风扇进行对流交换散热。
  
  推荐型号：猫头鹰系列（高风扇转速下非常安静）

+ 水冷: CPU的热量通过水泵对冷却液的循环，抽到散热片风扇冷却，循环散热。
  
  推荐型号：240起步，恩杰X系列、利民Frozen Magic EX系列、华硕ROG龙神系列。

CPU散热部分主要考虑的是噪音，风冷噪音实在太大，而水冷的风险是漏液。建议选购推荐品牌的240/360水冷。

### 硬盘 & 内存

**硬盘比较好的牌子：三星、铠侠、西部数据**，硬盘按照接口主要分为两种

+ PCIE3.0/4..0

+ SATA：速度太慢，不推荐

在AI训练、测试用途中，**硬盘部分主要考虑的是速度，容量其次**，有时候GPU显存的利用率很低，很大的程度是硬盘IO瓶颈。由于B550/B660主板预留的PCIE接口只有两个，所以推荐采用2T NVME PCIE3.0(装系统) + 1T NVME PCIE4.0. 存放有价值的数据集跟训练checkpoint是完全没问题的。后续如果想增加容量完全可以用PCIE扩展卡上面额外加固态即可。关于容量，比如imagenet 138G. COCO 27G. 1T + 2T的配置完全够用。

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/03/14-18-08-22-1678788499.png)

PCIE扩展卡的使用

```bash
# 格式化新硬盘并挂载到新目录的方法
df –h # 查看分区以及挂载点
fdisk –l # 查看服务器上所有硬盘状态（已安装和未安装）
mkfs -t ext4 -c /dev/sdb1 # 格式化硬盘
mount /dev/nvme1n1 /home/wlsh/ssd # 创建新目录作为新硬盘挂载点
vim /etc/fstab # 开机自动挂载
/dev/nvme1n1 /ssd ext4 defaults 0 0
```

**内存比较好的牌子：英睿达、海盗船、芝奇都可。**

**原则：内存的容量 > 2*GPU显存，越高越好**

在AI训练、测试用途中，**内存部分主要考虑的是容量**，数据的处理流程是硬盘=>内存=>GPU显存，一定量的内存能保证进行数据预处理的时候能非常好的。频率不需要太高，建议适中3200即可，考虑到后续参加比赛需求。预算不足情况下32G即可。

### GPU &电源

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/03/14-18-20-12-1678789210.png)

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/03/14-18-23-20-1678789397.png)

可见FP16算力远远大于FP32。

[AutoDL GPU算力排名](https://www.autodl.com/home)

GPU与Al训练、测试相关的参数:

+ FP64: Linpack(Linear system package)Test （通常在物理模拟等需要精度特别高的场景下才会考虑）

+ FP32: Deep Learning 单精算力

+ FP16: Quantization（压缩） & amp（混合精度）: python1.6 +++（以后的版本都支持） 半精算力

推荐两款型号3080 12G / 3090 24G，建议预算充足选择3090

注意：同样的型号 3090 24G，半精度下71TFLOPS远远大于单精度35TFLOPS。因此，可以选择开启半精度训练。也就是同样场景下半精度训练速度比单精度快一倍。

> 2张2080ti+1200w 在满载跑模型的时候，主机断电重启的问题，经检查pytorch启动瞬时功率过大导致的。

**电源建议：3080 12G 选用850w，3090选用1000w**

牌子：振华、海韵都可

#### N卡进化历程

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/03/14-18-31-26-1678789885.png)

在第三代的Kepler架构里，FP64单元和FP32单元的比例是1:3或者1:24。

第四代的Maxwell架构里，这个比例下降到了只有1:32。

第五代的Pascal架构里，这个比例又提高到了1:2，但低端型号里仍然保持为1:32。

一般重点关注FP32峰值算力，与DL息息相关。

对于一些特殊的卡，例如T4专门用于推理，只需要关注FP16和INT8的算力即可。

#### 其他的Process Unit

+ APU— Accelerated Processing Unit,加速处理器，AMD公司推出加速图像处理芯片产品。

+ BPU— Brain Processing Unit,地平线公司主导的嵌入式处理器架构。

+ CPU—Centrall Processing Unit中央处理器。目前PC core的主流产品。

+ DPU —Deep learning Processing Unit,深度学习处理器。最早由国内深鉴科技提出;另说有Dataflow Processing Unit数据流处理器,Wave Computing 公司提出的AI架构;Data storageProcessing Unit。深圳大普微的智能固态硬盘处理器。

+ FPU— Floating Processing Unit浮点计算单元，通用处理器中的浮点运算模块。

+ GPU —Graphics Processing Unit,图形处理器，采用多线程SIMD架构。为图形处理而生。

+ HPU一Holographics Processing Unit全息图像处理器，微软出品的全息计算芯片与设备。

+ IPU—Intelligence Processing Unit，Deep Mind投资的Graphcore公司出品的AI处理器产品。

+ MPU/MCU — Microprocessor/Micro controller Unit，微处理器/微控制器，一般用于低计算应用的RISC计算机体系架构产品,如ARM-M系列处理器。

+ NPU — Neural Network Processing Unit，神经网络处理器，是基于神经网络算法与加速的新型处理器总称。如中科院计算所/寒武纪公司出品的diannao系列。

+ RPU — Radio Processing Unit,无线电处理器,lmagination Technologies公司推出的集合集wifi/蓝牙/FM/处理器为单片的处理器。

+ TPU一Tensor Processing Unit张量处理器,Google公司推出的加速人工智能算法的专用处理器。目前一代TPU面向Inference，二代面向训练。

+ VPU—Vector Processing Unit 矢量处理器。Intel收购的Movidius公司推出的图像处理与人工智

从目前的实践来看，AI算法和传统HPC算法相比，对精度的要求低得多。因此我们看到很多AI芯片主要强调在FP16或者INT8中的精度。可以说，对目前AI芯硬件效率的提升，低比特精度有很大贡献。

### 机箱

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/03/14-18-36-11-1678790170.png)

[机箱风扇如何分配？](https://www.zhihu.com/question/320561511/answer/654827754)

构建合理的机箱风道能保证CPU跟显卡的温度，在确定自己机箱需求后，建议机箱安装若干风扇组件合理风道是非常重要的。

## 多GPU情况

GPUx4 or GPUx8: 在多GPU情况下、保证机器的稳定性是至关重要的，这时候就要选择更高系列的CPU。

1）Intel至强系列 4210R, 5218R, 6230R

2）AMD霄龙系列 7320, 7402, 74F3....

这些CPU比如支持ECC自动纠错内存、支持的CPU通道数更多、支持更高的PCIE通道等。

## 云服务器的推荐

Colab

AutoDL


