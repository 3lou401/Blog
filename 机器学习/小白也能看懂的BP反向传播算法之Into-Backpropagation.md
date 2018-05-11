 > 本文相关代码可以从[Backpropagation](https://github.com/chi2liu/Backpropagation)下载

在上一篇文章[小白也能看懂的BP反向传播算法之Towards-Backpropagation](https://www.jianshu.com/p/924398a59c6b)，我们学习了如何利用函数的微分来更新变量值，是函数值发生相应的变化！
例如，对于函数
![image.png](http://upload-images.jianshu.io/upload_images/1234352-882774e49c335a5e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
我们想要更新变量a，b的值使f的值增加，就可以根据以下公式来更新
![image.png](http://upload-images.jianshu.io/upload_images/1234352-396f377f270f9705.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

实际上这就是反向传播的最基本的思想！我们试想假设f函数是一个代价函数，神经网络的训练就是将代价函数的值变小，那么就是问题就变成了，对于一个代价函数f，我们将改变f的变量，使其f能减小，而f不就是关于每个神经元权重和偏置的函数么，```f = f(w,b)```。只不过是代价函数的变量更多，函数形式更复杂，更新起来相对复杂，这个我们将在后面详细介绍！但其实反向传播的基本思想就是根据微分去更新！

接下来，我们就将问题慢慢复杂化，一步一步接近最终的神经网络中的反向传播！

前文中，我们利用的是一个神经元，这里我们讲问题变复杂，变成两个神经元，并且是有嵌套关系的两个神经元！如下图：
![image.png](http://upload-images.jianshu.io/upload_images/1234352-273f5828e57e5a23.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

，将输入值相加然后输出到第二个神经元，同时第二个神经元还接受输入c，并将两个值相乘，最后输出！
这个简单网络的正向传播很容易写出来：
```python
def product(x, y):
    return x * y


def addition(x, y):
    return x + y


def forward(a, b, c):
    d = addition(a, b)
    return product(d, c)


print(forward(5, -6, 7))
```
Our aim is still the same as was in last post viz;we want to manipulate the values of our inputs `a`,`b`,`c` in such a way that the value of output `f`increases.

现在开始我们的训练吧！
目标和之前一样，就是改变输入的`a`,`b`,`c`三个值，使函数`f`的值增加！之后我们就会发现，在这个过程中，我们会慢慢接触到反向传播的核心思想！
[previous post](https://jasdeep06.github.io/posts/towards-backpropagation/).

初看上去，这个网络似乎比之前的要复杂，但依照我们前一篇文章提出的思路！我们先看函数f，函数f是一个关于输入a，b，c的函数，想要让函数f的值增加，直接微分即可，然后加上步长与微分的乘积，如下：
![image.png](http://upload-images.jianshu.io/upload_images/1234352-b5e485d95f5fc6eb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

所以，核心问题在此就变成怎么求解函数f关于a,b,c的微分！
我们不能向之前一个神经元那样直接计算，因为此处的神经元是相互嵌套的。我们将函数f反着往回写！
首先，与函数f直接关联的神经元，就是接受两个输入，一个来自第一个神经元的，一个来自输入c，我们把第一个神经元的输出记作d，那么函数f就可以写成
![image.png](http://upload-images.jianshu.io/upload_images/1234352-4279ea44a13d3dc5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后我们继续反着往前推，对于d，其实就是第一个神经元的输出，也就是可以直接写成：
![image.png](http://upload-images.jianshu.io/upload_images/1234352-90d97ec17a55336f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这样我们就反向的把函数f简化成了两个函数：
![image.png](http://upload-images.jianshu.io/upload_images/1234352-bfb2f672ea298770.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

熟悉微积分的朋友应该就知道，我们在此可以利用函数求微分的链式法则。
首先考虑![image.png](http://upload-images.jianshu.io/upload_images/1234352-f9b749935144026a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

分别求微分之后，如下：
![image.png](http://upload-images.jianshu.io/upload_images/1234352-83ece3293d14069a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后再对![image.png](http://upload-images.jianshu.io/upload_images/1234352-9250c41466accab0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)求微分

![image.png](http://upload-images.jianshu.io/upload_images/1234352-1c0b5a3fcaa715a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

现在我们有四个微分的值：
![image.png](http://upload-images.jianshu.io/upload_images/1234352-ae50b60c2a290659.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们的目标是求取这三个微分的值：
![image.png](http://upload-images.jianshu.io/upload_images/1234352-56635db1c2b14837.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# Backpropagation
这个时候，就轮到链式法则出场了！
链式法则其实就是：
![image.png](http://upload-images.jianshu.io/upload_images/1234352-427860d0177a4586.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如我们所见，链式法则有点代数的特点；因为莱布尼兹的导数符号表明两个分式中的du可以消掉，所以这个公式很好记忆。如果我们将导数看作变化率的话，直观上也很容易理解：

**如果y的变化速度是u的a倍，u的变化速度是x的b倍，那么y的变化速度是x的ab倍。**

**或者用日常用语来说，如果车的速度是自行车的两倍，自行车的速度是步行的四倍，那么车的速度是步行的2⋅4=8倍。**

所以此处我们对a，b应用链式法则，就能求取出微分：
![image.png](http://upload-images.jianshu.io/upload_images/1234352-bc418e270f8b6ed6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](http://upload-images.jianshu.io/upload_images/1234352-139c8ba60ada8f8f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

所以最后求出微分就是：
![image.png](http://upload-images.jianshu.io/upload_images/1234352-5fceaa54d1bf466a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

根据以上求出的微分，我们就能很好的写出变量的更新规则：
![image.png](http://upload-images.jianshu.io/upload_images/1234352-59660d107861efa1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们用python实现上面的更新的过程：
```python
def product(x, y):
    return x * y


def addition(x, y):
    return x + y


def forward(a, b, c):
    d = addition(a, b)
    return product(d, c)


print(forward(5, -6, 7))# output -7


def update(a, b, c):
    d = addition(a, b)
    h = 0.01

    derivative_f_d = c
    derivative_f_c = d
    derivative_d_a = 1
    derivative_d_b = 1

    derivative_f_a = derivative_f_d * derivative_d_a
    derivative_f_b = derivative_f_d * derivative_d_b

    a = a + h * derivative_f_a
    b = b + h * derivative_f_b
    c = c + h * derivative_f_c

    d = addition(a, b)
    return product(d, c)


print(update(5, -6, 7))# output -6.0113999999999965
```
可以看到更新之后的输出确实增大了！说明我们现在已经可以实现嵌套神经元的变量参数的更新了！离真正的反向传播的又近了一步！

Why did it work?

接下来，我们深入整个更新的过程，一步步分析看看究竟更新的本质是什么。首先，我们从输入到输出，分析一遍，前向传播，输入值为a=5,b=-6,c=7，很容易发现，第一个神经元的输出为d为1，然后第二个神经元输出为-7，所以最后结果就是-7!这就是此网络前向传播的过程！

然后我们开始反向传播，从输出开始分析，我们现在的目标是将输出的值增大，输出值是由-1*7得到的，现在要增加输出值，我们先不看微分，显然就是增加-1，减少7，这样就能使他们的乘积变大！我们再来计算微分，微分结果就是增加d的值，减少c的值。我们继续反向推理，这里d的值又是有a，b的值决定的！我们现在的目标又变成了
对于第一个神经元，减少输出值，那么很显然，只要减少a和b的值就行了，我们运用链式法则求取a，b的微分，也能得到相同的结果！
我们可以看到，当我们反向传播的时候，一个神经元的输入会变成上一个神经元的输出，然后他们之间相互影响，从而使传播下去！

我们倒着从输出分析到输入的过程就是反向传播的过程！我们通过计算微分可以从输出到输入更新变量的值，以使得输出朝着我们期待的方向的变化！

# 待续
本文就在这里结束了！本文将前文的更新变量的算法扩展到嵌套的多个神经元中，并应用到了链式法则求微分！而且在这个过程中，其实我们已经逐渐接触到反向传播的基本思想！

下一篇文章[小白也能看懂的BP反向传播算法之Let's practice Backpropagation](https://www.jianshu.com/p/1ba80e1ffe1e)，我们会将算法应用到一个标准的神经网络中，让我们看看真正的反向传播算法是什么样的！

 > 本文相关代码可以从[Backpropagation](https://github.com/chi2liu/Backpropagation)下载
