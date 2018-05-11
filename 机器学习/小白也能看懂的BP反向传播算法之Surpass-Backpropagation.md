 > 本文相关代码可以从[Backpropagation](https://github.com/chi2liu/Backpropagation)下载

上篇文章[小白也能看懂的BP反向传播算法之Further into Backpropagation](https://www.jianshu.com/p/96a5ba02bacc)中，我们小试牛刀，将反向传播算法运用到了一个两层的神经网络结构中！然后往往实际中的神经网络拥有3层甚至更多层的结构，我们接下来就已一个三层的神经网络结构为例，分析如何运用**动态规划**来优化反向传播时微分的计算！


# Lets get started!!!
如下的网络结构：
![image.png](http://upload-images.jianshu.io/upload_images/1234352-6ecc46969fc8492e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在正式分析神经网络之前，我们先修改一下权重矩阵的表示形式！

让我们以一个符号开始，它代表网络中任意方式的权重信息。我们将使用
![image.png](http://upload-images.jianshu.io/upload_images/1234352-4a7666f84e7dd2cf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

来表示从网络第(l−1)层中第k个神经元指向l层中第j个神经元的连接权重。因此举个例子，下图中的权重就表示从第二层中第四个神经元指向第三层中第二个神经元的权重：
![image.png](http://upload-images.jianshu.io/upload_images/1234352-b003e3eff33154aa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这个符号起始比较麻烦，的确需要一些努力才能掌握。但是通过努力你会发现它将变得简单和自然。符号中的一个不容易接受的地方就是j和k的位置关系。你可能认为用j来表示输入神经元，k表示输出神经元，而不是实际定义中反过来的方式。我将在下面解释这样做法的原因。

我将使用相似的符号来表示网络中的偏差和激活。明确地，我们使用blj来表示第l层中第j神经元的偏差，用alj来表示第l层中第j神经元的激活。下面的图将展示这些符号：
![image.png](http://upload-images.jianshu.io/upload_images/1234352-15ce5d78c3a10c57.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

有了这些符号，第l层中第j神经元的激活alj就与第(l−1)层中所有激活相关。
![image.png](http://upload-images.jianshu.io/upload_images/1234352-fb5beff863cf469a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

能够用漂亮而简洁的向量格式进行重写
![image.png](http://upload-images.jianshu.io/upload_images/1234352-7b8d803c1321a22d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这个表达式能给我们更多的启发，某一层的激活与上一层的激活是有什么关系：我们只是将权重矩阵应用到激活上，然后再加上一个偏差向量，最后应用σ函数!**顺便说一下，这个表达式诱发了前面提到的wljk符号。如果我们使用j来指示输入神经元，k来指示输出神经元，那么我们就需要替换表达式中的权重矩阵 用权重矩阵的转置。虽然这是一个很小的变化，但是烦人的是，我们将失去解释和思考的简单性“应用权重矩阵到激活上。**这个全局视图非常简单和简洁（使用了很少的下标），相对于一个神经元到一个神经元的方式。也可以将其想象成一种避免下标混乱，而且还能保持精确的方法。这个表达式在实际中非常有用，因为许多矩阵库都能提供快速的矩阵乘法，向量加法和向量化。

我们间接的计算
![image.png](http://upload-images.jianshu.io/upload_images/1234352-23188247cc6c2d2b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这个值非常有用，我们将zl命名为：网络l层的加权输入。我们将在本章大量的使用加权输入zl。

# Hadamard乘积s⊙t
后向传播算法是基于通用的线性代数运算——就像向量加法，矩阵乘向量等等。但是有一个操作平常很少用到。特别的，假设s和t是相同维数的两个向量，那么我们使用s⊙t来表示两个向量元素级的乘法。
![image.png](http://upload-images.jianshu.io/upload_images/1234352-4c32d4ba9efdfc85.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这种元素级的乘法有时叫做 Hadamard乘积或者Schur乘积。我们将把它叫做Hadamard乘积。好的矩阵库一般都能提供Hadamard乘积的快速实施，因此在实施后向传播时候就非常方便。

# 

根据前面多篇文章所学，我们如果要写出第l层j个神经元的加权输入的微分应该不难，就是链式法则求导，如下：
![image.png](http://upload-images.jianshu.io/upload_images/1234352-eb16ebf1d52d63cd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里我们假设l层是最后一层，那么此时求出了关于加权输入的微分，就可以继续微分求取关于权重的微分
根据
![image.png](http://upload-images.jianshu.io/upload_images/1234352-8f1c85d6dbaf92e4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

所以，不难求出关于权重的微分
![image.png](http://upload-images.jianshu.io/upload_images/1234352-6b1ccd4fdd963ec2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

好，现在问题来了，如果我们再往前，求倒数第二层的某个权重，思路也是一致的，也就是要从最后一层一直往回算，为了避免公式太长，我们先求关于第l-1层第j个神经元的加权输入的微分
![image.png](http://upload-images.jianshu.io/upload_images/1234352-5502f1b8aa0821ca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后再根据
![image.png](http://upload-images.jianshu.io/upload_images/1234352-8f1c85d6dbaf92e4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

求取权重的微分！

可以看到，只是倒数第二层而已，我们的求取的公式就已经很长了，如果再有个几层，估计就已经爆炸了！

这个时候，就轮到动态规划出场了，动态规划就是在递归的过程中，保存已有的结果，下次计算的时候就不用再算了，可以直接从内存中红取结果。那么我们如何用在这里呢？

仔细观察第l层和l-1层的权重计算公式，我们不难发现，计算第l-1层要经过l层，而在第l-1层中
![image.png](http://upload-images.jianshu.io/upload_images/1234352-95b0c09ab053268f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们发现前面两个微分
![image.png](http://upload-images.jianshu.io/upload_images/1234352-9ac9202e2ff208e5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

不就是第l层的关于加权输入的微分么？也就是说，我们在第l层的权重的微分的计算的时候，就已经计算过这个了，然后在第l-1层的计算的时候还要用到这个。所以我们可以考虑，在第l层计算权重的微分的时候，就把这个值保存下来，这样在后续计算的时候就可以直接用了，这就是动态规划的思想！

我们保存每一层的
![image.png](http://upload-images.jianshu.io/upload_images/1234352-1a7d70f5f4b54d93.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们给这个值取了个名字叫敏感度矩阵，或者误差矩阵！

![image.png](http://upload-images.jianshu.io/upload_images/1234352-5355dd0512d09ead.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后我们根据误差矩阵来进行反向传播，首先不难求出误差矩阵的初始值，也就是最后一层的误差矩阵，前面我们已经计算过，直接替换就行
![image.png](http://upload-images.jianshu.io/upload_images/1234352-195c12e3fef1751f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后就是关键的动态规划的递推式，这个其实我们也已经在前面求解出来了，参照前面已经分析得出的
![image.png](http://upload-images.jianshu.io/upload_images/1234352-95b0c09ab053268f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

会发现，相邻两层间误差矩阵的关系就是激活函数的微分和权重，我们将其简化成向量的形式就是
![image.png](http://upload-images.jianshu.io/upload_images/1234352-3073f820640e0119.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

具体的证明如下，也就是链式法则的运用：
首先，
![image.png](http://upload-images.jianshu.io/upload_images/1234352-84fe66b46d4eabd9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
然后，
![image.png](http://upload-images.jianshu.io/upload_images/1234352-f911a1e197e0ad6e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
得到，
![image.png](http://upload-images.jianshu.io/upload_images/1234352-27e8db018a1fc097.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
代入，
![image.png](http://upload-images.jianshu.io/upload_images/1234352-3c92ca036b968c7c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

至此，一个完美的反向传播算法基本上已经大功告成了！
还差最后一丢丢，就是已经有了每一层的敏感度矩阵，也就是每一层关于加权输入的微分，最后再计算我们需要的每一层关于权重和偏置的微分，自然也是手到擒来，直接利用微分求导：
关于权重，
![image.png](http://upload-images.jianshu.io/upload_images/1234352-04c6708680eb4e3a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

关于偏置，
![image.png](http://upload-images.jianshu.io/upload_images/1234352-38376267d402a404.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 后向传播算法
后向传播等式给我们提供了一种计算代价函数梯度的方法。让我们用算法显示的写出它们：

1. 输入x：为输入层设置对应的激活a1。
2. 向前反馈：对于每一层l=1,2,3...,计算![image.png](http://upload-images.jianshu.io/upload_images/1234352-701a80bc8946903c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
和![image.png](http://upload-images.jianshu.io/upload_images/1234352-9055b70868884bf6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
3. 输出层误差δL：计算向量![image.png](http://upload-images.jianshu.io/upload_images/1234352-005fa777f95d1de4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
4. 后向传播误差：对每一层l=L-1,L-2,...2计算![image.png](http://upload-images.jianshu.io/upload_images/1234352-ed1890dd11590e94.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
5. 输出：代价函数的梯度为![image.png](http://upload-images.jianshu.io/upload_images/1234352-0613836247b0cd52.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

检查这个算法，你能看到为什么它被叫作后向传播。我们从最末一层开始向后计算各层误差δl。看起来将网络向后传播很奇怪。但是如果你再想一下后向传播的证明，向后移动就是因为代价是网络输出的函数。为了理解代价是如何跟随早期的权重和偏差进行的改变，我们需要不断的应用链式规则，向后来获取有用的表达式。

# 随机梯度下降的后向传播算法
就像我上面描述那样，后向传播算法计算了某一个样本的代价函数的梯度C=Cx。实际上，通用的方式是将后向传播算法与随机梯度下降算法合并，我们便能计算许多训练样本的梯度。特别的，给出一小批训练样本mm，下面算法将基于小批训练样本进行梯度下降学习步骤：
1. 输入训练样本集合
2. 对于每一个训练样本x： 设置对应的输入激活，执行以下步骤（参照前文的后向传播算法）：
    * 向前反馈：对于每一层l=1,2,3...,计算![image.png](http://upload-images.jianshu.io/upload_images/1234352-701a80bc8946903c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
和![image.png](http://upload-images.jianshu.io/upload_images/1234352-9055b70868884bf6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
    * 输出层误差δL：计算向量![image.png](http://upload-images.jianshu.io/upload_images/1234352-005fa777f95d1de4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
    * 后向传播误差：对每一层l=L-1,L-2,...2计算![image.png](http://upload-images.jianshu.io/upload_images/1234352-ed1890dd11590e94.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3. 梯度下降：按照更新法则更新权重和偏置
![image.png](http://upload-images.jianshu.io/upload_images/1234352-c45cb9dcc778a5a4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](http://upload-images.jianshu.io/upload_images/1234352-212d2d6887208b92.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当然，为了实施随机梯度下降，你也许要一个外部循环来产生小批量的训练样本，还需要一个外部循环来实施更多的训练代。我们将为了简化暂且忽略掉这些。

# 后向传播的实施代码
抽象上理解了后向传播算法，我们就能根据以上算法，实现一个完整的神经网络的后向传播的算法了！

```python
# %load network.py

"""
network.py
~~~~~~~~~~
IT WORKS

A module to implement the stochastic gradient descent learning
algorithm for a feedforward neural network.  Gradients are calculated
using backpropagation.  Note that I have focused on making the code
simple, easily readable, and easily modifiable.  It is not optimized,
and omits many desirable features.
"""

#### Libraries
# Standard library
import random

# Third-party libraries
import numpy as np

class Network(object):

    def __init__(self, sizes):
        """The list ``sizes`` contains the number of neurons in the
        respective layers of the network.  For example, if the list
        was [2, 3, 1] then it would be a three-layer network, with the
        first layer containing 2 neurons, the second layer 3 neurons,
        and the third layer 1 neuron.  The biases and weights for the
        network are initialized randomly, using a Gaussian
        distribution with mean 0, and variance 1.  Note that the first
        layer is assumed to be an input layer, and by convention we
        won't set any biases for those neurons, since biases are only
        ever used in computing the outputs from later layers."""
        self.num_layers = len(sizes)
        self.sizes = sizes
        self.biases = [np.random.randn(y, 1) for y in sizes[1:]]
        self.weights = [np.random.randn(y, x)
                        for x, y in zip(sizes[:-1], sizes[1:])]

    def feedforward(self, a):
        """Return the output of the network if ``a`` is input."""
        for b, w in zip(self.biases, self.weights):
            a = sigmoid(np.dot(w, a)+b)
        return a

    def SGD(self, training_data, epochs, mini_batch_size, eta,
            test_data=None):
        """Train the neural network using mini-batch stochastic
        gradient descent.  The ``training_data`` is a list of tuples
        ``(x, y)`` representing the training inputs and the desired
        outputs.  The other non-optional parameters are
        self-explanatory.  If ``test_data`` is provided then the
        network will be evaluated against the test data after each
        epoch, and partial progress printed out.  This is useful for
        tracking progress, but slows things down substantially."""

        training_data = list(training_data)
        n = len(training_data)

        if test_data:
            test_data = list(test_data)
            n_test = len(test_data)

        for j in range(epochs):
            random.shuffle(training_data)
            mini_batches = [
                training_data[k:k+mini_batch_size]
                for k in range(0, n, mini_batch_size)]
            for mini_batch in mini_batches:
                self.update_mini_batch(mini_batch, eta)
            if test_data:
                print("Epoch {} : {} / {}".format(j,self.evaluate(test_data),n_test));
            else:
                print("Epoch {} complete".format(j))

    def update_mini_batch(self, mini_batch, eta):
        """Update the network's weights and biases by applying
        gradient descent using backpropagation to a single mini batch.
        The ``mini_batch`` is a list of tuples ``(x, y)``, and ``eta``
        is the learning rate."""
        nabla_b = [np.zeros(b.shape) for b in self.biases]
        nabla_w = [np.zeros(w.shape) for w in self.weights]
        for x, y in mini_batch:
            delta_nabla_b, delta_nabla_w = self.backprop(x, y)
            nabla_b = [nb+dnb for nb, dnb in zip(nabla_b, delta_nabla_b)]
            nabla_w = [nw+dnw for nw, dnw in zip(nabla_w, delta_nabla_w)]
        self.weights = [w-(eta/len(mini_batch))*nw
                        for w, nw in zip(self.weights, nabla_w)]
        self.biases = [b-(eta/len(mini_batch))*nb
                       for b, nb in zip(self.biases, nabla_b)]

    def backprop(self, x, y):
        """Return a tuple ``(nabla_b, nabla_w)`` representing the
        gradient for the cost function C_x.  ``nabla_b`` and
        ``nabla_w`` are layer-by-layer lists of numpy arrays, similar
        to ``self.biases`` and ``self.weights``."""
        nabla_b = [np.zeros(b.shape) for b in self.biases]
        nabla_w = [np.zeros(w.shape) for w in self.weights]
        # feedforward
        activation = x
        activations = [x] # list to store all the activations, layer by layer
        zs = [] # list to store all the z vectors, layer by layer
        for b, w in zip(self.biases, self.weights):
            z = np.dot(w, activation)+b
            zs.append(z)
            activation = sigmoid(z)
            activations.append(activation)
        # backward pass
        delta = self.cost_derivative(activations[-1], y) * \
            sigmoid_prime(zs[-1])
        nabla_b[-1] = delta
        nabla_w[-1] = np.dot(delta, activations[-2].transpose())
        # Note that the variable l in the loop below is used a little
        # differently to the notation in Chapter 2 of the book.  Here,
        # l = 1 means the last layer of neurons, l = 2 is the
        # second-last layer, and so on.  It's a renumbering of the
        # scheme in the book, used here to take advantage of the fact
        # that Python can use negative indices in lists.
        for l in range(2, self.num_layers):
            z = zs[-l]
            sp = sigmoid_prime(z)
            delta = np.dot(self.weights[-l+1].transpose(), delta) * sp
            nabla_b[-l] = delta
            nabla_w[-l] = np.dot(delta, activations[-l-1].transpose())
        return (nabla_b, nabla_w)

    def evaluate(self, test_data):
        """Return the number of test inputs for which the neural
        network outputs the correct result. Note that the neural
        network's output is assumed to be the index of whichever
        neuron in the final layer has the highest activation."""
        test_results = [(np.argmax(self.feedforward(x)), y)
                        for (x, y) in test_data]
        return sum(int(x == y) for (x, y) in test_results)

    def cost_derivative(self, output_activations, y):
        """Return the vector of partial derivatives \partial C_x /
        \partial a for the output activations."""
        return (output_activations-y)

#### Miscellaneous functions
def sigmoid(z):
    """The sigmoid function."""
    return 1.0/(1.0+np.exp(-z))

def sigmoid_prime(z):
    """Derivative of the sigmoid function."""
    return sigmoid(z)*(1-sigmoid(z))

```
以上代码实现了一个完整的神经网络的类，里面包括前向传播，结合小批量随机梯度法实现的后向传播，可以直接应用于神经网络问题的求解！

# 写在最后
终于，我们从微分开始，讲到链式法则，从单个简单的神经元到嵌套神经元，再到两层的神经网络，最后到多层的神经网络，从微分结合链式法则的暴力进行反向传播的计算，到引入动态规划的计算，引入敏感度函数，真正理解了神经网络的反向传播算法！希望能对读者理解神经网络的反向传播有一定的帮助

# Further reading
* [How the backpropagation algorithm works](http://neuralnetworksanddeeplearning.com/chap2.html). 
* [A_Gentle_Introduction_to_Backpropagation](http://numericinsight.com/uploads/A_Gentle_Introduction_to_Backpropagation.pdf).
* [jasdeep06](https://jasdeep06.github.io/)


 > 本文相关代码可以从[Backpropagation](https://github.com/chi2liu/Backpropagation)下载
