# pytorch

## 简单入门准备

### 神经网络的一般迭代过程

1. 定义一个拥有大量可学习的参数的神经网络
2. 在输入数据集上迭代
3. 使用神经网络处理输出
4. 计算loss
5. 后向传播误差
6. 跟新权重（参数）

### 基础classes

1. torch.Tensor 支持自动求梯度(autograd)操作的一个多维数组实现。该数组含有梯度w.r.t

2. nn.Module 自然神经网络模块。构建在autograd之上，用来定义运行神经网络。

3. nn.Parameter 一种Tensor, 当给一个Module赋予属性时自动注册成参数

4. autograd pytorch神经网络的核心是autograd自动求导包。该包针对Tensor的所有操作都提供了自动微分操作。该包是一个逐个运行的框架，这意味着反向传播由自己的代码定义来定义如何运行，每一个单一的迭代都可以不一样。

5. autograd.Function autograd前向或者后向实现的定义。每个Tensor操作操作会创建至少一个单一的Function 节点（该节点创建Tensor并encode Tensor历史，变量的.grad_fn属性记录该function节点）

6. autograd.Variable 是tensor的封装，支持几乎所有的操作。完成tensor操作后可以直接调用backward方法，然后所有的梯度计算会自动进行。通过.data属性可以访问原始张量，该变量的梯度会累积到.grad属性上。每个变量还有一个.grad_fn属性，它引用一个已经创建了Variable的Function。通过调用 .backward方法计算变量的导数，如果变量是标量，.backward无需指定任何参数，当变量有很多元素时，需要指定grad_output参数，该参数是一个匹配shape的张量。

7. torch.optim 包含跟新权重的一些规则，例如SGD, Nesterov-SGD, Adam, RMSPro, 

示例代码
```
#LeNet实现
import torch
from torch.autograd import Variable
import torch.nn as nn
import torch.nn.functional as F


class Net(nn.Module):

    def __init__(self):
        super(Net, self).__init__()
        # 卷积层 '1'表示输入图片为单通道, '6'表示输出通道数, '5'表示卷积核为5*5
        # 核心
        self.conv1 = nn.Conv2d(1, 6, 5)
        self.conv2 = nn.Conv2d(6, 16, 5)
        # 仿射层/全连接层: y = Wx + b
        self.fc1 = nn.Linear(16 * 5 * 5, 120)
        self.fc2 = nn.Linear(120, 84)
        self.fc3 = nn.Linear(84, 10)

    def forward(self, x):
        #在由多个输入平面组成的输入信号上应用2D最大池化.
        # (2, 2) 代表的是池化操作的步幅
        x = F.max_pool2d(F.relu(self.conv1(x)), (2, 2))
        # 如果大小是正方形, 则只能指定一个数字
        x = F.max_pool2d(F.relu(self.conv2(x)), 2)
        x = x.view(-1, self.num_flat_features(x))
        x = F.relu(self.fc1(x))
        x = F.relu(self.fc2(x))
        x = self.fc3(x)
        return x

    def num_flat_features(self, x):
        size = x.size()[1:]  # 除批量维度外的所有维度
        num_features = 1
        for s in size:
            num_features *= s
        return num_features


net = Net()
print(net)
input = torch.randn(1, 1, 32, 32)
out = net(input)
print(out)
params = list(net.parameters())
print(len(params))
print(params[0].size())
net.zero_grad()
out.backward(torch.randn(1, 10))
output = net(input)
target = torch.randn(10)  # a dummy target, for example
target = target.view(1, -1)  # make it the same shape as output
criterion = nn.MSELoss()

loss = criterion(output, target)
print(loss)
params = net.parameters()
import torch.optim as optim
optimizer = optim.SGD(net.parameters(),lr=0.01)
optimizer.zero_grad()
output = net(input)
loss = criterion(output,target)
loss.backward()
optimizer.step()
```
注：
- 网络学习的参数可以通过.parameters()获取，.named_parameters()可以同时返回学习的参数以及名称。
- 训练中，向前传输的是一个autograd.Variable
- 使用.zero_grad()方法将网络中所有梯度清零
- torch只支持mini-bathes（小批量）,不支持一次输入一个样本。若是想输入一个样本，必须使用Variable.unsqueeze(0)将batch_size设置为1
- 后向传播error使用loss.backward(), 使用前需要清零已存在的梯度(zero_grad)