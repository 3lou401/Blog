 > 本文相关代码可以从[Backpropagation](https://github.com/chi2liu/Backpropagation)下载

在上一篇文章[小白也能看懂的BP反向传播算法之Let's practice Backpropagation](https://www.jianshu.com/p/1ba80e1ffe1e)，我们计算了一个带sigmoid函数的嵌套网络的反向传播！从这篇文章开始，我们正式进入实际的神经网络的反向传播！本文将以一个两层的神经网络结构为例子，并且利用矩阵的方法实现神经网络的反向传播训练算法！

# Lets get started!!!

神经网络的结构如下:
![image.png](http://upload-images.jianshu.io/upload_images/1234352-805c2ed90410dabd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


上图的神经网络包括两层网络。第一层是输入层，包括三个神经元，第二层也就是输出层，包括了两个神经元。标准的神经网络中，sigmoid层也就是激活函数，是输出层的一部分，这里为了反向传播时计算微分更直观，就将其分开！

对于不了解基本神经网络的同学可以参考,对于不了解激活函数的同学可以参考[‘神经网络’初探](https://www.jianshu.com/p/fa8d5ff9321a)

我们下面来分析这个神经网络。首先，三个输入值被输入到输入层的三个节点中，因此我们的输入，用矩阵表示，应该是三维的。然后输入层将和各自的权重相乘，得到输出层，这里和权重的相乘，可以简化成矩阵的乘法运算。然后再输入到sigmoid函数中，进行激活计算，得到一个0-1之间的输出值。最后输出到cost function中，进行误差的计算，这里的cost function可以选取不同的计算函数，这里我们用交叉熵函数作为代价函数， [cross-entropy](https://en.wikipedia.org/wiki/Cross_entropy) 。单纯对于研究反向传播来说，我们都可以不需要知道这些一层层的函数是干嘛的，因为我们反向传播要求的只是微分而已。只要这些函数是可微的，不管结构在复杂，无非是链式求导的时候多求几个微分而已！反向传播的本质就是在微分的计算！

# Aim

误差当然是越小越好，所以我们训练网络的目标是将cost function的值减小，这和我们之前几篇文章将输出结果增加正好是相反的，其实也很简单，只需要在更新的时候，减去步长和微分的乘积就行，将之前的＋变成－！具体可以参考梯度下降法
这里我们要更新的是权重的值，所以更新的方法如下：
![image.png](http://upload-images.jianshu.io/upload_images/1234352-ef7736f44014a161.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里的Wij代表，第i个输入节点到第j的输出节点的权重！
只需要求出costfunction关于每个权重的微分即可！

首先，我们自然要先进行正向传播，也就是正向计算最后的输出cost function！

# Forward Propagation
首先，我们将输入矩阵化，就是一个1*3的矩阵：
![image.png](http://upload-images.jianshu.io/upload_images/1234352-e1d1d506ede66626.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果我们有多个样本的输入值，比如有n个输入，那么输入矩阵就可以写成n*3的矩阵！

权重矩阵如下：
![image.png](http://upload-images.jianshu.io/upload_images/1234352-16158fe3ed82fe10.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后输入层到输出层的计算，就可以简化成，两个矩阵的相乘：
![image.png](http://upload-images.jianshu.io/upload_images/1234352-414d1a79091a5d5a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

正好得到一个 1x2 的矩阵，对应输出层的两个神经元，符合我们的预期，然后我们给第一个输出层的神经元标记y1，给第二个神经元标记为y2。
![image.png](http://upload-images.jianshu.io/upload_images/1234352-08c4fe1e9b0d6d55.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后再进行激活函数的计算
![image.png](http://upload-images.jianshu.io/upload_images/1234352-5694958da9ae7bbb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Cost function
得到输出层的输出并进行激活函数计算之后，就要输入到cost function中计算errors！。这里我们采用的交叉熵代价函数， [cross-entropy](https://en.wikipedia.org/wiki/Cross_entropy) 
![image.png](http://upload-images.jianshu.io/upload_images/1234352-03f5c709542f381a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里p是预期的值，q是我们经过神经网络计算得到的预测值，具体交叉熵函数的意义，可以参考 [cross-entropy](https://en.wikipedia.org/wiki/Cross_entropy) 
![image.png](http://upload-images.jianshu.io/upload_images/1234352-03f5c709542f381a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
而我们只要知道我们要将C的值降低，利用反向传播算法，降低C的输出，所以我们就要求得C的微分，首先我们把C展开：
![image.png](http://upload-images.jianshu.io/upload_images/1234352-c792a33bd27a0b96.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后将我们网络中计算得到的输出层的输出带入进去：
![image.png](http://upload-images.jianshu.io/upload_images/1234352-0b33bac34501394d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这样我们就分析完了怎么进行这个神经网络的正向传播！

# Backpropagation

反向传播之前，我们先回顾一下，每一层的输出结果

激活函数层的输出结果
![image.png](http://upload-images.jianshu.io/upload_images/1234352-2918269745cc3a15.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

输出层的输出结果
![image.png](http://upload-images.jianshu.io/upload_images/1234352-6d604a74d9bcdad5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

权重矩阵
![image.png](http://upload-images.jianshu.io/upload_images/1234352-3c180556d5158095.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

输入层
![image.png](http://upload-images.jianshu.io/upload_images/1234352-ef9755a816da0eb8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

明确了每层的值之后，我们要切记，我们反向传播所需的就是关于权重的微分，也就是
![image.png](http://upload-images.jianshu.io/upload_images/1234352-39cadccfed42f59a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

也就是我们要想办法求出C关于各个权重的微分！
求微分的基本思路和之前是一样的，不管网络的结构多复杂，根本都是利用链式法则，一层层的从输出求导到输入！这里，我们会采取矩阵的算法来进行微分的求解，这可以让我们的求解方法更适合于编写程序，并且更直观！

首先我们看输出C是关于sigmoid层的输出y0的函数，然后y0又是关于输出层的输出y的函数，y同时又是输入层x与权重相乘而得来的。所以，基本就明确了，我们需要先求取C关于y0的微分，再求取y0关于y的微分，然后求取y关于w的微分，最后又链式法则相乘在一起，就得到了C关于w的微分!

* 首先从cost function到sigmoid layer
![image.png](http://upload-images.jianshu.io/upload_images/1234352-1daa5e1e67a4ea27.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们可以很容易写出微分：
![image.png](http://upload-images.jianshu.io/upload_images/1234352-add9eb7969a105c9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

写成矩阵的形式
![image.png](http://upload-images.jianshu.io/upload_images/1234352-ff92189f032adf5b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 从sigmoid层到输出层的微分，就是求取sigmoid函数的微分
![image.png](http://upload-images.jianshu.io/upload_images/1234352-0a7681f0a7ec2a8c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

变成矩阵的形式就是：
![image.png](http://upload-images.jianshu.io/upload_images/1234352-02e8d9f499931cb4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 从输出层到输入层的微分就是关于权重的微分，我们先看y关于权重的形式
![image.png](http://upload-images.jianshu.io/upload_images/1234352-6d0baad0ae136528.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从这个形式不难得出关于权重的微分就是：
![image.png](http://upload-images.jianshu.io/upload_images/1234352-ec60b99b42037a0f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这样我们就可以运用链式法则，求取C关于权重W的微分了：
![image.png](http://upload-images.jianshu.io/upload_images/1234352-5945054e084d2841.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

将每个微分的值带入：
![image.png](http://upload-images.jianshu.io/upload_images/1234352-e3acde23cf4c8cd8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

将六个微分全部求取出来就是：
![image.png](http://upload-images.jianshu.io/upload_images/1234352-bbc00f568ca888a7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

不难写成矩阵的形式：
![image.png](http://upload-images.jianshu.io/upload_images/1234352-2abf84524833ff50.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里T代表矩阵的转置，X代表矩阵的乘法，圆圈加点代表矩阵对应元素相乘，也就是element-wise product。

最后，我们就可以得到完整的权重更新的法则：
![image.png](http://upload-images.jianshu.io/upload_images/1234352-20352d755d203ea4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

根据以上计算出的更新法则，编写python代码就很直观了

```python
import numpy as np


def sigmoid(x):
    return 1/(1+np.exp(-x))


def derivative_sigmoid(x):
    return np.multiply(sigmoid(x), (1-sigmoid(x)))


# initialization
# X : 1*3
X = np.matrix("2, 4, -2")
# W : 3*2
W = np.random.normal(size=(3, 2))
# label
ycap = [0]
# number of training of examples
num_examples = 1
# step size
h = 0.01
# forward-propogation
y = np.dot(X, W)
y_o = sigmoid(y)
# loss calculation
loss = -np.sum(np.log(y_o[range(num_examples), ycap]))
print(loss)     # outputs 3.6821105514(for you it would be different due to random initialization of weights.)
# backprop starts
temp1 = np.copy(y_o)
# implementation of derivative of cost function with respect to y_o
temp1[range(num_examples), ycap] = 1 / -(temp1[range(num_examples), ycap])
temp = np.zeros_like(y_o)
temp[range(num_examples), ycap] = 1
# derivative of cost with respect to y_o
dcost = np.multiply(temp, temp1)
# derivative of y_o with respect to y
dy_o = derivative_sigmoid(y)
# element-wise multiplication
dgrad = np.multiply(dcost, dy_o)
dw = np.dot(X.T, dgrad)
# weight-update
W -= h * dw
# forward prop again with updated weight to find new loss
y = np.dot(X, W)
yo = sigmoid(y)
loss = -np.sum(np.log(yo[range(num_examples), ycap]))
print(loss)     # 3.45476397276 outpus (again for you it would be different!)
```

运行程序，就会看到，进行反向传播，C的值也就是代价函数减少了！（由于初始权重是随机生成的，所以每次运行结果就不尽相同，但可以确定的，反向传播后的输出结果相对之前一定是减小的）


# 待续
这篇文章将会在此结束！我们已经成功将反向传播的计算扩展到真实的两层的神经网络中，并且将计算过程矩阵化！下一篇就是反向传播算法的终结篇，将会实现一个多层的神经网络的反向传播，并且运用动态规划算法对反向传播中微分的计算进行优化！

 > 本文相关代码可以从[Backpropagation](https://github.com/chi2liu/Backpropagation)下载
