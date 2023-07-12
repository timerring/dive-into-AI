# 数据增强之图像变换与自定义transforms

### torchvision.transforms.Pad

```bash
torchvision.transforms.Pad(padding, fill=0, padding_mode='constant')
```

功能：对图像边缘进行填充

- padding: 设置填充大小
  - 当为 a 时，上下左右均填充 a 个像素
  - 当为 (a, b) 时，左右填充 a 个像素，上下填充 b 个像素
  - 当为 (a, b, c, d) 时，左上右下分别填充 a，b，c，d
  - padding_mode: 填充模式，有 4 种模式，constant、edge、reflect、symmetric
  - fill: 当 padding_mode 为 constant 时，设置填充的像素值，(R, G, B) 或者 (Gray)

### torchvision.transforms.ColorJitter

```scss
torchvision.transforms.ColorJitter(brightness=0, contrast=0, saturation=0, hue=0)
```

功能：调整亮度、对比度、饱和度、色相。在照片的拍照过程中，可能会由于设备、光线问题，造成色彩上的偏差，因此需要调整这些属性，抵消这些因素带来的扰动。

- brightness: 亮度调整因子
- contrast: 对比度参数
- saturation: 饱和度参数
- brightness、contrast、saturation 参数：
  + 当为 a 时，从 [max(0, 1-a), 1+a] 中随机选择；
  + 当为 (a, b) 时，从 [a, b] 中选择。
- hue: 色相参数
  - 当为 a 时，从 [-a, a] 中选择参数。其中 $0\le a \le 0.5$。
  - 当为 (a, b) 时，从 [a, b] 中选择参数。其中 $0 \le a \le b \le 0.5$。

### transforms.Grayscale(RandomGrayscale)

```scss
torchvision.transforms.Grayscale(num_output_channels=1)
```

功能：将图片转换为灰度图

- num_output_channels: 输出的通道数。只能设置为 1 或者 3 (如果在后面使用了`transforms.Normalize`，则要设置为 3，因为`transforms.Normalize`只能接收 3 通道的输入)
- Grayscale是RandomGrayscale的一个特例，即p = 1的特例。

```scss
torchvision.transforms.RandomGrayscale(p=0.1, num_output_channels=1)
```

- p: 概率值，图像被转换为灰度图的概率
- num_output_channels: 输出的通道数。只能设置为 1 或者 3

功能：根据一定概率将图片转换为灰度图。

### transforms.RandomAffine

```python
torchvision.transforms.RandomAffine(degrees, translate=None, scale=None, shear=None, resample=False, fillcolor=0)
```

功能：对图像进行仿射变换，仿射变换是 2 维的线性变换，由 5 种基本操作组成，分别是旋转、平移、缩放、错切和翻转。

- degree: 旋转角度设置
- translate: 平移区间设置，如 (a, b)，a 设置宽 (width)，b 设置高 (height)。图像在宽维度平移的区间为 $- img_{width} \times a < dx < img_{width} \times a$，高以相同的方式进行。
- scale: 缩放比例，以面积为单位
- fillcolor: 填充颜色设置
- shear: 错切角度设置，有水平错切和垂直错切。错切的意思：例如水平侧切（x轴侧切），保持图片的x平行，将图片y轴斜拉，使得整张图片类似于一个平行四边形。
  - 若为 a，则仅在 x 轴错切（保持x轴平行），在 (-a, a) 之间随机选择错切角度
  - 若为 (a, b)，x 轴在 (-a, a) 之间随机选择错切角度，y 轴在 (-b, b) 之间随机选择错切角度
  - 若为 (a, b, c, d)，x 轴在 (a, b) 之间随机选择错切角度，y 轴在 (c, d) 之间随机选择错切角度
- resample: 重采样方式，有 NEAREST、BILINEAR、BICUBIC。

### transforms.RandomErasing

```python
torchvision.transforms.RandomErasing(p=0.5, scale=(0.02, 0.33), ratio=(0.3, 3.3), value=0, inplace=False)
```

> 以上参数是论文中给出的较好的范围。

功能：对图像进行随机遮挡。这个操作接收的输入是 tensor。因此在此之前需要先执行`transforms.ToTensor()`。同时注释掉后面的`transforms.ToTensor()`。

- p: 概率值，执行该操作的概率
- scale: 遮挡区域的面积。如(a, b)，则会随机选择 (a, b) 中的一个遮挡比例
- ratio: 遮挡区域长宽比。如(a, b)，则会随机选择 (a, b) 中的一个长宽比
- value: 设置遮挡区域的像素值。(R, G, B) 或者 Gray，或者任意字符串。由于之前执行了`transforms.ToTensor()`，像素值归一化到了 0~1 之间，因此这里设置的 (R, G, B) 要除以 255

`transforms.RandomErasing(p=1, scale=(0.02, 0.33), ratio=(0.3, 3.3), value=(254/255, 0, 0))`的效果如下，从`scale=(0.02, 0.33)`中随机选择遮挡面积的比例，从`ratio=(0.3, 3.3)`中随机选择一个遮挡区域的长宽比，value 设置的 RGB 值需要归一化到 0~1 之间。

`transforms.RandomErasing(p=1, scale=(0.02, 0.33), ratio=(0.3, 3.3), value='timerring')`的效果如下，**value 设置任意的字符串**，就会**使用随机的值**填充遮挡区域。

### transforms.Lambda

`transforms . Lambda ( lambd)` lambd: Lambda匿名函数。

**功能：自定义 transform 方法**。

`lambda [arg1 [arg2, ... , argn]] : expression` 冒号前面是输入的参数，后面是处理的表达式，类似于return的意义。

> 例如在上面的`FiveCrop`中就用到了`transforms.Lambda`。
>
> ```scss
> transforms.FiveCrop(112, vertical_flip=False),
> transforms.Lambda(lambda crops: torch.stack([(transforms.ToTensor()(crop)) for crop in crops]))
> ```
>
> `transforms.FiveCrop`返回的是长度为 5 的 tuple，因此需要使用`transforms.Lambda` 把 tuple 转换为 4D 的 tensor。

## transforms 的组合与选择

### torchvision.transforms.RandomChoice

```css
torchvision.transforms.RandomChoice([transforms1, transforms2, transforms3])
```

功能：从一系列 transforms 方法中随机选择一个

### transforms.RandomApply

```scss
torchvision.transforms.RandomApply([transforms1, transforms2, transforms3], p=0.5)
```

功能：根据概率**执行一组** transforms 操作，要么全部执行，要么全部不执行。

### transforms.RandomOrder

```css
transforms.RandomOrder([transforms1, transforms2, transforms3])
```

对一组 transforms 操作打乱顺序。

## 自定义transforms

自定义 transforms 两要素：

+ 仅接受一个参数，返回一个参数；
+ 注意上下游的输入与输出，上一个 transform 的输出是下一个 transform 的输入。

实现椒盐噪声。椒盐噪声又称为脉冲噪声，是一种随机出现的白点或者黑点，白点称为盐噪声，黑点称为椒噪声。信噪比 (Signal-Noise Rate，SNR) 是衡量噪声的比例，图像中正常像素占全部像素的占比。

定义一个`AddPepperNoise`类，作为添加椒盐噪声的 transform。在构造函数中传入信噪比和概率，在`__call__()`函数中执行具体的逻辑，返回的是 image。

```python
import numpy as np
import random
from PIL import Image

# 自定义添加椒盐噪声的 transform
class AddPepperNoise(object):
    """增加椒盐噪声
    Args:
        snr （float）: Signal Noise Rate
        p (float): 概率值，依概率执行该操作
    """

    def __init__(self, snr, p=0.9):
        assert isinstance(snr, float) or (isinstance(p, float))
        self.snr = snr
        self.p = p

    # transform 会调用该方法
    def __call__(self, img):
        """
        Args:
            img (PIL Image): PIL Image
        Returns:
            PIL Image: PIL image.
        """
        # 如果随机概率小于 seld.p，则执行 transform
        if random.uniform(0, 1) < self.p:
            # 把 image 转为 array
            img_ = np.array(img).copy()
            # 获得 shape
            h, w, c = img_.shape
            # 信噪比
            signal_pct = self.snr
            # 椒盐噪声的比例 = 1 -信噪比
            noise_pct = (1 - self.snr)
            # 选择的值为 (0, 1, 2)，每个取值的概率分别为 [signal_pct, noise_pct/2., noise_pct/2.]
            # 椒噪声和盐噪声分别占 noise_pct 的一半
            # 1 为盐噪声，2 为 椒噪声
            mask = np.random.choice((0, 1, 2), size=(h, w, 1), p=[signal_pct, noise_pct/2., noise_pct/2.])
            mask = np.repeat(mask, c, axis=2)
            img_[mask == 1] = 255   # 盐噪声
            img_[mask == 2] = 0     # 椒噪声
            # 再转换为 image
            return Image.fromarray(img_.astype('uint8')).convert('RGB')
        # 如果随机概率大于 seld.p，则直接返回原图
        else:
            return img
```

然后直接通过 `AddPepperNoise` 调用即可。

完整代码如下：

```python
# -*- coding: utf-8 -*-
import os
BASE_DIR = os.path.dirname(os.path.abspath(__file__))
import numpy as np
import torch
import random
import torchvision.transforms as transforms
from PIL import Image
from matplotlib import pyplot as plt
from torch.utils.data import DataLoader

path_lenet = os.path.abspath(os.path.join(BASE_DIR, "..", "..", "model", "lenet.py"))
path_tools = os.path.abspath(os.path.join(BASE_DIR, "..", "..", "tools", "common_tools.py"))
assert os.path.exists(path_lenet), "{}不存在，请将lenet.py文件放到 {}".format(path_lenet, os.path.dirname(path_lenet))
assert os.path.exists(path_tools), "{}不存在，请将common_tools.py文件放到 {}".format(path_tools, os.path.dirname(path_tools))

import sys
hello_pytorch_DIR = os.path.abspath(os.path.dirname(__file__)+os.path.sep+".."+os.path.sep+"..")
sys.path.append(hello_pytorch_DIR)

from tools.my_dataset import RMBDataset
from tools.common_tools import set_seed, transform_invert

set_seed(1)  # 设置随机种子

# 参数设置
MAX_EPOCH = 10
BATCH_SIZE = 1
LR = 0.01
log_interval = 10
val_interval = 1
rmb_label = {"1": 0, "100": 1}


class AddPepperNoise(object):
    """增加椒盐噪声
    Args:
        snr （float）: Signal Noise Rate
        p (float): 概率值，依概率执行该操作
    """

    def __init__(self, snr, p=0.9):
        assert isinstance(snr, float) and (isinstance(p, float))    # 2020 07 26 or --> and
        self.snr = snr
        self.p = p

    def __call__(self, img):
        """
        Args:
            img (PIL Image): PIL Image
        Returns:
            PIL Image: PIL image.
        """
        if random.uniform(0, 1) < self.p:
            img_ = np.array(img).copy()
            h, w, c = img_.shape
            # 信号的百分比
            signal_pct = self.snr
            # 噪声的百分比
            noise_pct = (1 - self.snr)
            # 通过0，1，2表示具体的选择
            mask = np.random.choice((0, 1, 2), size=(h, w, 1), p=[signal_pct, noise_pct/2., noise_pct/2.])
            mask = np.repeat(mask, c, axis=2)
            img_[mask == 1] = 255   # 盐噪声 白色的
            img_[mask == 2] = 0     # 椒噪声 黑色的
            return Image.fromarray(img_.astype('uint8')).convert('RGB')
        else:
            return img


# ============================ step 1/5 数据 ============================
split_dir = os.path.abspath(os.path.join(BASE_DIR, "..", "..", "data", "rmb_split"))
if not os.path.exists(split_dir):
    raise Exception(r"数据 {} 不存在, 回到lesson-06\1_split_dataset.py生成数据".format(split_dir))
train_dir = os.path.join(split_dir, "train")
valid_dir = os.path.join(split_dir, "valid")

norm_mean = [0.485, 0.456, 0.406]
norm_std = [0.229, 0.224, 0.225]


train_transform = transforms.Compose([
    transforms.Resize((224, 224)),
    AddPepperNoise(0.9, p=0.5),
    transforms.ToTensor(),
    transforms.Normalize(norm_mean, norm_std),
])

valid_transform = transforms.Compose([
    transforms.Resize((224, 224)),
    transforms.ToTensor(),
    transforms.Normalize(norm_mean, norm_std)
])

# 构建MyDataset实例
train_data = RMBDataset(data_dir=train_dir, transform=train_transform)
valid_data = RMBDataset(data_dir=valid_dir, transform=valid_transform)

# 构建DataLoder
train_loader = DataLoader(dataset=train_data, batch_size=BATCH_SIZE, shuffle=True)
valid_loader = DataLoader(dataset=valid_data, batch_size=BATCH_SIZE)


# ============================ step 5/5 训练 ============================
for epoch in range(MAX_EPOCH):
    for i, data in enumerate(train_loader):

        inputs, labels = data   # B C H W

        img_tensor = inputs[0, ...]     # C H W
        img = transform_invert(img_tensor, train_transform)
        plt.imshow(img)
        plt.show()
        plt.pause(0.5)
        plt.close()
```

最后总结一下数据增强的transforms方法：

一、裁剪

+ transforms.CenterCrop
+ transforms.RandomCrop
+ transforms.RandomResizedCrop
+ transforms.FiveCrop
+ transforms.TenCrop

二、翻转和旋转

+ transforms.RandomHorizontalFlip
+ transforms.RandomVerticalFlip
+ transforms.RandomRotation

三、图像变换

+ transforms.Pad
+ transforms.ColorJitter
+ transforms.Grayscale
+ transforms.RandomGrayscale
+ transforms.RandomAffine
+ transforms.LinearTransformation
+ transforms.RandomErasing
+ transforms.Lambda
+ transforms.Resize
+ transforms.Totensor
+ transforms.Normalize

四、transforms 的操作

+ transforms.RandomChoice
+ transforms.RandomApply
+ transforms.RandomOrder

强调一下数据增强的原则：让训练集与测试集更接近。无论是从位置上，还是灰度上，还是变换填充等等，都要向这个方向去做。