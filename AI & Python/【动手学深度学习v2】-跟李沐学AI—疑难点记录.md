[08 线性回归 + 基础优化算法【动手学深度学习v2】-跟李沐学AI-【完结】动手学深度学习 PyTorch版-哔哩哔哩视频 (bilibili.com)](https://www.bilibili.com/list/1567748478?sid=358497&spm_id_from=333.999.0.0&desc=1&oid=714872301&bvid=BV1PX4y1g7KC)

课程网站：[课程安排 - 动手学深度学习课程 (d2l.ai)](https://courses.d2l.ai/zh-v2/)    【英文版内容比中文版多一些】

在线书阅读：[《动手学深度学习》 — 动手学深度学习 2.0.0 documentation (d2l.ai)](https://zh.d2l.ai/)

> 本笔记用于记录一些疑难点

<img src="https://gitee.com/HYHZRYX/images/raw/master/imgs/202404240036123.png" style="zoom: 50%;" />

## 轴 axis / dim

对于一个张量，它的shape有几维，就对应有几个轴，也就对应着张量的层级，最直观的可以通过看最前面的方括号数量来判断。

```python
import torch
a = torch.Tensor([[1,2,3], [4,5,6]])
b = torch.Tensor([[7,8,9], [10,11,12]])
c = torch.Tensor([[[1,2,3], [4,5,6]], [[7,8,9], [10,11,12]]])
print(a.shape)

# torch.Size([2, 3])
```

上面的张量 a 和 b，都对应两个轴。axis/dim=0 对应 shape [2, 3] 中的2，axis/dim=1 对应 shape [2, 3] 中的3，而张量 c 有三个轴。

张量运算时对轴参数的设定非常常见，在 Numpy 中一般是参数axis，在 Pytorch 中一般是参数dim，但它们含义是一样的。

在做张量的拼接操作时，**axis/dim设定了哪个轴，那对应的轴在拼接之后张量数会发生变化**

```python
>> torch.cat((a,b), dim=0)
tensor([[ 1.,  2.,  3.],
        [ 4.,  5.,  6.],
        [ 7.,  8.,  9.],
        [10., 11., 12.]])

>> torch.cat((a,b), dim=1)
tensor([[ 1.,  2.,  3.,  7.,  8.,  9.],
        [ 4.,  5.,  6., 10., 11., 12.]])
```

对于上面torch中的cat操作，当设定dim=0时，两个维度是(2,3)的张量合并成了一个(4,3)的张量，在第0维，张量数从2变成了4，第1维没有变化；当设定dim=1时，在第1维，张量数从3变成了6，第0维没有变化。

在做张量的运算操作时，**axis/dim设定了哪个轴，就会遍历这个轴去做运算，其他轴顺序不变**

```python
>> torch.softmax(a, dim=0)
tensor([[0.0474, 0.0474, 0.0474],
        [0.9526, 0.9526, 0.9526]])

>> torch.softmax(a, dim=1)
tensor([[0.0900, 0.2447, 0.6652],
        [0.0900, 0.2447, 0.6652]])
```

对于上面torch中的 softmax 操作，当设定 dim=0 时，就是其他轴不变，单次遍历 dim=0 轴的所有元素去做运算，上例中就相当于分别取了张量a中的第0列、第1列、第2列去做计算。

换一个角度，假设用for循环去遍历一个张量，那运算中设定的dim就是被放在最内层的for循环，其它的轴保持正常的顺序。

可以用下面的例子作为验证，这里tensor c 的shape 是 (m,n,p)，用for循环去计算 torch.softmax(c, dim=1)

```python
# for循环计算方式
c = torch.Tensor([[[1,2,3], [4,5,6]], [[7,8,9], [10,11,12]]])   # shape (2,2,3)
m,n,p = c.shape
res = torch.zeros((m,n,p))
for i in range(m):
    for j in range(p):
        res[i,:,j] = torch.softmax(torch.tensor([c[i,k,j] for k in range(n)]), dim=0)  #这里对应最内层的for循环

# 库函数设定轴计算方式
res1 = torch.softmax(c, dim=1)
print(res.equal(res1))      # True
```

axis/dim使用小总结：

1. 在做张量的拼接操作时，**axis/dim设定了哪个轴，那对应的轴在拼接之后张量数会发生变化**
2. 在做张量的运算操作时，**axis/dim设定了哪个轴，就会遍历这个轴去做运算，其他轴顺序不变**

实际上，第一条拼接操作也可以用第二条去理解，但拼接的轴张量数会发生变化更好理解和记忆。





注意“广播”机制





按特定轴求和：[05 线性代数【动手学深度学习v2】-跟李沐学AI-【完结】动手学深度学习 PyTorch版-哔哩哔哩视频 (bilibili.com)](https://www.bilibili.com/list/1567748478?sid=358497&spm_id_from=333.999.0.0&desc=1&oid=929681632&bvid=BV1eK4y1U7Qy&p=3)





<img src="https://gitee.com/HYHZRYX/images/raw/master/imgs/202404240153685.png" style="zoom: 67%;" />



<img src="https://gitee.com/HYHZRYX/images/raw/master/imgs/202404240158103.png" style="zoom: 50%;" />



![](https://gitee.com/HYHZRYX/images/raw/master/imgs/202404240203765.png)



<img src="https://gitee.com/HYHZRYX/images/raw/master/imgs/202404240159250.png" style="zoom: 50%;" />

> 分子布局即表示最终形状与分子形状相同

<img src="https://gitee.com/HYHZRYX/images/raw/master/imgs/202404240200037.png" style="zoom:50%;" />



![](https://gitee.com/HYHZRYX/images/raw/master/imgs/202404240202961.png)





![](https://gitee.com/HYHZRYX/images/raw/master/imgs/202404240202015.png)



## 回归问题与分类问题

![](https://gitee.com/HYHZRYX/images/raw/master/imgs/202404260042441.png)
