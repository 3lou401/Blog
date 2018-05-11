 > 本文相关代码可以从[Backpropagation](https://github.com/chi2liu/Backpropagation)下载

在上一篇文章[小白也能看懂的BP反向传播算法之Into-Backpropagation](https://www.jianshu.com/p/2e02bc6384a8) 
，我们研究了一个嵌套神经元的反向传播的计算，了解到反向传播本质就是利用链式法则，求取所需要更新的变量的偏导数！但我们前文所研究的神经元是比较简单的，没有复杂的函数，也没有复杂的结构，而真实的神经网络中，往往神经元的函数和结构都比较复杂！

为了更好的过渡到复杂的神经网络中的反向传播，本文先引入复杂函数，也就是神经网络中最基本的激活函数，并联系如何计算反向传播，为后续进入神经网络的反向传播计算打下坚实的基础！

# Lets get started!!!
我们将引入神经网络最常见的激活函数sigmoid函数！
![image.png](http://upload-images.jianshu.io/upload_images/1234352-f58b122fadbd679f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](http://upload-images.jianshu.io/upload_images/1234352-ae375582d596d1af.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

实现这个单一神经元很简单
```python
import numpy as np
def sigmoid(x):
	return 1/(1+np.exp(-x))
a=-2
f=sigmoid(a)
print(f) #outputs 0.1192
```

# Aim

接下来依旧是老套路，我们是=试着使输出值增加。首先我们 就要计算Sigmoid的函数的导数，根据微分的法则，我们可以求出
![image.png](http://upload-images.jianshu.io/upload_images/1234352-841bbbdebf77b559.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后，就可以得到更新变量的方程：
![image.png](http://upload-images.jianshu.io/upload_images/1234352-8918ed133a024b0f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们用python实现：
```python
import numpy as np


def sigmoid(x):
    return 1./(1+np.exp(-x))


def derivative_sigmoid(x):
    return sigmoid(x) * (1 - sigmoid(x))


a = -2
h = 0.1
a = a + h * derivative_sigmoid(a)
f = sigmoid(a)
print(f)  #outputs 0.1203
```
观察输出结果，0.1203比0.1192大.所以我们的算法成功将输出值增加！

现在我们已经知道如何对一个复杂的函数的神经元进行反向传播，从而改变输出值！那么，接下来我们就将复杂函数放到一个嵌套的神经网络结构中，看看如何进行反向传播的计算：
![image.png](http://upload-images.jianshu.io/upload_images/1234352-85b05d75d2be4d79.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这个神经网络的结构就是在前文的基础上增加了一个sigmoid函数！我们先用python实现它的正向传播
```python
import numpy as np


def addition(x,y):
	return x+y


def product(x, y):
    return x * y


def sigmoid(x):
    return 1 / (1 + np.exp( -x ))


a=1
b=-2
c=-3
d=addition(a,b)
e=product(c,d)
f=sigmoid(e)
print(f)  #outputs 0.952574
```

现在我们开始计算反向传播，首先很明确的是，要进行反向传播，就得求得所要更新变量的微分：
![image.png](http://upload-images.jianshu.io/upload_images/1234352-a1fcd76aadde8b3b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

所以我们需要的计算就是a，b，c三个变量的偏导数！具体的求解规则和前文一样就是倒着从输出往回推，看看经过了哪些神经元的计算，然后利用链式法则：
![image.png](http://upload-images.jianshu.io/upload_images/1234352-a74cc27ab57cb19d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

希望读者能独立推导出上述的公式！

得到上述微分的计算公式，我们就要开始实际计算这些微分值，不难求出
![image.png](http://upload-images.jianshu.io/upload_images/1234352-8a74c0010e4a6b1c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果读者对此推导过程依旧有疑问，请重新阅读前两篇文章即能理解！

最后，就是编写程序来实现反向传播了！

```python
import numpy as np


def addition(x, y):
    return x + y


def product(x, y):
    return x * y


def sigmoid(x):
    return 1 / (1 + np.exp(-x))


def derivative_sigmoid(x):
    return sigmoid(x) * (1 - sigmoid(x))


# initialization
a = 1
b = -2
c = -3
# forward-propogation
d = addition(a, b)
e = product(c, d)
# step size
h = 0.1
# derivatives
derivative_f_e = derivative_sigmoid(e)
derivative_e_d = c
derivative_e_c = d
derivative_d_a = 1
derivative_d_b = 1
# backward-propogation (Chain rule)
derivative_f_a = derivative_f_e * derivative_e_d * derivative_d_a
derivative_f_b = derivative_f_e * derivative_e_d * derivative_d_b
derivative_f_c = derivative_f_e * derivative_e_c
# update-parameters
a = a + h * derivative_f_a
b = b + h * derivative_f_b
c = c + h * derivative_f_c
d = addition(a, b)
e = product(c, d)
f = sigmoid(e)
print(f)  # prints 0.9563
```
输出结果是0.9563比0.9525大，可以看到，经过一次反向传播，我们的输出值成功增加！

经过练习，我们可以发现，不管网络多复杂，无非是链式法则求导是复杂一些，只要我们能求出微分，就能进行反向传播！

# 待续
我们目前练习的都还是比较简单的网络，但恭喜你已经了解到反向传播的最核心的思想！下一篇文章[小白也能看懂的BP反向传播算法之Further into Backpropagation](https://www.jianshu.com/p/96a5ba02bacc)，我们会正式引入一个真实的神经网络结构，然后进行反向传播的计算！并且利用矩阵来简化计算过程！

 > 本文相关代码可以从[Backpropagation](https://github.com/chi2liu/Backpropagation)下载
