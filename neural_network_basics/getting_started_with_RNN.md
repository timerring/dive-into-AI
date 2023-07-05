- [循环神经网络](#循环神经网络)
  - [序列数据](#序列数据)
  - [语言模型](#语言模型)
  - [RNN—循环神经网络（Recurrent neural network）](#rnn循环神经网络recurrent-neural-network)
  - [GRU—门控循环单元](#gru门控循环单元)
  - [LSTM—长短期记忆网络](#lstm长短期记忆网络)
  - [总结](#总结)


# 循环神经网络

## 序列数据

序列数据是常见的数据类型，**前后数据通常具有关联性**

例如 “**Cats** average **15** hours of sleep a day”

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20230627170322804.png)

## 语言模型

语言模型是自然语言处理 (NLP, Natural Language Processing) 重要技术。

NLP中常把文本看为离散时间序列，一段长度为T的文本的词依次为 $\mathrm{W}_{1}, \mathrm{~W}_{2}, \ldots, \mathrm{W}_{\mathrm{T}}$, 其中  $\mathrm{w}_{\mathrm{t}}(1 \leq \mathrm{t} \leq \mathrm{T})$ 是**时间步 ( Time Step）**t 的输出或标签。

语言模型将会计算该序列概率$\mathrm{P}\left(\mathrm{w}_{1}, \mathrm{w}_{2}, \ldots, \mathrm{w}_{\mathrm{T}}\right)$，例如 **Cats** average **15** hours of sleep a day，这句话中 T = 8.

语言模型计算序列概率：
$$
\mathrm{P}\left(\mathrm{w}_{1}, \mathrm{w}_{2}, \ldots, \mathrm{w}_{\mathrm{T}}\right)=\prod_{\mathrm{t}=1}^{\mathrm{T}} \mathrm{P}\left(\mathrm{w}_{\mathrm{t}} \mid \mathrm{w}_{1}, \ldots, \mathrm{w}_{\mathrm{t}-1}\right)
$$

> 例如：P( 我, 在, 听, 课 ) = P(我) * P（在｜我）*  P（ 听｜我 ，在）*  P（课 ｜ 我 ，在，听）

统计**语料库（Corpus）**中的词频，得到以上概率，最终得到P(我, 在, 听, 课)

**缺点**： 时间步t的词需要考虑t -1步的词，其计算量随t呈指数增长。

## RNN—循环神经网络（Recurrent neural network）

RNN 是针对序列数据而生的神经网络结构，核心在于循环使用网络层参数，避免时间步增大带来的参数激增，并引入**隐藏状态（Hidden State）**用于记录历史信息，有效处理数据的前后关联性。

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20230627184534592.png)



**隐藏状态（Hidden State）**用于记录历史信息，有效处理数据的前后关联性，**激活函数采用Tanh，将输出值域限制在（-1，1），防止数值呈指数级变化。**可以进行如下对比：

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20230627184710058.png)

RNN构建语言模型，实现文本生成。假设文本序列：“想”，“要”，“有”，“直”，“升”，“机”。

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20230627184903730.png)

RNN特性：

1. 循环神经网络的**隐藏状态**可以捕捉截至当前时间步的序列的历史信息；
2. 循环神经网络模型参数的数量不随时间步的增加而增长。（一直是$W_{hh}$、$W_{xh}$、$W_{hq}$）

$$
\begin{aligned}
\boldsymbol{H}_{t} & =\phi\left(\boldsymbol{X}_{t} \boldsymbol{W}_{x h}+\boldsymbol{H}_{t-1} \boldsymbol{W}_{h h}+\boldsymbol{b}_{h}\right) \\
\boldsymbol{O}_{t} & =\boldsymbol{H}_{t} \boldsymbol{W}_{h q}+\boldsymbol{b}_{q}
\end{aligned}
$$

RNN的**通过（穿越）时间反向传播**（backpropagation through time）

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20230627185151032.png)

有几条通路，就几项相加。

> 方便起见，一下分别称为式1-4.

$$
\begin{aligned}
\frac{\partial L}{\partial \boldsymbol{W}_{q h}} & =\sum_{t=1}^{T} \operatorname{prod}\left(\frac{\partial L}{\partial \boldsymbol{o}_{t}}, \frac{\partial \boldsymbol{o}_{t}}{\partial \boldsymbol{W}_{q h}}\right)=\sum_{t=1}^{T} \frac{\partial L}{\partial \boldsymbol{o}_{t}} \boldsymbol{h}_{t}^{\top} \\
\frac{\partial L}{\partial \boldsymbol{h}_{T}} & =\operatorname{prod}\left(\frac{\partial L}{\partial \boldsymbol{o}_{T}}, \frac{\partial \boldsymbol{o}_{T}}{\partial \boldsymbol{h}_{T}}\right)=\boldsymbol{W}_{q h}^{\top} \frac{\partial L}{\partial \boldsymbol{o}_{T}} \\
\frac{\partial L}{\partial \boldsymbol{h}_{t}} & =\operatorname{prod}\left(\frac{\partial L}{\partial \boldsymbol{h}_{t+1}}, \frac{\partial \boldsymbol{h}_{t+1}}{\partial \boldsymbol{h}_{t}}\right)+\operatorname{prod}\left(\frac{\partial L}{\partial \boldsymbol{o}_{t}}, \frac{\partial \boldsymbol{o}_{t}}{\partial \boldsymbol{h}_{t}}\right) \\
& =\boldsymbol{W}_{h h}^{\top} \frac{\partial L}{\partial \boldsymbol{h}_{t+1}}+\boldsymbol{W}_{q h}^{\top} \frac{\partial L}{\partial \boldsymbol{o}_{t}} \\
\frac{\partial L}{\partial \boldsymbol{h}_{t}} & =\sum_{i=t}^{T}\left(\boldsymbol{W}_{h h}^{\top}\right)^{T-i} \boldsymbol{W}_{q h}^{\top} \frac{\partial L}{\partial \boldsymbol{o}_{T+t-i}}
\end{aligned}
$$

如上，T=3。可以由第二个式子算出$L$对于$h_T$的偏导。

然后由第三个式子计算$L$对于$h_t$的偏导，注意其中的$L$对于$h_T$的偏导已经计算完成了，直接带入即可。

然后以此类推，得到一个$L$对于$h_t$的偏导的通式，见第四个式子。

> 这里可以用第四个通式计算一下$L$对于$h_1$的偏导，如下：
>
> ![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20230627225310177.png)

计算剩余两个参数，由于通路过多，因此这里再计算通路就相对来说复杂了，只需要采用反向传播的思想即可，将问题拆解。但是结果还是很复杂的，如下所示，计算$W_{hx}$的梯度，使用了$h_t$的偏导，同时如上，求$h_t$的偏导还会涉及$h_{t+1}$的偏导,递归下去...梯度的计算穿越了时间。

> 方便起见，一下分别称为式5-6.

$$
\begin{aligned}
\frac{\partial L}{\partial \boldsymbol{W}_{h x}} & =\sum_{t=1}^{T} \operatorname{prod}\left(\frac{\partial L}{\partial \boldsymbol{h}_{t}}, \frac{\partial \boldsymbol{h}_{t}}{\partial \boldsymbol{W}_{h x}}\right)=\sum_{t=1}^{T} \frac{\partial L}{\partial \boldsymbol{h}_{t}} \boldsymbol{x}_{t}^{\top}, \\
\frac{\partial L}{\partial \boldsymbol{W}_{h h}} & =\sum_{t=1}^{T} \operatorname{prod}\left(\frac{\partial L}{\partial \boldsymbol{h}_{t}}, \frac{\partial \boldsymbol{h}_{t}}{\partial \boldsymbol{W}_{h h}}\right)=\sum_{t=1}^{T} \frac{\partial L}{\partial \boldsymbol{h}_{t}} \boldsymbol{h}_{t-1}^{\top} .
\end{aligned}
$$

因此会存在一个问题：梯度随时间t呈指数变化，易引发**梯度消失**或**梯度爆炸**。(例如$W_{hh}$，见式4${W}_{h h}^{\top}$涉及一个次方问题，那么$W_{hh}$ < 1会使最终趋于0，引发梯度消失，而若$W_{hh}$ > 1会使最终趋于无穷，引发梯度爆炸)。

## GRU—门控循环单元

引入门的循环网络缓解RNN梯度消失带来的问题，引入门概念，来控制信息流动，使模型更好的记住长远时期的信息，并缓解梯度消失。

+ **重置门**：哪些信息需要遗忘
+ **更新门**：哪些信息需要注意

激活函数为：sigmoid，值域为(0, 1)，0表示遗忘，1表示保留。可知，门的值域是(0, 1)之间。

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20230628182432977.png)

**重置门**是用于候选隐藏状态计算过程当中遗忘上一时间步隐藏状态中的哪一些信息。

**更新门**的作用是更新当前时间步隐藏状态的时候，组合上一时间步隐藏状态$\boldsymbol{H}_{t-1}$。以及当前时间步的候选隐藏状态$\tilde{\boldsymbol{H}}_{\mathrm{t}}$得到最终的$\boldsymbol{H}_{t}$。
$$
\begin{aligned}
\boldsymbol{R}_{t} & =\sigma\left(\boldsymbol{X}_{t} \boldsymbol{W}_{x r}+\boldsymbol{H}_{t-1} \boldsymbol{W}_{h r}+\boldsymbol{b}_{r}\right) \\
\boldsymbol{Z}_{t} & =\sigma\left(\boldsymbol{X}_{t} \boldsymbol{W}_{x z}+\boldsymbol{H}_{t-1} \boldsymbol{W}_{h z}+\boldsymbol{b}_{z}\right)
\end{aligned}
$$
**侯选隐藏状态**

输入与上一时间步隐藏状态共同计算得到候选隐藏状态，用于隐藏状态计算。通过重置门，对上一时间步隐藏状态进行**选择性遗忘**， 对历史信息更好的选择。

GRU：
$$
\tilde{\boldsymbol{H}}_{\mathrm{t}}=\tanh \left(\boldsymbol{X}_{\mathrm{t}} \boldsymbol{W}_{\mathrm{xh}}+\left(\boldsymbol{R}_{\mathrm{t}} \odot \boldsymbol{H}_{\mathrm{t}-1}\right) \boldsymbol{W}_{\mathrm{hh}}+\boldsymbol{b}_{\mathrm{h}}\right)
$$

> 试比较原来的RNN：
> $$
> \boldsymbol{H}_{\mathrm{t}}=\phi\left(\boldsymbol{X}_{\mathrm{t}} \boldsymbol{W}_{\mathrm{xh}}+\boldsymbol{H}_{\mathrm{t}-1} \boldsymbol{W}_{\mathrm{hh}}+\boldsymbol{b}_{\mathrm{h}}\right)
> $$

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20230628182945326.png)

**隐藏状态**

隐藏状态由**候选隐藏状态**及**上一时间步隐藏状态**组合得来
$$
\boldsymbol{H}_{\mathrm{t}}=\boldsymbol{Z}_{\mathrm{t}} \odot \boldsymbol{H}_{\mathrm{t}-1}+\left(1-\boldsymbol{Z}_{\mathrm{t}}\right) \odot \tilde{\boldsymbol{H}}_{\mathrm{t}}
$$
![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20230628183048305.png)

GRU特点:

门机制采用Sigmoid激活函数，使门值为(0,1)，0表示遗忘， 1表示保留。

若更新门自第一个时间步到 t - 1时间过程中，一直保持为1 ， **信息可有效传递到当前时间步**。
$$
\boldsymbol{H}_{\mathrm{t}}=\boldsymbol{Z}_{\mathrm{t}} \odot \boldsymbol{H}_{\mathrm{t}-1}+\left(1-\boldsymbol{Z}_{\mathrm{t}}\right) \odot \tilde{\boldsymbol{H}}_{\mathrm{t}}
$$
重置门：用于遗忘上一时间步隐藏状态
$$
\tilde{\boldsymbol{H}}_{\mathrm{t}}=\tanh \left(\boldsymbol{X}_{\mathrm{t}} \boldsymbol{W}_{\mathrm{xh}}+\left(\boldsymbol{R}_{\mathrm{t}} \odot \boldsymbol{H}_{\mathrm{t}-1}\right) \boldsymbol{W}_{\mathrm{hh}}+\boldsymbol{b}_{\mathrm{h}}\right)
$$
更新门： 用于更新当前时间步隐藏状态
$$
\boldsymbol{H}_{\mathrm{t}}=\boldsymbol{Z}_{\mathrm{t}} \odot \boldsymbol{H}_{\mathrm{t}-1}+\left(1-\boldsymbol{Z}_{\mathrm{t}}\right) \odot \tilde{\boldsymbol{H}}_{\mathrm{t}}
$$

## LSTM—长短期记忆网络

**LSTM**

引入 **3 个门**和**记忆细胞**，控制信息传递

+ 遗忘门：哪些信息需要遗忘
+ 输入门：哪些信息需要流入当前**记忆细胞**
+ 输出门：哪些记忆信息流入隐藏状态
+ 记忆细胞： 特殊的隐藏状态，记忆历史信息

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20230628183545472.png)
$$
\begin{aligned}
\boldsymbol{I}_{t} & =\sigma\left(\boldsymbol{X}_{t} \boldsymbol{W}_{x i}+\boldsymbol{H}_{t-1} \boldsymbol{W}_{h i}+\boldsymbol{b}_{i}\right) \\
\boldsymbol{F}_{t} & =\sigma\left(\boldsymbol{X}_{t} \boldsymbol{W}_{x f}+\boldsymbol{H}_{t-1} \boldsymbol{W}_{h f}+\boldsymbol{b}_{f}\right) \\
\boldsymbol{O}_{t} & =\sigma\left(\boldsymbol{X}_{t} \boldsymbol{W}_{x o}+\boldsymbol{H}_{t-1} \boldsymbol{W}_{h o}+\boldsymbol{b}_{o}\right)
\end{aligned}
$$
**候选记忆细胞**

记忆细胞：可理解为**特殊隐藏状态**，储存历史时刻信息
$$
\tilde{\boldsymbol{C}}_{\mathrm{t}}=\tanh \left(\boldsymbol{X}_{\mathrm{t}} \boldsymbol{W}_{\mathrm{xc}}+\boldsymbol{H}_{\mathrm{t}-1} \boldsymbol{W}_{\mathrm{hc}}+\boldsymbol{b}_{\mathrm{c}}\right)
$$
**记忆细胞与隐藏状态**

记忆细胞由**候选记忆细胞**及**上一时间步记忆细胞**组合得来
$$
\boldsymbol{C}_{t}=\boldsymbol{F}_{t} \odot \boldsymbol{C}_{t-1}+\boldsymbol{I}_{t} \odot \tilde{\boldsymbol{C}}_{\mathrm{t}}
$$
由输出门控制记忆细胞信息流入**隐藏状态**
$$
\boldsymbol{H}_{\mathrm{t}}=\boldsymbol{O}_{\mathrm{t}} \odot \tanh \left(\boldsymbol{C}_{\mathrm{t}}\right)
$$
![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20230628183931806.png)

## 总结

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20230628184849665.png)