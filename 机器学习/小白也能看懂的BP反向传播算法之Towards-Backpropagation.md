 > 本文相关代码可以从[Backpropagation](https://github.com/chi2liu/Backpropagation)下载

想要理解backpropagation反向传播算法，就必须先理解**微分**！本文会以一个简单的神经元的例子来讲解backpropagation反向传播算法中的微分的概念。
![image.png](http://upload-images.jianshu.io/upload_images/1234352-83a3bde1ba80b0b6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这是一个非常简单的神经元，如图所示，接收两个输入a， b，然后在神经元内部对两个神经元进行相乘操作，得到a*b的输出！

然后，我们可以想象这样一个情景，现在对于某个特定的输入a，b，输出结果与预期结果相比较小，为了提高准确率，那我们就要将输出结果增加，以更符合预期的结果！

所以，我们现在的目标就是通过改变a，b的值来增加神经元的输出！

# Method 1

第一个方法是很直观的，我们就随机的给输入a，b的两个值添加一个随机值，然后用一个步长step来控制增加的程度，用python代码实现：

```python
import random


def product(a, b):
	return a * b


def methodOne():
    a = 5
    b = 6
    step = 0.01
    a = a + step * (random.random())
    b = b + step * (random.random())
    print(product(a, b))

methodOne()
```

运行程序，我们可以得到输出30.04698146865633（**由于涉及到随机数，所以读者的运行结果会和此处有区别，但一定会是比30大的数**），是比原本的输出30增加了相应值的，说明我们的目标实现了！但是如果我们继续测试，就会发现程序是存在问题的：
* 不可靠！如果我们改变输入值，会发现有时候值会增加，有时候又会减少！，比如以下这个例子:

```python
def testTwo():
    a = -5
    b = -6
    step = 0.01
    a = a + step * (random.random())
    b = b + step * (random.random())
    print(product(a, b))

testTwo()
```
运行程序，我们会得到结果29.970177554213326，是比原本的输出30小的。只因为我们将输入值a，b变为了负数，这个算法就失效了！究其原因，因为我们对于不同的输入值，变化的都是random()函数，也就是一个在（0，1）之间的正数。而对于负数的输入，增加一个正数，最后反而导致绝对值减小，也就导致输出的结果变小了！

所以，通过以上这个例子，我们可以得出结论：
**对于 ``` a = a + step * (random.random()) ```中step后面所乘的系数，我们不能一概而论，而是应该更多的控制它的值，也就是根据不同的输入值来控制，也就是说，step所乘的系数，也就是输入值的变化应该是一个关于输入值的函数**

# Method 2

Method 1中的结论是，我们应该有一个step乘以的应该是一个关于输入值的函数。也就是我们需要知道在输入值那个点上，输出结果关于输入值的一个变化率，如果在这个点上，输出结果是随着输入值增加而增加，那么step乘以一个正数即可，反之，则需要乘以一个负数才能使输出结果增加！显然这就是函数在某点上的**微分**的定义！

首先我们将神经元内部的函数写出来
![image.png](http://upload-images.jianshu.io/upload_images/1234352-760c542176c2a704.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

神经元输出是一个关于输入的函数！然后我们将输入值a增加一个h

![image.png](http://upload-images.jianshu.io/upload_images/1234352-58cba2dd010d5eb5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

此时新的输出值就变成 ```(a+h)*b```，展开就是```a*b+h*b```，所以我们可以看到此时输出值相对于原有的输出值的变化就是```h*b```，只要我们保证```h*b```是正值，就是说可以保证是输出结果增加，而保证正值的方法自然就是h和b同号！更深入的理解，我们将a的值增加h，最后导致输出结果，也就是 函数f的值增加```h*b```，这时候就可以理解b为函数f在点（a，b）处关于a的变化率！这就是微分的概念了，如果读者有微积分基础，会发现这里实际上f就是一个二元函数的微分，b就是函数f在点（a，b）的微分！微分的实质就是函数在某点的变化率，如果是多元函数就是函数在某点关于某个变量的变化率！多元函数对某一个变量微分时候，通常会将其他变量看作常量！
![image.png](http://upload-images.jianshu.io/upload_images/1234352-aa33fce47827bb8e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
以上公式就是微分的计算方法！

但实际上，学过微积分，都应该掌握了一套微分的基本法则，我们往往可以根据这套法则，直接写出函数的微分！可以参考[Derivative-rules](https://www.mathsisfun.com/calculus/derivatives-rules.html)
对于我们这里的函数f，我们可以直接写出关于a的微分

![image.png](http://upload-images.jianshu.io/upload_images/1234352-5915d878aa286509.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

自然，关于b的微分就是

![image.png](http://upload-images.jianshu.io/upload_images/1234352-05dcf5a4602d489d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

求出了微分，就相当于知道了变量在某点的变化率，那么很自然，a，b的更新规则就是

![image.png](http://upload-images.jianshu.io/upload_images/1234352-96eb7260f937de54.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```python
def methodTwo(a, b):
    step = 0.01
    a = a + step * b
    b = b + step * a
    print(product(a, b))

methodTwo(5, 6)
methodTwo(-5, -6)
```
我们发现输出结果30.616035999999998和30.616035999999998，不管输入的值是正是负，都成功将输出结果增加了！而正是利用了微分的概念才能做到，微分实际上就是梯度，梯度就指明了函数上升最快的方向！读者可以参考笔者相关梯度下降的文章[*梯度下降*](https://liuchi.coding.me/2018/01/17/%E6%B7%B1%E5%85%A5%E6%B5%85%E5%87%BA-%E6%A2%AF%E5%BA%A6%E4%B8%8B%E9%99%8D%E6%B3%95%E5%8F%8A%E5%85%B6%E5%AE%9E%E7%8E%B0/)

最后我们来详细分析一下，为什么利用微分更新输入可以做到？

我们深入分析微分几何意义！一个函数对某个变量的微分实际上就是函数在关于这个变量的变化率，变化率的方向是正的，也就是说的变化率，指的是增加的变化率！我们看看我们输入的更新方程

![image.png](http://upload-images.jianshu.io/upload_images/1234352-26c9f603c20e5829.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 多元函数的微分关于某个变量的微分的意义就是，就是将其他变量全部看成常量，函数关于此变量的变化率，变化率的方向是正的，也就是说上升的方向，如果想要知道下降的变化率，即是负微分

我们再举一个单变量函数的例子来分析一下如何利用微分更新变量值

![image.png](http://upload-images.jianshu.io/upload_images/1234352-72bcac57c4c5a496.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们看到上面这个函数关于x先上升，再下降！我们假设两个场景

场景1：

首先，想象我们现在处于A点，我们想要改变变量x的值，从而使函数y的值增加。从图中我们可以清晰的看出，我们只要直接增加x的值，就可以增加函数值。然后结合微分，我们的更新方法就是

![image.png](http://upload-images.jianshu.io/upload_images/1234352-a4a476c50b12b44b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

场景2：

现在想象我们在C点，也要改变x的值让函数值y增加，这时候我们从图像中可以看到，我们减少x的值，就可以增加函数值！而这正是由函数的微分决定的。我们求取函数关于x在此点的微分，会发现微分是负值，所以我们依然利用场景1中的更新公式，也可以达到增加函数值的目的。

> 微分会反映函数的变化趋势，如果我们想要让函数值增加，微分会告诉我们一个正确的更新变量的方向

场景3：

想象我们此时处于B的左侧，但是非常接近B点！从图像中，我们也会知道此时微分的是正的，也就是我们增加x的值就能使函数值y的值增加！但要注意的一点是，如果我们的step也就是步长如果过大，会导致跑到右边去了，可能还会导致函数值的下降！所以我们就要调整步长！这其实就是一个梯度上升的问题，与梯度下降类似，读者有兴趣可以参考笔者相关文章[*梯度下降*](https://liuchi.coding.me/2018/01/17/%E6%B7%B1%E5%85%A5%E6%B5%85%E5%87%BA-%E6%A2%AF%E5%BA%A6%E4%B8%8B%E9%99%8D%E6%B3%95%E5%8F%8A%E5%85%B6%E5%AE%9E%E7%8E%B0/)


我会在这里结束本文的介绍！希望通过本文读者能对微分有一个理解，同时知道如何将微分利用到更新变量值中，从而改变函数值！

**下一篇文章[小白也能看懂的BP反向传播算法之Into-Backpropagation](https://www.jianshu.com/p/2e02bc6384a8)我会将这个利用微分更新变量的方法，应用到多个神经元的场景中，慢慢读者就会接触到真正的backpropagation反向传播算法**

 > 本文相关代码可以从[Backpropagation](https://github.com/chi2liu/Backpropagation)下载
