# 学习链接

[【莫烦Python】Numpy & Pandas (数据处理教程)_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Ex411L7oT/?spm_id_from=333.337.search-card.all.click&vd_source=bf4c8577fb22820a80ba9f8a7e04169a)



# Numpy

一个在Python中做科学计算的基础库，重在数值计算，也是大部分PYTHON科学计算库的基础库，多用于在大型、多维数组上执行数值运算。



## 创建数组 (矩阵)

```py
import numpy as np
a = np.array([1, 2, 3, 4,5])  # [1 2 3 4 5]
print(type(a))   # <class 'numpy.ndarray'>
b = np.array(range(1, 6))  # [1 2 3 4 5]
c = np.arange(1, 6)        # [1 2 3 4 5]
# np.arange的用法: arange([start,] stop[, step,], dtype=None)
t1 = np.arange(4, 10, 2)  # [4 6 8]
```

> range()生成一个整数序列，返回的是一个“范围对象”（range object）。只能生成整数序列。
>
> np.arange()返回的是一个 NumPy 数组。可以生成整数、浮点数等类型的序列。

```py
# a.dtype 查看的是当前数组a里面存放的数据的类型
print(a.dtype)  # int64  (默认数据类型在64位电脑且是整数，就是int64)
```

## 指定数据类型   

（涉及数据量特别大时的内存占用问题）

| 类型                              | 类型代码     | 说明                                                       |
| --------------------------------- | ------------ | ---------------------------------------------------------- |
| int8、uint8                       | i1、u1       | 有符号和无符号的8位（1个字节）整型                         |
| int16、uint16                     | i2、u2       | 有符号和无符号的16位（2个字节）整型                        |
| int32、uint32                     | i4、u4       | 有符号和无符号的32位（4个字节）整型                        |
| int64、uint64                     | i8、u8       | 有符号和无符号的64位（8个字节）整型                        |
| float16                           | f2           | 半精度浮点数  （16位）                                     |
| float32                           | f4 或 f      | 标准的单精度浮点数(32位)。与C的`float`兼容                 |
| float64                           | f8 或 d      | 标准的双精度浮点数。与C的`double`和Python的`float`对象兼容 |
| float128                          | f16 或 g     | 扩展精度浮点数                                             |
| complex64、complex128、complex256 | c8、c16、c32 | 分别用两个32位、64位或128位浮点数表示的复数                |
| bool                              | ？           | 存储True和False值的布尔类型                                |

```py
t2 = np.array(range(1, 4), dtype=float)
print(t2.dtype)  # float64  (根据电脑的位数来设置)
t2 = np.array(range(1, 4), dtype="float32")
print(t2.dtype)  # float32
t2 = np.array(range(1, 4), dtype="i1")
print(t2.dtype)  # int8

''' 用astype来调整存放的数据类型 '''
t3 = np.array([1,1,0,0,1], dtype=bool)
print(t3)    # [True True False False True]
t4 = t3.astype("int8")
print(t4)    # [1 1 0 0 1]

''' 修改浮点型的小数位数'''
t5 = np.array([random.random() for i in range(4)])
print(t5)  # [0.207305 0.30895612 0.1388269 0.44063964]
t6 = np.round(t5, 2)  # 四舍五入到两位小数
print(t6)  # [0.21 0.31 0.14 0.44]
```

> ```py
> # Python中的保留两位小数  (两种写法均可)
> round(random.random(), 2)
> "%.2f"%random.random()
> ```

## 矩阵的形状

```py
a = np.array([1,2,3])
print(a.shape)  # (3, )
b = np.array([[1,2,3,8,7,8], [4,5,6,9,8,9]])
print(b.shape)  # (2, 6)
# reshape() 改变形状：该方法是有返回值的非原地操作 [参数是一个元组，但不带括号的写法也支持]
# b.reshape(3, 4)  与 b.reshape((3, 4)) 效果一样
c = b.reshape((3, 4))   
print(c) # [[1,2,3,8], [7,8,4,5], [6,9,8,9]]
print(b.shape)  # (2, 6)

# 三维矩阵 (2, 3, 4): 表示 2个 3行4列 的矩阵
[ [[1,2,3,4], 
   [5,6,7,8], 
   [9,10,11,12]],
  [[9,8,7,6], 
   [5,4,3,2], 
   [1,10,11,13]] ]

# 注意：转换成一维矩阵要传入的元组参数是 (6,)  而不是 (6, 1):  (6,1) 还是二维矩阵
# 转成一维数组 ↓
c = c.reshape(c.shape[0]*c.shape[1], )  
# flatten() 可以直接把数组转换成一维的
c = c.flatten()
print(c)  # [1,2,3,8,7,8,4,5,6,9,8,9]
```

## 矩阵的计算

```py
a = np.array([[1,2,3], [4,5,6], [7,8,9]])
# 直接做 加减乘除 某个数的运算 都会“广播”到所有元素上
print(a+2) # [[3,4,5], [6,7,8], [9,10,11]]

# 两个矩阵的运算: 如果两个的形状一样，则对应位置上的元素相运算
a = np.array([[1,2,3], [4,5,6], [7,8,9]])
b = np.array([[10,11,12],[13,15,19], [21,25,26]])
print(a + b) 
'''[[11,13,15], 
    [17,20,25],
    [28,33,35]]'''

# 不同维度的矩阵之间的运算：当在某一维度上一样时，维度一样部分的元素相运算；否则会报错
c = np.array([[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]])
a = np.array([3,4,5,6,7,8], [4,5,6,7,8,9])
a * c  # ValueError: operands could not be broadcast together with shapes(2,6) (3,4)

# 如果 a是2行6列(2,6), c是1行6列(6,) ↓ 可以在列的基础上计算
a = np.array([3,4,5,6,7,8], 
             [4,5,6,7,8,9])

c = np.array([1,2,3,4,5,6])
print(a - c)
'''[2,2,2,2,2,2],
   [3,3,3,3,3,3]'''
print(a * c)
'''[[3,8,15,24,35,48],
    [4,10,18,28,40,54]]'''

# 如果 a是2行6列(2,6), c是2行1列(2,1) ↓ 可以在行的基础上计算
a = np.array([3,4,5,6,7,8], [4,5,6,7,8,9])
c = np.array([[1], [2]])
print(c + a)
'''[[4,5,6,7,8,9], 
   [6,7,8,9,10,11]]'''
print(a * c)
'''[[3,4,5,6,7,8],
    [8,10,12,14,16,18]]'''
print(c * a)   # 结果与a*c相同
'''[[3,4,5,6,7,8],
    [8,10,12,14,16,18]]'''
```

## 广播原则（Broadcast）

如果两个数组的 trailing dimension（即从末尾开始算起的维度）的轴(axis) 长度相符 或 其中一方的长度为1，则认为它们是广播兼容的。广播会在 缺失 和（或）长度为1的 维度上进行。

例如：

- shape为(3, 3, 2) 和shape为(3，2) 就可以计算
- shape为（3，3，3）和shape为（3，2）就不能计算
- shpae为（3，3，2）和shape为（3，3）可以计算

   

## 轴(axis)：

轴（axis）在numpy中可以理解为“方向”，使用`0,1,2...`数字表示。

​	对于一个一维数组，只有一个0轴；对于2维数组shape(2,2)，有0轴和1轴；对于三维数组shape(2,2,3)，有0,1,2轴。

- 比如计算一个2维数组的平均值，必须指定是计算哪个方向(轴)上面的数字的平均值。
- `np.arange(0,10).reshape((2,5))`：reshape中的2表示`0轴`长度为2，`1轴`长度为5。

![](https://gitee.com/HYHZRYX/images/raw/master/imgs/202412112344060.png)

## Numpy读取数据

> 一般不用numpy来读，因为有更强大的工具（如pandas）

CSV 文件: （Comma-Separated Value, 逗号分隔值文件） 里面的值都是通过“逗号”隔开的。

- 显示：表格状态  （如Excel表格）
- 源文件：换行和逗号分隔行列的格式化文本,每一行的数据表示一条记录

由于csv便于展示,读取和写入,所以很多地方也是用csv的格式存储和传输中小型的数据。为了方便教学,我们会经常操作csv格式的文件,但是操作数据库中的数据也是很容易的实现的。

```py
# numpy从文本文件中读取内容 (不是只有.txt文件)
np.loadtxt(fname, dtype=np.float, delimiter=None, skiprows=0, usecols=None, unpack=False)
''' 参数: 解释
frame: 文件、字符串或产生器，可以是.gz或bz2压缩文件
dtype: 数据类型，可选，CSV的字符串以什么数据类型读入numpy数组中。(默认为np.float)
delimiter: 分隔字符串，默认是任何空格。(对于读取csv, 要改为逗号)
skiprows: 跳过前x行。
usecols: 读取指定的列，索引，元组类型。  
unpack: 相当于“转置”的效果. 如果为True,读入属性将分别写入不同数组变量; 为False, 读入数据将只写入一个数组变量。(默认为False)
'''
```

> "unpack"（解包）通常指的是将一个数据结构中的元素分解出来，并将它们分别赋值给一组变量。

```py
uk_file_path = "./youtube_video_data/GB_video_data_numbers.csv"
t1 = np.loadtxt(us_file_path, delimiter=",", dtype = "int")
'''
delimiter：指定边界符号是什么,不指定会导致每行数据为一个整体的字符串而报错
dtype：默认情况下对于较大的数据会将其变为科学计数展示的方式 (所以指定为int可以显示整数)
upack：默认是Flase(0),默认情况下,有多少条数据,就会有多少行。为True(1)的情况下,每一列的数据会组成一行,原始数据有多少列,加载出来的数据就会有多少行,相当于"转置" (行列交换) 的效果
'''
```



## 转置 (transpose)

转置是一种变换，对于numpy中的数组来说，就是在对角线方向交换数据。目的也是为了更方便的去处理数据。

转置和交换轴的效果一样。

```py
''' 三种实现二维的numpy数组转置的方法 '''
a = np.array([[0,1,2,3,4,5],
              [6,7,8,9,10,11],
              [12,13,14,15,16,17]])

a.transpose()   # 转置方法 （第一行变成第一列）
'''[[0,6,12],
    [1,7,13],
    [2,8,14],
    [3,9,15],
    [4,10,16],
    [5,11,17]] '''
a.T     # 转置 （这个是属性，transpose是方法）
'''[[0,6,12],
    [1,7,13],
    [2,8,14],
    [3,9,15],
    [4,10,16],
    [5,11,17]] '''
a.swapaxes(1,0)  # 交换轴 
'''[[0,6,12],
    [1,7,13],
    [2,8,14],
    [3,9,15],
    [4,10,16],
    [5,11,17]] '''
```



## 索引和切片

只想选择其中的某一列(行)：   **索引下标从0开始**

```py
a = np.array([[0,1,2,3,4,5],
              [6,7,8,9,10,11],
              [12,13,14,15,16,17]])
# 取一行
a[1]  # 取出第1行 (下标从0开始)
'''[6,7,8,9,10,11]'''  # 这时是个一维数组
a[-1]  # -1就是最后一列
''' [12,13,14,15,16,17] '''

# 取一列
a[:, 2]   # 把每一行的下标为2的数取出来
'''[2, 8, 14]'''

# 取连续的多行
a[1:3]    # 左闭右开
'''[[6,7,8,9,10,11],
    [12,13,14,15,16,17]]'''

# 取不连续的多行
a[[0, 2]]
'''[[0,1,2,3,4,5],
	[12,13,14,15,16,17]]'''

# 取连续的多列
a[:, 2:4]
'''[[2,3],
	[8,9],
	[14,15]]'''

a[1:, 2:4]   # 从下标为1的行开始的下标第2、3列

a[0:2, 2:4]   # 第0、1行与第2、3列的相交点
'''[[2,3],
    [8,9]]'''

# 选出 坐标为(0,1) (2,4)的结果
a[[0,2], [1, 4]]    # 使用方括号取值只能取到对应的点，使用“:”来取得相交点的值
'''[1 16]'''	   # 注意：这种情况是取出单个的值。

# 步长 （再加一个":"）
a[1::2]    # 步长为2
a[1:6:2]   # 步长为2
```



## 数值的修改 (布尔索引)

```py
# 取出某几行某几列，直接赋值即可
t = np.array([[0,1,2,3,4,5],
              [6,7,8,9,10,11],
              [12,13,14,15,16,17],
              [18,19,20,21,22,23]])
t[:, 2:4] = 0
'''[[0,1,0,0,4,5],
    [6,7,0,0,10,11],
    [12,13,0,0,16,17],
    [18,19,0,0,22,23]]'''

# 把 t 中小于10的数字全部替换为3
t<10
'''[[True,True,True,True,True,True],
    [True,True,True,True,False,False],
    [False,False,False,False,False,False],
    [False,False,False,False,False,False]] dtype=bool'''
t[t < 10] = 3    # 把为True的位置赋值为3 #
'''[[3,3,3,3,3,3],
    [3,3,3,3,10,11],
    [12,13,14,15,16,17],
    [18,19,20,21,22,23]]'''
```

### np.where

```py
t = np.array([[0,1,2,3,4,5],
              [6,7,8,9,10,11],
              [12,13,14,15,16,17],
              [18,19,20,21,22,23]])
# np.where() 是 numpy中的三元运算符
# np.where(t == 2)  # t==2会生成一个布尔数组, np.where 会返回这些 True 值的索引
np.where(t < 10, 0, 10)   # 把t中小于10的赋值为0，其余的赋值为10 #
'''[[0,0,0,0,0,0],
    [0,0,0,0,10,10],
    [10,10,10,10,10,10],
    [10,10,10,10,10,10]]'''
```

### clip (裁剪)

```py
t = np.array([[0.,1.,2.,3.,4.,5.],
              [6.,7.,8.,9.,10.,11.],
              [12.,13.,14,15,16,17],
              [18,19,20,nan,nan,nan]])
# 把小于10的替换为10, 大于18的替换成18
t.clip(10, 18)    # nan不会被替换 （NaN是float类型）#
'''[[10.,10.,10.,10.,10.,10.],
    [10.,10.,10.,10.,10.,11.],
    [12,13,14,15,16,17],
    [18,18,18,nan,nan,nan]]'''

# 将一个位置赋值为 NaN (Not a Number)
t2 = np.array([[0,1,2,3,4,5],
              [6,7,8,9,10,11],
              [12,13,14,15,16,17],
              [18,19,20,21,22,23]])
t2 = t2.astype(float) # 将int转成float
t2[3, 1] = np.nan
'''[[0,1,2,3,4,5],
    [6,7,8,9,10,11],
    [12,13,14,15,16,17],
    [18,nan,20,21,22,23]]'''
```



## numpy中的nan与inf

nan(NAN,Nan)：not a number  表示不是一个数字。

什么时候numpy中会出现nan：

- 当读取本地的文件为float的时候，如果**有缺失**，就**会出现nan**
- 当做了一个不合适的计算的时候（比如无穷大(inf)减去无穷大）
- 0 除以 0

inf(-inf,inf)：infinity,    inf 表示正无穷，-inf 表示负无穷

什么时候回出现inf包括（-inf，+inf)：

- 比如一个数字除以0, （python中直接会报错，numpy中是一个`inf`或者`-inf`）

```py
# 如何指定一个nan或者inf
a = np.inf
type(a)
''' float '''
a = np.nan
type(a)
''' float '''
```

注意他们的type类型都是 `float`

**nan的注意点：**

1. 两个 nan 是不相等的 。  （因为都不是数字，不可比）

  ```py
  np.nan == np.nan   
  ''' False'''
  np.nan != np.nan
  ''' True '''
  ```

2. 判断数组中nan的个数：

  ```py
  t = np.array([1., 2., nan])
  np.count_nonzero(t != t)  # 利用 nan != nan为True的特性
  ''' 1 '''
  ```

3. 通过`np.isnan()`判断一个数字是否为nan；会返回bool类型

  ```py
  t = np.array([1., 2., nan])
  # 把t中为nan的替换为0
  t[np.isnan(t)] = 0
  '''[1., 2., 0.]'''
  ```

4. nan 和 任何值计算都为 nan

	```py
	t2 = np.array([[0,1,2,3,4],
	               [nan,5,6,7,8]])
	np.sum(t2)
	''' nan '''
	np.sum(t2, axis=0)
	''' [nan, 6, 8, 10, 12] '''
	```

注意：在一组数据中单纯的把nan替换为0，不一定合适：

- 比如，全部替换为0后，替换之前的 行或列 的平均值如果大于0，替换之后的均值肯定会变小；小于0，则均值变大。
- 所以**更一般的方式是把缺失的数值替换为均值（中值, 中位数）或者是直接删除有缺失值的一行。**

> 很多时候是替换成均值，并不直接删除。为了尽可能利用数据。

## numpy中常用统计函数

```py
# 求和:
t.sum(axis=None)
# 均值：
t.mean(a,axis=None)  # 受离群点的影响较大
# 中值 (中位数): 
np.median(t,axis=None)
# 最大值：
t.max(axis=None)
# 最小值：
t.min(axis=None)
# 极值：(Peak to Peak)
np.ptp(t, axis=None)  # 即最大值和最小值之差
# 标准差：
t.std(axis=None)

''' 默认返回多维数组的全部的数的一个统计结果(最终返回的是一个值, 比如求和就是把数组中所有元素全部加起来)
     如果指定axis则返回一个当前轴上的结果 (比如二维数组t.sum(axis=1)就是把每一行的所有列求和)'''
```

> 标准差是一组数据平均值分散程度的一种度量。一个较大的标准差，代表大部分数值和其平均值之间差异较大；一个较小的标准差，代表这些数值较接近平均值。标准差反映出数据的波动稳定情况，越大表示波动越大，越不稳定。
>
> 标准差：  $$\Large \sqrt{\frac{1}{N} \sum_{i=1}^N(x_i - \mu)^2}$$

## 给ndarry缺失值填充均值

> 此为Numpy的方法；在pandas里更容易。

如果 t 中存在nan值，如何操作把其中的nan填充为每一列的均值：

```py
t = array([[ 0., 1., 2., 3., 4., 5.],
		  [ 6., 7., nan, 9., 10., 11.],
           [ 12., 13., 14., nan, 16., 17.],
           [ 18., 19., 20., 21., 22., 23.]])
# 把其中的nan填充为每一列的均值 #
def fill_nan_by_column_mean(t) :
    for i in range(t.shape[1]):  # 遍历每一列 (假设是二维数组)
        nan_num = np.count_nonzero(t[:, i][t[:, i] != t[:, i]]) # 计算当前列的非nan的行数
        if nan_num > 0: # 存在nan值
            now_col = t[:，i]   # 当前列
            now_col_not_nan = now_col[np.isnan(now_col) == False].sum() # 把当前列不为nan的求和
            now_col_mean = now_col_not_nan / (t.shape[0]-nan_num）# 求不为nan的均值
            now_col[np.isnan(now_col)] = now_col_mean  # 把均值赋值给当前列中为nan的
            t[:, i] = now_col  # 再把当前列赋值给t，即更新t的当前列
    return t
```

## 数组的拼接

```py
t1 = np.array([[0,1,2,3,4,5],
               [6,7,8,9,10,11]])
t2 = np.array([[12,13,14,15,16,17],
               [18,19,20,21,22,23]])
# vstack：vertically 竖直拼接
np.vstack((t1, t2))
'''[[0,1,2,3,4,5],
    [6,7,8,9,10,11],
    [12,13,14,15,16,17],
    [18,19,20,21,22,23]]'''
# hstack：horizontally 水平拼接
vp.hstack((t1, t2))  # t1放参数里的左边，拼接时t1就在左边
'''[[0,1,2,3,4,5, 12,13,14,15,16,17],
    [6,7,8,9,10,11, 18,19,20,21,22,23]]'''
```

注意：竖直 / 水平分割是怎样的，对应的竖直 / 水平分割就是其逆过程。

## 数组的行列交换

竖直拼接的时候：要保证每一列代表的意义相同！如果每一列的意义不同，这个时候应该交换某一组的数的列，让其和另外一类相同。

```py
t = np.array([[12,13,14,15],
              [21,22,23,24],
              [31,32,33,35]])
# 行交换
t[[1,2], :] = t[[2,1], :]
print(t)
'''[[12,13,14,15],
	[31,32,33,35],
    [21,22,23,24]]'''
# 列交换
t[:, [0,2]] = t[:, [2,0]]  # 第0列和第2列交换 #
'''[[14,13,12,15],
    [33,32,31,35],
    [23,22,21,24]]'''
```

## Numpy的更多方法

1. 获取最大值最小值的**位置**

  ```py
  np.argmax(t, axis=0)  # 对于二维数组来说，求出每一列的最大数所在的行
  np.argmin(t, axis=1)  # 对于二维数组来说，求出每一行的最大数所在的列
  ```

2. 创建一个全0的数组: 

  ```py
  np.zeros((3,4))  # 参数是一个元组。这个表示创建shape为(3, 4) 的全0数组
  zero_data = np.zeros((t.shape[0], 1)).astype(int)  # 默认是float类型
  ```

3. 创建一个全1的数组:

  ```py
  np.ones((3,4))
  ```

4. 创建一个对角线为1的正方形数组(方阵)：

  ```py
  np.eye(3)  # 创建一个shape为(3, 3)的对角线为1, 其余为0的正方形数组
  '''[[1., 0., 0.],
      [0., 1., 0.],
      [0., 0., 1.]]'''
  ```

## numpy生成随机数

```py
np.random.rand(2, 3)
np.random.randint(10, 20, (4,5)) # 随机生成整数从[10,20) 的shape为(4, 5)的数组
```

| 参数                        | 解释                                                         |
| --------------------------- | ------------------------------------------------------------ |
| `.rand(do,d1,..dn)`         | 创建d0-dn维度的**均匀分布**的随机数数组，浮点数，范围从0-1   |
| `.randn(d0,d1,..dn)`        | 创建d0-dn维度的**标准正态分布**随机数，浮点数，平均数0，标准差为1 |
| .randint(low,high, (shape)) | 从给定上下限范围选取随机数整数，范围是low,high，形状是shape  |
| .uniform( low,high,(size))  | 产生具有均匀分布的数组，low起始值，high结束值，size形状      |
| .normal(loc,scale, (size))  | 从指定正态分布中随机抽取样本，分布中心是loc（概率分布的均值），标准差是scale，形状是size |
| .seed(s)                    | 随机数种子，s 是给定的种子值。因为计算机生成的是伪随机数，所以通过**设定相同的随机数种子，可以每次生成相同的随机数** |

- 均匀分布 (Uniform Distribution)：在相同的大小范围内的出现概率是等可能的。
- 正态分布 (Normal Distribution)： 呈钟型，两头低，中间高，左右对称。

## numpy的copy与view(视图)

```py
# 1.将b赋值给a
a = b  # 完全不复制，a和b相互影响 （浅拷贝）

# 2.视图操作
a = b[:] # 一种切片，会创建新的对象a，但是a的数据完全由b保管，他们两个的数据变化是一致的。

# 3.复制
a = b.copy() # 复制，a和b互不影响
```

# pandas

**numpy能够处理数值型数据**，但是这还不够。很多时候，数据除了数值之外，还有字符串，还有时间序列等。
比如：我们通过爬虫获取到了存储在数据库中的数据
比如：之前youtube的例子中除了数值之外还有国家的信息，视频的分类(tag)信息，标题信息等
所以，numpy能够帮助我们处理数值，但是**pandas除了处理数值之外(基于numpy)，还能够帮助处理其他类型的数据**。

> pandas is an open source, BSD-licensed（Berkeley Software Distribution license） library providing high-performance, easy-to-use data structures and data analysis tools for the Python programming language.

NumPy是Python中用于科学计算的核心库，提供了一个强大的N维数组对象`ndarray`以及用于操作这些数组的工具。NumPy的数组是同质的，意味着数组中的所有元素必须是相同的数据类型。

Pandas是建立在NumPy之上的一个数据分析工具库，提供了DataFrame和Series两种数据结构，它们是异质的，可以存储不同类型的数据（比如，数字、字符串等）。Pandas非常适合处理表格数据，提供了大量用于数据清洗、处理和分析的高级功能。

- 主要处理的是数值数据和科学计算，NumPy可能是更好的选择
- 需要处理复杂的表格数据，进行数据清洗和分析，Pandas将更加合适



## 常用数据类型

- Series 一维，带标签(index)的一维数组。
- DataFrame 二维，Series容器

### Series

```py
import pandas as pd
# 直接创建Series
t = pd.Series([1,2,3,4])
''' 
↓index
0    1
1    2
2    3
3    4
dtype: int64 '''
type(t)
''' <class 'pandas.core.series.Series'> '''
# 指定index （注意index长度要和数组里的元素数量一致）
t1 = pd.Series([1,3,5,7], index=list("abcd"))
'''
a    1
b    3
c    5
d    7
dtype: int64
'''
t2 = pd.Series(np.arange(5), index=list(string.ascii_uppercase[:5]))
'''
A    0
B    1
C    2
D    3
E    4
'''
# 通过字典(dict)来创建Series
temp_dict = {"name": "xiaoming", "age": 30, "tel":10086}
t3 = pd.Series(temp_dict)  # 字典的key就是index，value就是value
'''
age		30
name	xiaoming
tel		10086
dtype: object
'''
a = {string.ascii_uppercase[i]: i for i in range(3)}
a1 = pd.Series(a)
'''
A	0
B	1
C	2
dtype: int64
'''
# 重新给其指定其他的索引之后: 如果能够对应上，就取其值; 如果不能，就为Nan #
a2 = pd.Series(a1, index=list(string.ascii_uppercase[1:4]))
'''
B	1.0
C	2.0
D	NaN   
dtype: float64 '''   # numpy中nan为float，pandas会自动根据数据类更改series的dtype类型

# 转换dtype
a1.astype(float)
```

### Series切片和索引

- 切片：直接传入start、end或者步长即可
- 索引：一个的时候直接传入序号或者index，多个的时候传入序号或者index的列表

```py
t = pd.Series([0,1,2,3,4,5,6,7,8,9], index=list("ABCDEFGHIJ"))
'''
A	0
B	1
C	2
...
H	7
I	8
J	9
dtype: int64
'''

# 切片：start(包含) : end(不包含) : step(步长)
t[2:10:2]   # 从第2个数开始（从第0个数开始计数）
'''
C	2
E	4
G	6
I	8
'''
# 直接根据在Series中的位置序号来取value 
t[1]  # 排在第1个位置的值 （从0开始计数）
'''
1
'''
# 传一个列表当参数，取出不连续的位置上的多个value
t[[2,3,6]]
'''
C	2
D	3
G	6
'''
# 布尔索引
t[t>4]
'''
F	5
G	6
H	7
I	8
J	9
'''
# 获取index对应的value
t["F"]
'''
5
'''
# 传一个列表当参数，获取多个index对应的各自的value
t[["A", "F", "g"]]
'''
A	0.0
F	5.0
g	NaN   # 如果查询的index无对应的value，就是NaN
dtype: float64
'''
```

对于一个陌生的Series类型，获取它的索引和具体的值：

```py
t = pd.Series([0,1,2,3,4,5,6,7,8,9], index=list("ABCDEFGHIJ"))
'''
A	0
...
J	9
dtype: int64
'''
# 获取Series类型的index
t.index
'''
Index(['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J'], dtype='object')
'''
type(t.index)
''' <class 'pandas.core.indexes.base.Index'> '''
for i in t.index:
    print(i)
len(t.index)  # 10
list(t.index) # ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J']
    
# 获取Series类型的values
t.values
'''[0 1 2 3 4 5 6 7 8 9]'''
type(t.values)
''' <class 'numpy.ndarray'> '''
```

Series对象本质上由两个数组构成：键 → 值

- 一个数组构成对象的 键(index，索引)
- 一个数组构成对象的 值(values)

ndarray的很多方法都可以运用于series类型，比如argmax，clip

series具有where方法，但是结果和ndarray不同  (建议去看pandas官网文档)



### DataFrame

DataFrame对象既有行索引，又有列索引：  是个二维的Series容器。

- 行索引，表明不同行，横向索引，叫index，0轴，axis=0
- 列索引，表名不同列，纵向索引，叫columns，1轴，axis=1

```py
# 创建DataFrame
t = pd.DataFrame(np.arange(12).reshape((3,4)))
'''
	0	1	2	3    # → columns
0	0	1	2	3
1	4	5	6	7
2	8	9	10	11
↑ index
'''
# 手动指定index和columns
t1 = pd.DataFrame(np.arange(12).reshape(3,4), index=list("abc"), columns=list("WXYZ"))
'''
	W	X	Y	Z
a	0	1	2	3
b	4	5	6	7
c	8	9	10	11	
'''

# 通过一个字典(dict)来创建DataFrame
d1 = {"name":["ming","gang"], "age":[20,32], "tel":[10086,10010]}
t1 = pd.DataFrame(d1)
'''  
	age		name	 tel
0    20		ming	10086
1    32		gang	10010
'''   # 每一行是一个数据。字典里的key对应的columns

# 一个列表，列表里是字典来创建DataFrame
d2 = [{"name":"hong","age":32, "tel":10010}, {"name":"gang","tel":10000}, {"name":"wang","age":22} ]
t2 = pd.DataFrame(d2)
'''
	age		name	 tel
0    32.0	hong	10010.0
1    NaN	gang	10000.0
2	22.0	wang	 NaN
'''   # 字典的key还是columns，但如果有缺失的值会是NaN
```

通过字典创建DataFrame的两种方式：

- 一个字典，每个key里的可以对应多个value（值使用列表来装）【这种方式要求不能有缺失值，即每个key里值的个数是相等的】
- 使用一个列表，列表里的元素是字典   【该方式可以有NaN】



DataFrame的基本操作：

```py
df
'''
   age	   name	    tel
0  32.0   xiaohong  10010.0
1   NaN   xiaogang  10000.0
2   22.0  xiaowang   NaN
'''
# 行索引
df.index 
''' RangeIndex(start= 0,stop=3,step=1) '''
# 列索引
df.columns 
''' Index(['age'，'name'，'tel']，dtype='object') '''
# 对象值，二维的ndarray数组
df.values 
''' array([[32.0,'xiaohong',10010.0],
		  [nan,'xiaogang',10oo0.0],
		  [22.0, 'xiaowang', nan]], dtype=object) '''
# (行数, 列数)
df.shape 
''' (3, 3)'''
	
# 列数据的类型	
df.dtypes
''' age		float64
	name	object
	tel		float64
	dtype:	object'''
# 数据的维度
df.ndim
''' 2 '''

# 显示头部几行，默认5行
df.head(3)
# 显示末尾几行，默认5行
df.tail(3)
# 相关信息概览：行数, 列数, 列索引, 列非空值个数, 列类型, 列类型, 内存占用
df.info()
# 快速综合统计结果：计数count, 均值mean, 标准差std, 最大值max, 四分位数,最小值min  【只有数字类型的字段会显示】
df.describe()   # 常用
```



### 按某列排序

```py
# by表示按什么来排序
df.sort_values(by="Count_AnimalName",ascending=False)  # asc是升序（从小到大）
```



### 切片索引、loc

```py
# 排序后取前一百行
df_sorted = df.sort_values(by="Count_AnimalName")
df_sorted[:100]  # 取前一百行的数据

# 同时选择行和列
df[:100]["Count_AnimalName"]

# 具体要选择某一列
df["Count_AnimalName"]  # 取的是列的数据 【得到的是Series类型】
```

- `df.loc` 通过**标签**（索引的名字，字符串之类）索引行数据
- `df.iloc` 通过**位置**（序号，0、1、2 ...）获取行数据

注意：赋值的时候，如果是赋值np.nan，DataFrame里会自动进行类型转换，原int不会报错。

​	冒号(:)在loc里面是闭区间，但在iloc和其他地方是开区间。

```py
t3=pd.DataFrame(np.arange(12).reshape(3,4),index=list("abc"),columns=list("WXYZ"))
''' W	X	Y	Z
a   0	1	2	3
b	4	5	6	7
c	8	9	10	11
'''

# 取出 a 行 Z 列
t3.loc["a", "Z"]  # 这里取值得到的type是numpy.int64
''' 3 '''

# 取出 a 行
t3.loc["a"]
# 或 t3.loc["a", :]
'''
W	0
X	1
Y	2
Z	3
'''  # Series类型，WXYZ是索引

# 取出 Y 列
t3.loc[:, "Y"]
'''
a	2
b	6
c	10
'''  # Series类型

# 取出 间隔的多行
t3.loc[["a", "c"], :]   # 行位置传的是个list
''' W	X	Y	Z
a	0	1	2	3
c	8	9	10	11
'''  # DataFrame类型

# 同时取间隔的多行多列
t3.loc[["a", "c"], ["X", "Z"]]
''' X	Z
a	1	3
c	9	11
'''  # DataFrame类型

# 注意：冒号(:)在loc里面是闭合的, 即会选择到冒号后面的数据 【在这里是左闭右闭。在别处冒号右边是开区间】
t3.loc["a":"c", ["W", "Z"]]  # 取从a行到c行（没用list包装）
''' W	Z
a	0	3
b	4	7
c	8	11    # 这里c能取中
'''

# 给某处赋值
t3.loc["b":, :"Y"] = 30  # 注意，这里冒号(:)在loc里面是闭区间的  【但iloc不是】
print(t3)
''' W	X	Y	Z
a   0	1	2	3
b	30	30	30	7
c	30	30	30	11
'''
```

iloc：

```py
t3=pd.DataFrame(np.arange(12).reshape(3,4),index=list("abc"),columns=list("WXYZ"))
''' W	X	Y	Z
a   0	1	2	3
b	4	5	6	7
c	8	9	10	11
'''
# 取出 第一行  (编号从0开始)
t3.iloc[1]
'''
W	4
X	5
Y	6
Z	7
'''  # 超过编号索引，取值会报错
# 取出 第三列  (即Y列)
t3.iloc[:, 2]
'''
a	2
b	6
c	10
'''
# 取出不连续的多列
t3.iloc[:, [2, 1]]
''' Y	X
a	2	1
b	6	5
c	10	9
'''
# 
t3.iloc[[0,2], [2,1]]
''' Y	X
a	2	1
c	10	9
'''
# 取 连续的多行多列
t3.iloc[1:, :2]   # 这里冒号(:)在是开区间
''' W	X
b	4	5
c	8	9
'''

# 给某处赋值
t3.iloc[1:, :2] = 30
print(t3)
''' W	X	Y	Z
a   0	1	2	3
b	30	30	6	7
c	30	30	10	11
'''
```

### 布尔索引

```py
# 找到所有的使用次数超过800的狗的名字
df[df["Count_AnimalName"] > 800]
'''  Row_Labels		Count_AnimalName
1156   	BELLA		1195
...
'''

# 找到所有的使用次数超过700并且名字的字符串的长度大于4的狗的名字
df[(df["Row_Labels"].str.len() > 4) & (df["Count_AnimalName"] > 700)]
# 这里 '&', '|' 是位运算上的与操作、或操作
# .str是将对应的内容转成str类型
```



### pandas字符串方法

适用于转成str类型后

| 方法                  | 说明                                                         |
| --------------------- | ------------------------------------------------------------ |
| cat                   | 实现元素级的字符串连接操作，可指定分隔符                     |
| **contains**          | 返回表示各字符串是否含有指定模式的布尔型数组                 |
| count                 | 模式的出现次数                                               |
| endswith、startswith  | 相当于对各个元素执行x.endswith(pattern)或x.startswith（pattern) |
| findall               | 计算各字符串的模式列表                                       |
| get                   | 获取各元素的第i个字符                                        |
| join                  | 根据指定的分隔符将Series中各元素的字符串连接起来             |
| **len**               | 计算各字符串的长度                                           |
| **lower、upper**      | 转换大小写。相当于对各个元素执行x.lower(或x.upper()          |
| match                 | 根据指定的正则表达式对各个元素执行re.match                   |
| pad                   | 在字符串的左边、右边或左右两边添加空白符                     |
| center                | 相当于pad(side="both')                                       |
| repeat                | 重复值。例如，s.str.repeat(3)相当于对各个字符串执行x*3       |
| **replace**           | 用指定字符串替换找到的模式                                   |
| slice                 | 对Series中的各个字符串进行子串截取                           |
| **split**             | 根据分隔符或正则表达式对字符串进行拆分  【得到的会是Series，要再转成list】 |
| strip、rstrip、Istrip | 去除空白符，包括换行符。相当于对各个元素执行x.strip()、x.rstrip()、x.lstrip() |

```py
df["info"].str.split("/").tolist()
```



## 读取外部数据

```py
import pandas as pd
# 如果这组数据存在csv中，直接使用`pd.read_csv()`即可:
df = pd.read_csv("./dogName.csv")

# 读取其他数据 （方法有很多）
pd.read_clipboard()
pd.read_excel()
pd.read_pickle()
pd.read_sql(sql_sentence,connection)

# 也可以先用SQL把数据读出来，再放到pandas里处理
```



## 缺失数据处理

数据缺失通常有两种情况：
一种就是空，None等，在pandas是NaN(和np.nan一样)
另一种是让其为0

```py
# 判断数据是否为NaN：
pd.isnull(df)    # 为NaN的地方为True
''' W	      Y	
a	False	False
b	True	True
'''
pd.notnull(df)  # 不为NaN的地方为True
```

处理方式0：利用布尔索引取出不为NaN的行列

```py
t3[pd.notnull(t3["W"])]  # 取出t3的W列不为NaN的数据
'''	W		Y
a	0.0		1.0
'''
```

处理方式1：删除NaN所在的行列dropna (axis=0, how='any', inplace=False)

```py
t3.dropna(axis=0)  # 对于DataFrame，axis=0表示将 有NaN存在的行 删去
t3.dropna(axis=0, how='all')  # 一整行都为NaN 才会删去  （默认how='any', 即这一行有NaN就删去）
# 以上操作都不会对 t3 本身数据有影响

t3.dropna(axis=0, how='any', inplace=True) # inplace=True表示原地替换, 即直接修改t3本身的数据
```

处理方式2：填充数据，t.fillna(t.mean()),t.fiallna(t.median()),t.fillna(0)

```py
t2.fillna(0)   # 表示将所有NaN替换为0.0

t2.fillna(t2.mean()) # 将所有NaN替换为对应列的平均值mean
t2["age"] = t2["age"].fillna(t2["age"].mean()) # 对某一列的NaN替换为对应列的均值 

# 注意：pandas计算平均值等情况，NaN是不参与计算的（如果是0就会参与运算）。而在Numpy里NaN会参与运算并且结果为NaN
```

处理为0的数据：t[t==0]=np.nan   【如果不想让数据缺失时填充的 0 影响均值计算】

```py
t[t==0]=np.nan    # 将为0的值替换为NaN  

# 注意：不是每次为0的数据都需要处理
```

计算平均值等情况，nan是不参与计算的，但是0会



## pandas常用统计方法

```py
#评分的平均分
rating_mean = df["Rating"].mean()
#导演的人数
print(len(df["Director"].unique()))
# 演员的人数
temp_list = df["Actors"].str.split(",").tolist()
nums = set([i for j in temp_list for i in j])  # set来去重
#电影时长的最大最小值
max_runtime = df["Runtime (Minutes)"].max()
max_runtime_index = df["Runtime (Minutes)"].argmax()  # 最大值的位置
min_runtime = df["Runtime (Minutes)"].min()
min_runtime_index = df["Runtime (Minutes)"].argmin()
runtime_median = df["Runtime (Minutes)"].median()  # 中位数
```



## 数据合并(join、merge)

join:默认情况下是把行索引相同的数据合并到一起

```py
d1
'''	W		X		Y
A	1.0		2.0		3.0
B	20.0	3.5		4.1
'''
d2
'''	V		U		Z
A	0.0		0.0		1.0
B	2.0		3.0		1.0
C	2.5		6.0		4.0
'''

d1.join(d2)   # 这里d1在前就以d1的行为准
'''	W		X		Y		V		U		Z
A	1.0		2.0		3.0		0.0		0.0		1.0
B	20.0	3.5		4.1  	2.0		3.0		1.0
'''
d2.join(d1)  # 以d2为准时，d1没有的数据就为NaN
'''	V		U		Z		W		X		Y
A	0.0		0.0		1.0		1.0		2.0		3.0
B	2.0		3.0		1.0		20.0	3.5		4.1
C	2.5		6.0		4.0		NaN		NaN		NaN
'''
```

merge:按照指定的列把数据按照一定的方式合并到一起   【类似数据库的左右连接】

```py
d1
''' a	b	c	d
A	1.0 1.0 1.0 1.0
B	1.0 1.0 1.0 1.0
'''
d2
''' f	a	x
0	0	1	2
1	3	4	5
2	6	7	8
'''
# 默认是内连接，how="inner", 取交集
d1.merge(d2, on="a")  # d1的a列的两行都和d2的第0行匹配上了
'''	a	b	c	d	f	x
0	1.0	1.0	1.0	1.0	0	2
1	1.0	1.0	1.0	1.0	0	2
'''

d1.loc["A", "a"] = 100
d1
''' a	b	c	d
A 100.0 1.0 1.0 1.0
B	1.0 1.0 1.0 1.0
'''
d1.merge(d2, on="a")  # 只有d1的a列的第1行和d2的第0行匹配上
'''	a	b	c	d	f	x
0	1.0	1.0	1.0	1.0	0	2
'''

# 外连接, how="outer", 取并集
d1.merge(d2, on="a", how="outer")  # 如果一个有，另一个没有就用NaN补全
''' a	b	c	d	f	x
0 100.0	1.0	1.0	1.0	NaN	NaN
1	1.0	1.0	1.0	1.0	0.0	2.0
2	4.0	NaN	NaN	NaN	3.0	5.0
3	7.0	NaN	NaN	NaN	6.0	8.0
'''

# 左连接, how="left"
d1.merge(d2, on="a", how="left")  # 以左边的d1为准, 右边缺失的用NaN替换
''' a	b	c	d	f	x
0 100.0	1.0	1.0	1.0	NaN	NaN
1	1.0	1.0	1.0	1.0	0.0	2.0
'''
# 右连接, how="right"
d1.merge(d2, on="a", how="right")
''' a	b	c	d	f	x
0   1.0	1.0	1.0	1.0	 0	 2
1	4.0	NaN	NaN	NaN	 3	 5
2	7.0	NaN	NaN	NaN	 6 	 8
'''

# 如果没有公共的列明，就使用 left_on 与 right_on
t1
''' O	P
A	1.0	1.0
B	1.0	2.0
'''
t2
''' W	Q
A	0.0	2.0
B	3.0	2.0	
'''
t1.merge(t2, left_on="P", right_on="Q")
'''O    P    W    Q
0  1.0  2.0  0.0  2.0
1  1.0  2.0  3.0  2.0
'''
```



## 分组聚合

```py
# 按照columns_name列分组
grouped = df.groupby(by="columns_name")
# 得到的grouped是一个DataFrameGroupBy的对象，是可迭代的
# grouped中的每一个元素是一个元组
# 元组里面是 (索引【分组的值】, 分组之后的DataFrame)

for i, j in grouped:
    # ...
grouped.count()  # 会对所有的列都统计
grouped["Brand"].count()

# 统计中国每个省店铺的数量
china_data = df[df["Country"] == "CN"]
grouped = china_data.groupby(by="State/Province")count()["Brand"]
```

DataFrameGroupBy对象有很多经过优化的方法：

- count	分组中非NA值的数量
- sum         非NA值的和
- mean       非NA值的平均值
- median     非NaN值的算术中位数
- std、var    无偏（分母为n-1）标准差和方差
- min、max   非NA值的最小值和最大值

```py
# 如果需要对国家和省份进行分组统计:
grouped = df.groupby(by=[df["Country"],df["State/Province"]])  # by里传一个list参数

grouped = df["Brand"].groupby(by=[df["Country"],df["State/Province"]]).count()  #得到的前两列是索引，结果是Series类型
# 如果用两层方括号来取，就是DataFrame类型 （虽然结果中只有一列数据）
grouped_df = df[["Brand"]].groupby(by=[df["Country"],df["State/Province"]]).count()

# 获取分组之后的某一部分数据: 
df.groupby(by=["Country","State/Province"])["Country"].count()  # 这个地方df省略是因为外面的df可以识别到里面的列
# 对某几列数据进行分组:
df["Country"].groupby(by=[df["Country"],df["State/Province"]]).count()
```



## 索引和复合索引

