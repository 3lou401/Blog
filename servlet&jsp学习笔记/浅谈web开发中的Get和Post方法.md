在http协议中，实际上有八个http方法。但在实际开发中，绝大多数情况我们只会用到两个方法，就是get和post。所以我们来稍微谈谈两种方法的区别，以及何时应该选取何种方法。

# get和post的区别

## post有一个体！
这个是关键。

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-8525725b2feeda6c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-9edd6e5ab36db177.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

get和post都能发送参数，但是利用get的话，对参数数据量有限制，因为参数只能是放在请求行的内容中。而post由于在体中，则没有数据量的限制。
***
** 所以总结一下，第一方面的区别数据量的大小限制 **
***
但不仅仅是数据的大小。
使用get时，参数数据会显示在浏览器的输出栏，这就引发了安全问题。
同时还有一个问题，就是get可以建立书签，而post请求则不可以。

** 除了上述的数据量大小，安全，书签的差别之外，还有一个非常重要的差别就是是否幂等**

什么是幂等呢？
幂等就是只是简单的获取服务器上的信息，而不会对服务器上的内容进行改变，所以进行多次重复操作后，不会有预料不到的副作用。可以一遍一遍的反复做同一件事情而且不会出问题。这就是幂等的意义。

** get是幂等的，而post不是幂等的**
