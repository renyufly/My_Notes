PyTorch学习

From：[小土堆【PyTorch教程】bilibili](https://www.bilibili.com/video/BV1hE411t7RN?p=1&vd_source=bf4c8577fb22820a80ba9f8a7e04169a)



## PyTorch安装与启动

1.安装

不用换镜像源，

直接开全局VPN用pip下载（conda由于网络有问题）

[torch.cuda.is_available()返回false——解决办法-CSDN博客](https://blog.csdn.net/qq_46126258/article/details/112708781)

Anaconda中创建虚拟环境，安装PyTorch

2.启动

PyCharm中创建新项目时环境解释器选择conda，

使用`dir()` 与 `help()` 来查看package里有哪些函数以及对应作用。    示例：

```python
dir(torch)         # 打开torch里面的内容

dir(torch.cuda)    # 打开torch.cuda里面的内容

help(torch.cuda.is_available)  # 查询  torch.cuda.is_available() 的函数用法  [解释来自官方文档]
			# 注意is_available()函数在使用help查询时要把里面的括号去掉  
```

使用PyCharm底部栏中的`Python Console` 或者 新建 `Python file` 来运行代码

```shell
conda activate pytorch   // 启动叫"pytorch"的虚拟环境
jupyter notebook    // 打开jupyter
```

在Jupyter中，`shift + Enter`才进入下一块。

运行顺序对比：

Python文件的块是所有代码的整体；   【通用，适用于大型项目；   每次要重新运行整体】

Python Console以每一行为块；            【不利于代码阅读与修改】

Jupyter中以任意行为块运行；		【利于代码阅读及修改 ；  运行时环境需配置】



## PyCharm一些小tips

`ctrl + P`在函数的参数括号里点击可查看对应参数要求；

`ctrl + /` 可把框选的代码变成注释形式；

在PyCharm的左侧边栏点开`Structure`即可查看文件中的class以及方法函数；

针对import导入的不懂的类，在PyCharm中直接按住`ctrl`，然后鼠标点击相应的名字，可查看py文件；





## 数据加载-Dataset

Dataset：提供一种方式去获取数据及其对应的`label`

Dataloader：为后面的网络提供不同的数据形式

**Dataset:**

```python
from torch.utils.data import Dataset

from PIL import Image    # 读取图片  PIL (Python Image Library)，Python自带的一个强大的图片操作相关库
import os     # 系统相关的库

class MyData(Dataset):

    def __init__(self, root_dir, label_dir):
        self.root_dir = root_dir
        self.label_dir = label_dir
        self.path = os.path.join(self.root_dir, self.label_dir)
        self.img_path = os.listdir(self.path)


    def __getitem__(self, idx):    # 第二个参数一般改名叫idx，即index
        img_name = self.img_path[idx]

        img_item_path = os.path.join(self.root_dir, self.label_dir, img_name)

        img = Image.open(img_item_path)  # Image.open()打开的图片格式就为 PIL 类型

        label = self.label_dir

        return img, label

    def __len__(self):
        return len(self.img_path)


root_dir = "dataset/train"     # 路径最好都用'/'   [windows中'\'会是转义符,需要写成'\\']
ants_label_dir="ants"
bees_label_dir = "bees"
ants_dataset = MyData(root_dir, ants_label_dir)   # 创建实例
# ants_dataset[0]: (<PIL.JpegImagePlugin.JpegImageFileimage mode=RGB size=768x512 at 0x26F03A17080>, ants'>)
bees_dataset = MyData(root_dir, bees_label_dir)

train_dataset = ants_dataset + bees_dataset   # 拼接两个数据集
```



# 数据可视化-Tensorboard

> TensorBoard 是一组用于数据可视化的工具。它包含在流行的开源机器学习库 Tensorflow 中。TensorBoard 的主要功能包括：
>
> - 可视化模型的网络架构
> - 跟踪模型指标，如损失和准确性等
> - 检查机器学习工作流程中权重、偏差和其他组件的直方图
> - 显示非表格数据，包括图像、文本和音频
> - 将高维嵌入投影到低维空间
>
> TensorBoard刚出现时只能用于检查TensorFlow的指标和TensorFlow模型的可视化，但后来经过多方努力其他深度学习框架也可使用TensorBoard的功能，例如Pytorch已经抛弃了自家的visdom 而全面支持TensorBoard。

**add_scalar()的使用**

```python
from torch.utils.tensorboard import SummaryWriter

writer = SummaryWriter("logs")

for i in range(100):
    writer.add_scalar("y=2x", 2*i, i)   # (self,tag[标题], scalar_value[y轴], global_step=None[x轴], walltime=None)
    # writer.add_image()

writer.close()
```

> 如果修改了scalar_value，但未修改tag， 为了正确显示图像，需要要么把之前log files删掉，要么创建新的子文件夹来显示。

开启tensorboard的网页展示：  （在Pycharm内命令行控制台打开）

```shell
tensorboard --logdir=logs    # --logdir=事件文件所在文件夹名

tensorboard --logdir=logs --port 6007   # --port 指定端口
```

<img src="https://gitee.com/HYHZRYX/images/raw/master/imgs/202403022201065.png"  />

**add_image的使用**

PIL导入的Image打开的图片类型是`PIL.JpegImagePlugin.JpegImageFile` 不符合add_image()的参数要求。 可以通过`numpy.array()`，对PIL图片进行转换。  从PIL到numpy，需要在add_image()中指定shape中每一个数字/维度表示的含义。

利用opencv读取的图片类型就是numpy型，可满足参数要求。    `pip install opencv-python`

```python
from torch.utils.tensorboard import SummaryWriter
from PIL import Image
import numpy as np

writer = SummaryWriter("logs")

img_path = "data/train/ants_image/0013035.jpg"  # 相对地址
img_PIL = Image.open(img_path)
img_array = np.array(img_PIL)  # 把PIL类型转成ndarray再赋值

print(img_array.shape)   # 这里是 HWC 格式  (512, 768, 3)

writer.add_image("test", img_array, 1, dataformats="HWC")    # H-height,  W-width,  C-channel
                 # (self, tag, img_tensor, global_step=None, walltime=Nonde, dataformats="CHW")
								# 这里img_tensor要求 (torch.Tensor, numpy.ndarray, or string/blobname)的类型
writer.close()
```

![](https://gitee.com/HYHZRYX/images/raw/master/imgs/202403022228525.png)



## torchvision中的transforms

主要用来对图片进行变换

transforms使用：

PIL转入的图片格式

```python
from PIL import Image
from torchvision import transforms

img_path = "data/train/ants_image/0013035.jpg"
img = Image.open(img_path)

tensor_trans = transforms.ToTensor()  # ToTensor是个class，需要创建对应的实例来使用里面的方法;
tensor_img = tensor_trans(img)        #   不能直接 transforms.ToTensor(img) [Class必须先创建实例再使用里面method]

print(tensor_img)

'''
class ToTensor:
    """Convert a PIL Image 【使用PIL】 or ndarray 【使用opencv】 to tensor and scale the values accordingly.

    This transform does not support torchscript.

    Converts a PIL Image or numpy.ndarray (H x W x C) in the range
    [0, 255] to a torch.FloatTensor of shape (C x H x W) in the range [0.0, 1.0]
    if the PIL Image belongs to one of the modes (L, LA, P, I, F, RGB, YCbCr, RGBA, CMYK, 1)
    or if the numpy.ndarray has dtype = np.uint8

    In the other cases, tensors are returned without scaling.
''' 
```

OpenCV转入的numpy型图片格式：

```python
import cv2           # pip install opencv-python

img_path = "data/train/ants_image/0013035.jpg"

cv_img = cv2.imread(img_path)
#  此时cv_img 就是 ndarray类型
```

![](https://gitee.com/HYHZRYX/images/raw/master/imgs/202403022328713.png)



### tensor类型

可以简单理解为包装了神经网络所需要的一些参数(比如梯度、backward_hook、....)

在神经网络中肯定要转换成`tensor`类型来进行训练。



### 常见的transforms使用

**Python中`__call__()`函数的作用：**

下划线`__`表示属于内置函数；

```python
class Person:
	def __call__(self, name ) :      # 属于内置调用函数，可以直接在创建的实例里用参数 [注意这里是前后各2个下划线]
		print( " __call__"+" Hello " + name)
	
    def hello(self， name ) :
		print( " hello"+ name)

person = Person()  # 先创建一个实例

person("zhangsan" )    #  __call__ Hello zhangsan
person.hello("lisi")	#  hellolisi
```



**ToTensor的使用：**

“Convert a `PIL` Image or `ndarray `to tensor and scale the values accordingly.”

```python
from PIL import Image
from torch.utils.tensorboard import SummaryWriter
from torchvision import transforms

writer = SummaryWriter("logs")
img = Image.open("images/1.png")
print(img)

# ToTensor()使用
trans_totensor = transforms.ToTensor()  # 创建一个实例
img_tensor = trans_totensor(img)
writer.add_image("ToTensor", img_tensor)
writer.close()
```

> 还有 `transforms.ToPILImage()`  

**Normalize [归一化]**


“Normalize a  ` tensor`  image with mean and standard deviation.    This transform does not support PIL Image.”

```python
Given (mean[1],...,mean[n]) and (std[1],..,std[n]) for n channels, 
this transform will normalize each channel of the input ↓
# 公式：output[channel] = (input[channel] - mean[channel]) / std[channel] 
```

```python
from PIL import Image
from torchvision import transforms

img = Image.open("images/1.png")

trans_totensor = transforms.ToTensor()  # 创建一个实例
img_tensor = trans_totensor(img)

# Normalize - 归一化
trans_norm = transforms.Normalize([0.5, 0.5, 0.5], [0.5, 0.5, 0.5])   # mean-均值  std-标准差  [这里图片是3channel]
img_norm = trans_norm(img_tensor)  # 这里要求参数是tensor类型
```



**Resize**

“Resize the input image to the given size”

If size is a sequence like  (h, w), output size will be matched to this. If size is an int,  smaller edge of the image will be matched to this number.

```python
print(img.size)    # img是PIL类型
trans_resize = transforms.Resize((512,512))
# image PIL -> Resize -> image PIL
image_resize = trans_resize(img)   # Resize并不改变原图片类型

image_resize = trans_totensor(image_resize)  # 再手动将原PIL转成tensor
```





**Compose**

“Composes several transforms together. This transform does not support torchscript.”

Compose()中的参数需要是一个列表,  且需要是transforms类型, 所以 `Compose([transforms参数1, transforms参数2, ...])`

```python
trans_totensor = transforms.ToTensor()
trans_resize_2 = transforms.Resize(512)
# PIL -> resize -> PIL -> totensor -> tensor
trans_compose = transforms.Compose([trans_resize_2, trans_totensor])   # 组合执行一定要注意输入输出顺序
img_resize_2 = trans_compose(img_PIL)

''' 等价于分步执行 ↓ 
img_resize_2 = trans_resize_2(img_PIL)
img_resize_2 = trans_totensor(img_resize_2)
'''
```



**RandomCrop - 随机裁剪**

“Crop the given image at a random location.”  从原图片中每次随机裁剪出一个给定size大小的图片

 If size is an int instead of sequence like (h, w), a square crop` (size, size)` is made. If provided a sequence of length 1, it will be interpreted as `(size[0], size[0])`.

```python
trans_random = transforms.RandomCrop(512)   # or transforms.RandomCrop((500, 1000))
trans_compose_2 = transforms.Compose([trans_random, trans_totensor])   # RandomCrop不改变图片类型
for i in range(10):
    img_crop = trans_compose_2(img_PIL)   # 对原图片随机裁剪
    writer.add_image("RandomCrop", img_crop, i)   # 在TensorBoard中可查看每一步裁剪的图片
```



总结：

- 关注输入和输出类型
- 多看官方文档，或源码中的注释
- 关注方法需要什么参数 ,  尤其是 `__init()__` 里的参数
- 不知道返回值的时候`print + print(type()) + debug`



# torchvision使用内置数据集

> Torchvision 是 PyTorch 的一个独立子库，主要用于计算机视觉任务，包括图像处理、数据加载、数据增强、预训练模型等。
> Torchvision 提供了各种经典的计算机视觉数据集的加载器，如CIFAR-10、ImageNet，以及用于数据预处理和数据增强的工具，可以帮助用户更轻松地进行图像分类、目标检测、图像分割等任务。

在官网 `-> Docs -> PyTorch Domains -> torchvision -> `

[Datasets — Torchvision 0.18 documentation (pytorch.org)](https://pytorch.org/vision/stable/datasets.html)

```python
import torchvision

dataset_transform = torchvision.transforms.Compose([
    torchvision.transforms.ToTensor()
])

# 下载成功后不会重复下载 (Files already downloaded and verified)
train_set = torchvision.datasets.CIFAR10(root="./datasetCIFAR", train=True, transform=dataset_transform, download=True)  # 使用 transform=dataset_transform 后会先自动执行操作
test_set = torchvision.datasets.CIFAR10(root="./datasetCIFAR", train=False, transform=dataset_transform, download=True)
# download一直设置为True，这样如果本地把下载好的数据集压缩包放在"./datasetCIFAR"中，也能识别到不会重复下载并自动解压

print(test_set[0])  # 原始是(<PIL.Image.Image image mode=RGB size=32x32 at 0x242BE64D610>, 3)；
				  # 经过totensor转换后是(tensor([[[0.6196,...,0.4549],...], [[...]], ...]), 3)

img, target = test_set[0]

print(test_set.classes[target])  # "cat"

# img.show()
```



# DataLoader

在官网 `-> Docs -> PyTorch -> 搜索`

[torch.utils.data — PyTorch 2.3 documentation](https://pytorch.org/docs/stable/data.html#torch.utils.data.DataLoader)

```python
import torchvision
from torch.utils.data import DataLoader

#准备的测试数据集
test_data=torchvision.datasets.CIFAR10("./datasetCIFAR", train=False, transform=torchvision.transforms.ToTensor())
# 使用 DataLoader
test_loader=DataLoader(dataset=test_data, batch_size=4, shuffle=True, num_workers=0, drop_last=False)
	# dataset:要从中加载数据的数据集  shuffle:每个epoch是否打乱数据 num_workers:用于数据加载的子进程数
    # drop_last: True则设置为 如果数据集大小不能被批大小整除，就删除最后一个未完成的批处理。

#测试数据集中第一张图片及target
img, target = test_data[0]
print(img.shape)   #  torch.Size([3, 32, 32])    【图像是三通道】
print(target)      #   3    【表示对应分类序号是3号】

for epoch in range(2):      # 2轮epoch，每个epoch里会把所有batch看一遍
    step = 0
    for data in test_loader:
        imgs, targets = data
        print(imgs.shape)  # torch.Size([4, 3, 32, 32])  【因为batch_size=4，故每次以4张图片为一个单位。第一个4即size】
        print(targets)      # tensor([4, 4, 4, 4])  【表示batch里的4张图片每一张的对应分类序号】
```



# `nn.Module`介绍 & 使用

[torch.nn — PyTorch 2.3 documentation](https://pytorch.org/docs/stable/nn.html)

## 搭建自己的网络

```py
import torch
from torch import nn

# 定义自己的神经网络 ↓
class MyWork(nn.Module):
    def __init__(self):
        super().__init__()

    def forward(self, input):
        output = input +1    # 这里指对于该神经网络的每个输入input，会进行+1操作并输出output
        return output

# 使用自己的神经网络 ↓
mywork = MyWork()
x = torch.tensor(1.0)
output = mywork(x)  # x 为神经网络的输入
print(output)   # tensor(2.)
```



## 卷积层 `nn.Conv`

[torch.nn.functional.conv2d — PyTorch 2.3 documentation](https://pytorch.org/docs/stable/generated/torch.nn.functional.conv2d.html#torch.nn.functional.conv2d)     

[torch.nn.Conv2d  — PyTorch 2.3 documentation](https://pytorch.org/docs/stable/generated/torch.nn.Conv2d.html#torch.nn.Conv2d)          

> torch.nn 里面的类是 torch.nn.functional 中函数的封装，torch.nn 更抽象，是对 torch.nn.functional 更高一层的实现。
>
> 1. 具有学习参数的 （如 Conv2d, BatchNorm2d, Linear）采用` torch.nn `的方式，没有学习参数的（如 MaxPool2d, loss func, activation func）等根据个人选择使用 torch.nn.functional 或者 torch.nn 的方式。
> 2. 当模型较复杂时，推荐` torch.nn`；当其不能满足功能需求时，`torch.nn.functional `是最佳选择，如自定义模块时。因为其更接近底层，更加灵活。
> 3. 在前馈网络计算（forward）时，torch.nn.functional 中的参数无法保存，即没有状态参数。状态参数一般指的是可学习的参数，如卷积层的 weights 和 bias，或根据需要自定义的一些可学习参数。torch.nn.functional 中涉及的参数一般是超参，无法利用反向传播进行学习。
> 4. 关于 dropout，推荐使用 `torch.nn` 的方式。

 2D convolution  ↓

```python
'''
1 | 2 | 0 | 3 | 1                    1 | 2 | 1                                   10 | 12 | 12
0 | 1 | 2 | 3 | 1                    0 | 1 | 0                                   18 | 16 | 16
1 | 2 | 1 | 0 | 0                    2 | 1 | 0                                   13 | 9  | 3
5 | 2 | 3 | 1 | 1                    卷积核(3×3)								卷积后输出 (stride=1)
2 | 1 | 0 | 1 | 1
  输入图像(5×5)                            output[0][0] = 1*1 + 2*2 + 0*1 + 0*0 + 1*1 + 2*0 + 1*2 + 2*1 + 1*0 = 10
'''
```

```python
import torch
import torch.nn.functional as F

input = torch.tensor([[1,2,0,3,1],
                      [0,1,2,3,1],
                      [1,2,1,0,0],
                      [5,2,3,1,1],
                      [2,1,0,1,1]])  # 二维矩阵

kernel = torch.tensor([[1,2,1],
                       [0,1,0],
                       [2,1,0]])    # 卷积核 - kernel

print(input.shape)    # torch.Size([5, 5])
input = torch.reshape(input, (1, 1, 5, 5))
kernel = torch.reshape(kernel, (1, 1, 3, 3))

output = F.conv2d(input, kernel, stride=1)
print(output)
'''tensor([[[[10, 12, 12],
             [18, 16, 16],
             [13,  9,  3]]]])'''

output2 = F.conv2d(input, kernel, stride=1, padding=1)   # 在原input图像上下左右四周各填充一行 (默认的padding为零填充)
print(output2)
'''tensor([[[[ 1,  3,  4, 10,  8],
             [ 5, 10, 12, 12,  6],
             [ 7, 18, 16, 16,  8],
             [11, 13,  9,  3,  4],
             [14, 13,  9,  7,  4]]]])'''
```



Parameters

- **in_channels** ([*int*](https://docs.python.org/3/library/functions.html#int)) – Number of channels in the input image        【输入图像的通道数】
- **out_channels** ([*int*](https://docs.python.org/3/library/functions.html#int)) – Number of channels produced by the convolution
- **kernel_size** ([*int*](https://docs.python.org/3/library/functions.html#int) *or* [*tuple*](https://docs.python.org/3/library/stdtypes.html#tuple)) – Size of the convolving kernel     【卷积核大小.  3：3×3大小    (1,2)： 1×2大小】
- **stride** ([*int*](https://docs.python.org/3/library/functions.html#int) *or* [*tuple*](https://docs.python.org/3/library/stdtypes.html#tuple)*,* *optional*) – Stride of the convolution. Default: 1
- **padding** ([*int*](https://docs.python.org/3/library/functions.html#int)*,* [*tuple*](https://docs.python.org/3/library/stdtypes.html#tuple) *or* [*str*](https://docs.python.org/3/library/stdtypes.html#str)*,* *optional*) – Padding added to all four sides of the input. Default: 0
- **padding_mode** ([*str*](https://docs.python.org/3/library/stdtypes.html#str)*,* *optional*) – `'zeros'`, `'reflect'`, `'replicate'` or `'circular'`. Default: `'zeros'`
- **dilation** ([*int*](https://docs.python.org/3/library/functions.html#int) *or* [*tuple*](https://docs.python.org/3/library/stdtypes.html#tuple)*,* *optional*) – Spacing between kernel elements. Default: 1
- **groups** ([*int*](https://docs.python.org/3/library/functions.html#int)*,* *optional*) – Number of blocked connections from input channels to output channels. Default: 1
- **bias** ([*bool*](https://docs.python.org/3/library/functions.html#bool)*,* *optional*) – If `True`, adds a learnable bias to the output. Default: `True`

动画示意见 [conv_arithmetic   (github.com)](https://github.com/vdumoulin/conv_arithmetic/blob/master/README.md)

卷积核中的值会自己根据数学分布等采样调整，我们只需设置卷积核size

```py
import torch
import torchvision
from torch import nn
from torch.nn import Conv2d

from torch.utils.data import DataLoader
from torch.utils.tensorboard import SummaryWriter

#准备的测试数据集
test_data=torchvision.datasets.CIFAR10("./datasetCIFAR", train=False, transform=torchvision.transforms.ToTensor())
# 使用 DataLoader
test_loader=DataLoader(dataset=test_data, batch_size=64)

class MyWork(nn.Module):
    def __init__(self):
        super().__init__()
        # 自己的网络中定义了一个卷积层
        self.conv1 = Conv2d(in_channels=3, out_channels=6, kernel_size=3, stride=1, padding=0)
                                                        #  卷积核大小是3×3
    def forward(self, x):
        x = self.conv1(x)
        return x


mywork = MyWork()
print(mywork)
writer = SummaryWriter("./logs")
step = 0

for data in test_loader:
    imgs, target = data
    print("imgs: " , imgs.shape)  # imgs:  torch.Size([64, 3, 32, 32])
    output = mywork(imgs)
    print(output.shape)   # torch.Size([64, 6, 30, 30])

    writer.add_images("input", imgs, step)   # 注意使用的add_images 方法 不是 add_image方法

    output = torch.reshape(output, (-1, 3, 30, 30))  # 因为channel为6不能显示，所以转为3. -1表示自动处理该处数据
    writer.add_images("output", output, step)

    step = step + 1
```



## 池化层 `nn.Pool`

[Pooling Layers — PyTorch 2.3 documentation](https://pytorch.org/docs/stable/nn.html#pooling-layers)

常见如 [MaxPool2d — PyTorch 2.3 documentation](https://pytorch.org/docs/stable/generated/torch.nn.MaxPool2d.html#torch.nn.MaxPool2d)

最大池化操作 ↓

```py
'''                                                              【True表示当核覆盖小于9个数仍取覆盖数里最大的一个】   
1 | 2 | 0 | 3 | 1                    __|__|__                      Ceil_model=True    2 | 3     
0 | 1 | 2 | 3 | 1                    __|__|__                                         5 | 1 
1 | 2 | 1 | 0 | 0                    __|__|__                                
5 | 2 | 3 | 1 | 1                    池化核(3×3)				   Ceil_model=False 	2	
2 | 1 | 0 | 1 | 1
  输入图像(5×5)                                                        池化后输出 (stride=3)
                                       output[0][0] = 覆盖的9宫格数里最大的一个数，即2
'''
```

```py
import torch
from torch import nn
from torch.nn import MaxPool2d


input = torch.tensor([[1,2,0,3,1],
                      [0,1,2,3,1],
                      [1,2,1,0,0],
                      [5,2,3,1,1],
                      [2,1,0,1,1]], dtype=torch.float32)  # 二维矩阵

input = torch.reshape(input, (-1, 1, 5, 5))
print(input.shape)   # torch.Size([1, 1, 5, 5])

class MyWork(nn.Module):
    def __init__(self):
        super(MyWork, self).__init__()
        self.maxpool1 = MaxPool2d(kernel_size=3, ceil_mode=True)   # 定义最大池化层

    def forward(self, input):
        output = self.maxpool1(input)    # 对输入使用最大池化层
        return output

mywork = MyWork()
output = mywork(input)

print(output)
'''tensor([[[[2., 3.],
             [5., 1.]]]])
'''
```

最大池化（Max Pooling）是一种在卷积神经网络（CNN）中常用的池化操作，它的作用主要包括以下几点：

1. **降维**：最大池化通过减少特征图（feature map）的尺寸来降低数据的空间维度，这通常可以减少参数的数量和计算量，从而减少模型的复杂度。
2. **特征不变性**：最大池化有助于提高特征的不变性。这意味着即使输入图像中的物体发生小的位移或形变，最大池化仍然能够保留重要的特征信息。
3. **特征选择**：最大池化可以看作是一种特征选择机制，它选择每个池化窗口中的最大值，这通常代表了该窗口中最显著的特征。
4. **防止过拟合**：由于最大池化减少了特征图的尺寸，它有助于减少模型的过拟合风险，因为模型需要学习更加泛化的特征表示。
5. **保持特征层次结构**：最大池化在降低特征图尺寸的同时，保留了特征的层次结构，这对于后续的卷积层处理是有益的。
6. **加速计算**：由于特征图的尺寸减小，后续卷积层的计算量也会相应减少，从而加速了整个网络的前向传播和反向传播过程。

最大池化通常与卷积层交替使用，在提取特征的同时逐步降低特征图的尺寸，以实现有效的特征抽象和降维。常见的最大池化操作包括2x2的最大池化，其中每个2x2的窗口中的最大值被保留，窗口在特征图上滑动，直到覆盖整个特征图。

```py
import torch
import torchvision
from torch import nn
from torch.nn import MaxPool2d
from torch.utils.data import DataLoader
from torch.utils.tensorboard import SummaryWriter

test_data=torchvision.datasets.CIFAR10("./datasetCIFAR", train=False, transform=torchvision.transforms.ToTensor())
test_loader=DataLoader(dataset=test_data, batch_size=64)



class MyWork(nn.Module):
    def __init__(self):
        super(MyWork, self).__init__()
        self.maxpool1 = MaxPool2d(kernel_size=3, ceil_mode=True)

    def forward(self, input):
        output = self.maxpool1(input)

        return output

mywork = MyWork()

writer = SummaryWriter("./logs")
step = 0

for data in test_loader:
    imgs, target = data
    print("imgs: " , imgs.shape)  # imgs:  torch.Size([64, 3, 32, 32])
    output = mywork(imgs)
    writer.add_images("input", imgs, step)
    writer.add_images("output", output, step)

    step = step + 1

writer.close()
```



## 非线性激活 `nn.ReLU 等`

[Non-linear Activations — PyTorch 2.3 documentation](https://pytorch.org/docs/stable/nn.html#non-linear-activations-weighted-sum-nonlinearity)

```py
import torch
from torch import nn

input = torch.tensor([[1, -0.5],
                      [-1, 3]])

output = torch.reshape(input, (-1, 1, 2, 2))

print(output.shape)  # torch.Size([1, 1, 2, 2])

class MyWork(nn.Module):
    def __init__(self):
        super().__init__()
        self.relu1 = nn.ReLU()

    def forward(self, input):
        output = self.relu1(input)
        return output


mywork = MyWork()
output = mywork(input)
print(output)
'''tensor([[1., 0.],
           [0., 3.]])'''
```



## 线性层 `nn.Linear`

[linear-layers — PyTorch 2.3 documentation](https://pytorch.org/docs/stable/nn.html#linear-layers)

```py
#准备的测试数据集
import torch
import torchvision
from torch import nn
from torch.utils.data import DataLoader

test_data=torchvision.datasets.CIFAR10("./datasetCIFAR", train=False, transform=torchvision.transforms.ToTensor())
# 使用 DataLoader
test_loader=DataLoader(dataset=test_data, batch_size=64)

class MyWork(nn.Module):
    def __init__(self):
        super().__init__()
        self.linear1 = nn.Linear(196608, 10)   # 线性层

    def forward(self, input):
        output = self.linear1(input)
        return output

mywork = MyWork()

for data in test_loader:
    imgs, targets = data
    print(imgs.shape)      # torch.Size([64, 3, 32, 32])
    # output = torch.reshape(imgs, (1, 1, 1, -1))
    # print(output.shape)    # torch.Size([1, 1, 1, 196608])
    # output = mywork(output)
    # print(output.shape)    # torch.Size([1, 1, 1, 10])
    output = torch.flatten(imgs)
    print(output.shape)    # torch.Size([196608])
    output = mywork(output)
    print(output.shape)    # torch.Size([10])
```





## Sequential使用

[Sequential — PyTorch 2.3 documentation](https://pytorch.org/docs/stable/generated/torch.nn.Sequential.html#torch.nn.Sequential)

使用 `Sequential` 容器可以方便地堆叠多个神经网络层，而不需要显式地编写前向传播的代码。主要优点是简化了模型的定义和前向传播的代码。你不需要定义一个自定义的 `nn.Module` 子类并实现自己的 `forward` 方法。`Sequential` 会自动将输入数据传递到下一个模块。

```py
import torch
from torch import nn

class Cifar10Model(nn.Module):
    ''' Structure of CIFAR10-quick model. '''
    def __init__(self):
        super().__init__()
        # '''Inputs 3@32×32 -> Feature maps 32@32×32   [5×5 Convolution kernel]'''
        # self.conv1 = nn.Conv2d(3, 32, kernel_size=5, padding=2)  
        			# 【padding根据torch官网conv的shape公式利用in与out的size计算】
        # '''32@32×32 -> Feature maps 32@16×16  [2×2 Max-pooling kernel]'''
        # self.maxpool1 = nn.MaxPool2d(2)
        # '''32@16×16 -> Feature maps 32@16×16 [5×5 Convolution kernel]'''
        # self.conv2 = nn.Conv2d(32, 32, kernel_size=5, padding=2)
        # '''32@16×16 -> Feature maps 32@8×8  [2×2 Max-pooling kernel]'''
        # self.maxpool2 = nn.MaxPool2d(2)
        # '''32@8×8 -> Feature maps 64@8×8 [5×5 Convolution kernel]'''
        # self.conv3 = nn.Conv2d(32, 64, kernel_size=5, padding=2)
        # '''64@8×8 -> Feature maps 64@4×4  [2×2 Max-pooling kernel]'''
        # self.maxpool3 = nn.MaxPool2d(2)
        # '''64@4×4 ->  1024 = 64×4×4   [Flatten]'''
        # self.flatten = nn.Flatten()
        # '''1024 -> Hidden units 64'''
        # self.liear1 = nn.Linear(1024, 64)
        # '''64 -> outputs 10  [Fully connected]'''
        # self.linear2 = nn.Linear(64, 10)


        self.model1 = nn.Sequential(
            nn.Conv2d(3, 32, kernel_size=5, padding=2),
            nn.MaxPool2d(2),
            nn.Conv2d(32, 32, kernel_size=5, padding=2),
            nn.MaxPool2d(2),
            nn.Conv2d(32, 64, kernel_size=5, padding=2),
            nn.MaxPool2d(2),
            nn.Flatten(),
            nn.Linear(1024, 64),
            nn.Linear(64, 10)
        )

    def forward(self, x):
        # x = self.conv1(x)
        # x = self.maxpool1(x)

        x = self.model1(x)
        return x


mywork = Cifar10Model()
print(mywork)
'''Cifar10Model(
  (model1): Sequential(
    (0): Conv2d(3, 32, kernel_size=(5, 5), stride=(1, 1), padding=(2, 2))
    (1): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
    (2): Conv2d(32, 32, kernel_size=(5, 5), stride=(1, 1), padding=(2, 2))
    (3): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
    (4): Conv2d(32, 64, kernel_size=(5, 5), stride=(1, 1), padding=(2, 2))
    (5): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
    (6): Flatten(start_dim=1, end_dim=-1)
    (7): Linear(in_features=1024, out_features=64, bias=True)
    (8): Linear(in_features=64, out_features=10, bias=True)
  )
) '''
# 简单检验正确性
input = torch.ones((64, 3, 32, 32))  # 全一矩阵， 64是batch_size
output = mywork(input)
print(output.shape)   # torch.Size([64, 10])

# 借助tensorboard可视化查看模型
writer = SummaryWriter("./logs")
writer.add_graph(mywork, input)
writer.close()
```



## 损失函数与反向传播

[Loss Functions — PyTorch 2.3 documentation](https://pytorch.org/docs/stable/nn.html#loss-functions)

```py
import torch
from torch import nn

inputs =torch.tensor([1,2,3],dtype=torch.float32)
targets =torch.tensor([1,2,5],dtype=torch.float32)

inputs = torch.reshape(inputs, (1,1,1,3))
targets = torch.reshape(targets,(1,1,1,3))

loss1 = nn.L1Loss()
result = loss1(inputs, targets)
print(result)  # tensor(0.6667)

loss_mse = nn.MSELoss()
result_mse = loss_mse(inputs, targets)

print(result_mse)  # tensor(1.3333)

x = torch.tensor([0.1, 0.2, 0.3])
y = torch.tensor([1])
x= torch.reshape(x, (1, 3))
loss_cross = nn.CrossEntropyLoss()
result_cross = loss_cross(x, y)
print(result_cross)   # tensor(1.1019)
```

在神经网络中使用Loss function：

1. 计算实际输出和目标之间的差距

2. 为更新输出提供依据（反向传播），grad

```py
import torch
import torchvision
from torch import nn
from torch.utils.data import DataLoader

#准备的测试数据集
test_data=torchvision.datasets.CIFAR10("./datasetCIFAR", train=False, transform=torchvision.transforms.ToTensor())
# 使用 DataLoader
test_loader=DataLoader(dataset=test_data, batch_size=1)

class Cifar10Model(nn.Module):
    ''' Structure of CIFAR10-quick model. '''
    def __init__(self):
        super().__init__()
        self.model1 = nn.Sequential(
            nn.Conv2d(3, 32, kernel_size=5, padding=2),
            nn.MaxPool2d(2),
            nn.Conv2d(32, 32, kernel_size=5, padding=2),
            nn.MaxPool2d(2),
            nn.Conv2d(32, 64, kernel_size=5, padding=2),
            nn.MaxPool2d(2),
            nn.Flatten(),
            nn.Linear(1024, 64),
            nn.Linear(64, 10)
        )

    def forward(self, x):
        x = self.model1(x)
        return x


mywork = Cifar10Model()
# 分类用交叉熵
loss = nn.CrossEntropyLoss()

for data in test_loader:
    imgs, target = data
    output = mywork(imgs)
    print(output)  # tensor([[-0.0446,  0.0080, -0.0441, -0.1025,  0.0813, -0.0743,  0.0115,  0.1031, -0.0128,  0.0516]], grad_fn=<AddmmBackward0>)
    print(target)  # tensor([7])

    result_loss = loss(output, target)
    print(result_loss)  # tensor(2.2167, grad_fn=<NllLossBackward0>)
    # 计算出grad
    result_loss.backward() 
```



## 优化器

[torch.optim — PyTorch 2.3 documentation](https://pytorch.org/docs/stable/optim.html)

```py
import torch
import torchvision
from torch import nn
from torch.utils.data import DataLoader

#准备的测试数据集
test_data=torchvision.datasets.CIFAR10("./datasetCIFAR", train=False, transform=torchvision.transforms.ToTensor())
# 使用 DataLoader
test_loader=DataLoader(dataset=test_data, batch_size=1)

class Cifar10Model(nn.Module):
    ''' Structure of CIFAR10-quick model. '''
    def __init__(self):
        super().__init__()
        self.model1 = nn.Sequential(
            nn.Conv2d(3, 32, kernel_size=5, padding=2),
            nn.MaxPool2d(2),
            nn.Conv2d(32, 32, kernel_size=5, padding=2),
            nn.MaxPool2d(2),
            nn.Conv2d(32, 64, kernel_size=5, padding=2),
            nn.MaxPool2d(2),
            nn.Flatten(),
            nn.Linear(1024, 64),
            nn.Linear(64, 10)
        )

    def forward(self, x):
        x = self.model1(x)
        return x


mywork = Cifar10Model()

loss = nn.CrossEntropyLoss()

optim = torch.optim.SGD(mywork.parameters(), lr=0.01)
# 一共进行20轮所有数据的查看
for epoch in range(20):
    running_loss = 0.0
    for data in test_loader:
        imgs, target = data
        output = mywork(imgs)
        result_loss = loss(output, target)
        # 每次梯度清零 （上一次的梯度对这次的梯度更新没用）
        optim.zero_grad()
        # 计算出grad
        result_loss.backward()
        # 调用优化器进行参数调优
        optim.step()
        running_loss = running_loss + result_loss
    print(running_loss)
'''tensor(18816.8086, grad_fn=<AddBackward0>)
   tensor(16220.7354, grad_fn=<AddBackward0>)'''
```



# torchvision内置model使用

[Models and pre-trained weights — Torchvision 0.18 documentation (pytorch.org)](https://pytorch.org/vision/stable/models.html)

```py
import torch
import torchvision.models
from torch import nn

vgg16_false = torchvision.models.vgg16()

print(vgg16_false)
'''VGG(
  (features): Sequential(
    (0): Conv2d(3, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
    (1): ReLU(inplace=True)
    (2): Conv2d(64, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
    ...
    (29): ReLU(inplace=True)
    (30): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
  )
  (avgpool): AdaptiveAvgPool2d(output_size=(7, 7))
  (classifier): Sequential(
    (0): Linear(in_features=25088, out_features=4096, bias=True)
    (1): ReLU(inplace=True)
    ...
    (5): Dropout(p=0.5, inplace=False)
    (6): Linear(in_features=4096, out_features=1000, bias=True)
  )
)
'''
# 添加层
vgg16_false.add_module('add_linear', nn.Linear(1000, 10))   

print(vgg16_false)
'''VGG(
  (features): Sequential(
    (0): Conv2d(3, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
    (1): ReLU(inplace=True)
     ...
    (30): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
  )
  (avgpool): AdaptiveAvgPool2d(output_size=(7, 7))
  (classifier): Sequential(
    (0): Linear(in_features=25088, out_features=4096, bias=True)
    (1): ReLU(inplace=True)
    ...
    (5): Dropout(p=0.5, inplace=False)
    (6): Linear(in_features=4096, out_features=1000, bias=True)
  )
  (add_linear): Linear(in_features=1000, out_features=10, bias=True)    # 手动添加了一层
)'''

vgg16_false.classifier.add_module('cadd_linear', nn.Linear(1000, 10))
'''VGG(
  (features): Sequential(
    (0): Conv2d(3, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
    ...
    (29): ReLU(inplace=True)
    (30): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
  )
  (avgpool): AdaptiveAvgPool2d(output_size=(7, 7))
  (classifier): Sequential(
    (0): Linear(in_features=25088, out_features=4096, bias=True)
    ...
    (6): Linear(in_features=4096, out_features=1000, bias=True)
    (cadd_linear): Linear(in_features=1000, out_features=10, bias=True)   # 在(classifier)里加了一层
  )
)
'''
# 修改层
vgg16_false.classifier[6] = nn.Linear(4096, 10)
'''(classifier): Sequential(
    (0): Linear(in_features=25088, out_features=4096, bias=True)
    (1): ReLU(inplace=True)
    (2): Dropout(p=0.5, inplace=False)
    (3): Linear(in_features=4096, out_features=4096, bias=True)
    (4): ReLU(inplace=True)
    (5): Dropout(p=0.5, inplace=False)
    (6): Linear(in_features=4096, out_features=10, bias=True)     # 原有的一层被修改
  )
)'''
```



# 模型的保存&读取

```py
import torch
import torchvision.models
from torch import nn

vgg16 = torchvision.models.vgg16()

# 方式1：即保存模型结构又保存参数  【但对于自己Class定义的模型，在别的文件使用方式1读取还需要再定义(或者使用from import导入自己定义的模型)】
torch.save(vgg16, "vgg16_method1.pth")

# 对应方式1的读取
torch.load("vgg16_method1.pth")


# 方式2：以字典形式保存模型参数（官方推荐）
torch.save(vgg16.state_dict(), "vgg16_method2.pth")

# 对应方式2的加载   // 另一个文件中
vgg16_2 = torchvision.models.vgg16()            # 先创建模型
vgg16_2.load_state_dict(torch.load("vgg16_method2.pth"))   # 再加载参数
```



# 完整的模型训练套路



```py
'''train.py'''
import torch
import torchvision

from torch.utils.data import DataLoader
from torch.utils.tensorboard import SummaryWriter

from model import *

writer = SummaryWriter("./logs")

#准备数据集
train_data = torchvision.datasets.CIFAR10("./datasetCIFAR", train=True, transform=torchvision.transforms.ToTensor())
test_data = torchvision.datasets.CIFAR10("./datasetCIFAR", train=False, transform=torchvision.transforms.ToTensor())

# 查看数据集大小
train_data_size = len(train_data)
test_data_size = len(test_data)

print("训练集长度：{}".format(train_data_size))
print("测试集长度：{}".format(test_data_size))

# DataLoader
train_dataloader = DataLoader(train_data, batch_size=64)
test_dataloader = DataLoader(test_data, batch_size=64)

# 搭建神经网络
'''通常在另一个文件model.py中定义搭建神经网络，然后通过import导入'''

# 创建网络模型
mywork = MyNetwork()

# 定义损失函数
loss_fn = nn.CrossEntropyLoss()

# 定义优化器
# learning_rate = 0.01
learning_rate = 1e-2
optimizer = torch.optim.SGD(mywork.parameters(), lr=learning_rate)

# 设置参数
# 训练次数
total_train_step = 0
# 记录测试的次数
total_test_step = 0
# 训练的轮数
epoch = 10

for i in range(epoch):
    print("------第{}轮训练开始-------".format(i+1))

    # 训练步骤开始
    
    mywork.train()  # 
    
    for data in train_dataloader:      # 从dataloader里取数据
        imgs, targets = data
        outputs = mywork(imgs)
        loss = loss_fn(outputs, targets)

        # 优化器优化模型
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()

        total_train_step = total_train_step + 1
        if total_train_step % 100 == 0:      # 不每步都打印
            print("训练次数：{}， Loss：{}".format(total_train_step, loss.item()))  # 使用.item()
            writer.add_scalar("train_loss", loss.item(), total_train_step)


    # 测试步骤开始
    
    mywork.eval()     # 
    
    total_test_loss = 0
    total_accuracy = 0
    with torch.no_grad():     # 可以节约内存与性能
        for data in test_dataloader:
            imgs, targets = data
            outputs = mywork(imgs)
            loss = loss_fn(outputs, targets)
            total_test_loss = total_test_loss + loss.item()
            
            accuracy = (outputs.argmax(1) == targets).sum
            total_accuracy = total_accuracy + accuracy

    print("整体loss：{}".format(total_test_loss))
    print("整体测试集上的正确率：{}".format(total_accuracy / test_data_size))
    writer.add_scalar("test_loss", loss, total_test_step)
    total_test_step = total_test_step + 1

    # 每一轮结束后保存参数
    torch.save(mywork, "mywork_{}.pth".format(i+1))


writer.close()


'''model.py ↓'''
import torch
from torch import nn

# 搭建神经网络
class MyNetwork(nn.Module):
    ''' Structure of CIFAR10-quick model. '''
    def __init__(self):
        super().__init__()
        self.model1 = nn.Sequential(
            nn.Conv2d(3, 32, kernel_size=5, stride=1, padding=2),
            nn.MaxPool2d(2),
            nn.Conv2d(32, 32, kernel_size=5, stride=1, padding=2),
            nn.MaxPool2d(2),
            nn.Conv2d(32, 64, kernel_size=5, stride=1, padding=2),
            nn.MaxPool2d(2),
            nn.Flatten(),
            nn.Linear(64*4*4, 64),
            nn.Linear(64, 10)
        )

    def forward(self, x):
        x = self.model1(x)
        return x

if __name__ == '__main__':
    mywork = MyNetwork()
    input = torch.ones((64, 3, 32, 32))
    output = mywork(input)
    print(output.shape)
```



注意点：

关于`.train() ` 与 `.eval()`，  参考官方文档 [Module — PyTorch 2.3 documentation](https://pytorch.org/docs/stable/generated/torch.nn.Module.html#torch.nn.Module)  ，只对网络中特定层有效。（如果有一定要使用train与eval）

> train(*mode=True*)[[SOURCE\]](https://pytorch.org/docs/stable/_modules/torch/nn/modules/module.html#Module.train)
>
> Set the module in training mode.
>
> This **has any effect only on certain modules**. See documentations of particular modules for details of their behaviors in training/evaluation mode, **if they are affected, e.g. `Dropout`, `BatchNorm`, etc.**
>
> - Parameters
>
> 	**mode** (*bool*) – whether to set training mode (`True`) or evaluation mode (`False`). Default: `True`.
>
> - Returns
>
> 	self
>
> - Return type
>
> 	Module





# 使用GPU训练

## 方法1.使用`.cuda()`

找到 网络模型、数据（输入数据&标注数据）、损失函数；  然后在后面调用`.cuda()` 再返回 即可。

```py
'''train.py'''
import torch
import torchvision

from torch.utils.data import DataLoader
from torch.utils.tensorboard import SummaryWriter

from model import *

writer = SummaryWriter("./logs")

#准备数据集
train_data = torchvision.datasets.CIFAR10("./datasetCIFAR", train=True, transform=torchvision.transforms.ToTensor())
test_data = torchvision.datasets.CIFAR10("./datasetCIFAR", train=False, transform=torchvision.transforms.ToTensor())

# 查看数据集大小
train_data_size = len(train_data)
test_data_size = len(test_data)

print("训练集长度：{}".format(train_data_size))
print("测试集长度：{}".format(test_data_size))

# DataLoader
train_dataloader = DataLoader(train_data, batch_size=64)
test_dataloader = DataLoader(test_data, batch_size=64)

# 搭建神经网络
class MyNetwork(nn.Module):
    ''' Structure of CIFAR10-quick model. '''
    def __init__(self):
        super().__init__()
        self.model1 = nn.Sequential(
            nn.Conv2d(3, 32, kernel_size=5, stride=1, padding=2),
            nn.MaxPool2d(2),
            nn.Conv2d(32, 32, kernel_size=5, stride=1, padding=2),
            nn.MaxPool2d(2),
            nn.Conv2d(32, 64, kernel_size=5, stride=1, padding=2),
            nn.MaxPool2d(2),
            nn.Flatten(),
            nn.Linear(64*4*4, 64),
            nn.Linear(64, 10)
        )

    def forward(self, x):
        x = self.model1(x)
        return x

# 创建网络模型
mywork = MyNetwork()

if torch.cuda.is_available():
	mywork = mywork.cuda()    ## GPU —— 网络模型

# 定义损失函数
loss_fn = nn.CrossEntropyLoss()

if torch.cuda.is_available():
	loss_fn = loss_fn.cuda()    ## GPU —— 损失函数

# 定义优化器
# learning_rate = 0.01
learning_rate = 1e-2
optimizer = torch.optim.SGD(mywork.parameters(), lr=learning_rate)

# 设置参数
# 训练次数
total_train_step = 0
# 记录测试的次数
total_test_step = 0
# 训练的轮数
epoch = 10

for i in range(epoch):
    print("------第{}轮训练开始-------".format(i+1))

    # 训练步骤开始
    mywork.train()  # 
    
    for data in train_dataloader:      # 从dataloader里取数据
        imgs, targets = data
        if torch.cuda.is_available():
	        imgs = imgs.cuda()          ## GPU —— 数据
    	    targets = targets.cuda()    ## GPU —— 数据
        
        outputs = mywork(imgs)
        loss = loss_fn(outputs, targets)

        # 优化器优化模型
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()

        total_train_step = total_train_step + 1
        if total_train_step % 100 == 0:      # 不每步都打印
            print("训练次数：{}， Loss：{}".format(total_train_step, loss.item()))  # 使用.item()
            writer.add_scalar("train_loss", loss.item(), total_train_step)


    # 测试步骤开始
    
    mywork.eval()
    
    total_test_loss = 0
    total_accuracy = 0
    with torch.no_grad():
        for data in test_dataloader:
            imgs, targets = data
            if torch.cuda.is_available():
	            imgs = imgs.cuda()          ## GPU —— 数据
    	    	targets = targets.cuda()    ## GPU —— 数据
        
            outputs = mywork(imgs)
            loss = loss_fn(outputs, targets)
            total_test_loss = total_test_loss + loss.item()
            
            accuracy = (outputs.argmax(1) == targets).sum
            total_accuracy = total_accuracy + accuracy

    print("整体loss：{}".format(total_test_loss))
    print("整体测试集上的正确率：{}".format(total_accuracy / test_data_size))
    writer.add_scalar("test_loss", loss, total_test_step)
    total_test_step = total_test_step + 1

    # 每一轮结束后保存参数
    torch.save(mywork, "mywork_{}.pth".format(i+1))


writer.close()
```



也可寻找免费或付费的GPU使用平台，比如[Google - Colab (google.com)](https://colab.research.google.com/)、



## 方法2.使用`.to(device)`



```py
'''train.py'''
import torch
import torchvision

from torch.utils.data import DataLoader
from torch.utils.tensorboard import SummaryWriter

from model import *


## 定义训练的设备
# device = torch.device("cpu")     # 指定CPU
# device = torch.device("cuda:0")  # 指定第一个GPU
device = torch.device("cuda")    

常写成：device = torch.device("cuda" if torch.cuda.is_available() else "cpu")   


writer = SummaryWriter("./logs")

#准备数据集
train_data = torchvision.datasets.CIFAR10("./datasetCIFAR", train=True, transform=torchvision.transforms.ToTensor())
test_data = torchvision.datasets.CIFAR10("./datasetCIFAR", train=False, transform=torchvision.transforms.ToTensor())

# 查看数据集大小
train_data_size = len(train_data)
test_data_size = len(test_data)

print("训练集长度：{}".format(train_data_size))
print("测试集长度：{}".format(test_data_size))

# DataLoader
train_dataloader = DataLoader(train_data, batch_size=64)
test_dataloader = DataLoader(test_data, batch_size=64)

# 搭建神经网络
class MyNetwork(nn.Module):
    ''' Structure of CIFAR10-quick model. '''
    def __init__(self):
        super().__init__()
        self.model1 = nn.Sequential(
            nn.Conv2d(3, 32, kernel_size=5, stride=1, padding=2),
            nn.MaxPool2d(2),
            nn.Conv2d(32, 32, kernel_size=5, stride=1, padding=2),
            nn.MaxPool2d(2),
            nn.Conv2d(32, 64, kernel_size=5, stride=1, padding=2),
            nn.MaxPool2d(2),
            nn.Flatten(),
            nn.Linear(64*4*4, 64),
            nn.Linear(64, 10)
        )

    def forward(self, x):
        x = self.model1(x)
        return x

# 创建网络模型
mywork = MyNetwork()

mywork = mywork.to(device)    ## GPU —— 网络模型
# 也可直接写成   mywork.to(device)

# 定义损失函数
loss_fn = nn.CrossEntropyLoss()

loss_fn = loss_fn.to(device)    ## GPU —— 损失函数

# 定义优化器
# learning_rate = 0.01
learning_rate = 1e-2
optimizer = torch.optim.SGD(mywork.parameters(), lr=learning_rate)

# 设置参数
# 训练次数
total_train_step = 0
# 记录测试的次数
total_test_step = 0
# 训练的轮数
epoch = 10

for i in range(epoch):
    print("------第{}轮训练开始-------".format(i+1))

    # 训练步骤开始
    mywork.train()  # 
    
    for data in train_dataloader:      # 从dataloader里取数据
        imgs, targets = data
	    imgs = imgs.to(device)          ## GPU —— 数据
    	targets = targets.to(device)    ## GPU —— 数据
        
        outputs = mywork(imgs)
        loss = loss_fn(outputs, targets)

        # 优化器优化模型
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()

        total_train_step = total_train_step + 1
        if total_train_step % 100 == 0:      # 不每步都打印
            print("训练次数：{}， Loss：{}".format(total_train_step, loss.item()))  # 使用.item()
            writer.add_scalar("train_loss", loss.item(), total_train_step)


    # 测试步骤开始
    
    mywork.eval()
    
    total_test_loss = 0
    total_accuracy = 0
    with torch.no_grad():
        for data in test_dataloader:
            imgs, targets = data
	        imgs = imgs.to(device)          ## GPU —— 数据
    	    targets = targets.to(device)    ## GPU —— 数据
        
            outputs = mywork(imgs)
            loss = loss_fn(outputs, targets)
            total_test_loss = total_test_loss + loss.item()
            
            accuracy = (outputs.argmax(1) == targets).sum
            total_accuracy = total_accuracy + accuracy

    print("整体loss：{}".format(total_test_loss))
    print("整体测试集上的正确率：{}".format(total_accuracy / test_data_size))
    writer.add_scalar("test_loss", loss, total_test_step)
    total_test_step = total_test_step + 1

    # 每一轮结束后保存参数
    torch.save(mywork, "mywork_{}.pth".format(i+1))


writer.close()
```



# 完成的模型验证(测试)套路

`test.py` 

```py
''' 以 CIFAR10-quick model 为例，从网上下载dog图片，看模型是否能正确分类 '''
import torchvision
from PIL import Image

image_path = "../imgs/dog.png"       # 导入待分类的dog图片
image = Image.open(image_path)
image=image.convert("RGB")   # 图片格式转换
'''
要加image=image.convert('RGB')。因为png格式是四个通道，除了RGB三通道外，还有一个透明度通道。
所以调用 image = image.convert('RGB") 来保留其颜色通道。 但如果图片本来就是三个颜色通道，经过此操作就不变。 
加上这步后可以适应png、jpg各种格式的图片。
'''
print(image)                    

transform = torchvision.transforms.Compose([torchvision.transforms.Resize((32,32)),
                                            torchvision.transforms.ToTensor()])

image = transform(image)
print(image.shape)

class MyNetwork(nn.Module):
    ''' Structure of CIFAR10-quick model. '''
    def __init__(self):
        super().__init__()
        self.model1 = nn.Sequential(
            nn.Conv2d(3, 32, kernel_size=5, stride=1, padding=2),
            nn.MaxPool2d(2),
            nn.Conv2d(32, 32, kernel_size=5, stride=1, padding=2),
            nn.MaxPool2d(2),
            nn.Conv2d(32, 64, kernel_size=5, stride=1, padding=2),
            nn.MaxPool2d(2),
            nn.Flatten(),
            nn.Linear(64*4*4, 64),
            nn.Linear(64, 10)
        )

    def forward(self, x):
        x = self.model1(x)
        return x

'''使用torch.load时必须保证该模型的定义class存在或者使用import导入该模型的定义class'''    
model = torch.load("network_29.pth")
image = torch.reshape(image, (1, 3, 32, 32))

# 开启模型验证(测试)步骤
model.eval()
with torch.no_grad():
    ouput = model(image)
    
print(output)
''' tensor([[6.4093,0.4398,5.7485,-0.5811,-1.6378,-3.1114,-3.3110,-3.1231,2.7497，-3.1579]) '''
print(output.argmax(1))
''' tensor([0]) '''
```





