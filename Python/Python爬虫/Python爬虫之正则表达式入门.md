正则表达式是用来简洁表达一组字符串的表达式

> 使用正则表达式的优势是什么？
简洁
一行胜千言 一行就是特征(模式)

无穷字符串组的简洁表达


![image.png](http://upload-images.jianshu.io/upload_images/1234352-4def29e82a4db174.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

某种特征字符串组的简洁表达


![image.png](http://upload-images.jianshu.io/upload_images/1234352-c45604a44887bd35.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


> 正则表达式是用来简洁表达一组字符串的表达式
正则表达式是一种通用的字符串表达框架
正则表达式是一种针对字符串表达“简洁” 和“特征” 思想的工具
正则表达式可以用来判断某字符串的特征归属

> 正则表达式在文本处理中十分常用：
表达文本类型的特征（病毒、入侵等）
同时查找或替换一组字符串
匹配字符串的全部或部分
……
最主要应用在字符串匹配中

编译：将符合正则表达式语法的字符串转换成正则表达式特征


![image.png](http://upload-images.jianshu.io/upload_images/1234352-ea8979ef682f3970.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


# 正则表达式语法

正则表达式语法由字符和操作符构成

![image.png](http://upload-images.jianshu.io/upload_images/1234352-794c762ccbf187e3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-df124dedbeae1db8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


# 正则表达式实例


![image.png](http://upload-images.jianshu.io/upload_images/1234352-8e123dd7c1037767.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-b98bd75fbc6d05e1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



![image.png](http://upload-images.jianshu.io/upload_images/1234352-178af03158beff9a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# Re
Re库是Python的标准库，主要用于字符串匹配
调用方式：
import re

raw string类型（原生字符串类型）
re库采用raw string类型表示正则表达式，表示为：


![image.png](http://upload-images.jianshu.io/upload_images/1234352-701a052aa5c73f6e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

raw string是不包含对转义符再次转义的字符串

re库也可以采用string类型表示正则表达式，但更繁琐
例如：

![image.png](http://upload-images.jianshu.io/upload_images/1234352-37de0f53a2f3d041.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


建议：当正则表达式包含转义符时，使用raw string


![image.png](http://upload-images.jianshu.io/upload_images/1234352-7ae531a30afbbd5f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



![image.png](http://upload-images.jianshu.io/upload_images/1234352-aed2a08ac0aab362.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-76c1574c2f1fc825.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-074dfcba22e6be56.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-612f971db0aa5042.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-df2d0c96e465bb2e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-aa36d7d64ad79f69.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-84be948b238d7941.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-8c754482502edf53.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-75e3132a34316ac9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-f608f1c6bae5dba7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-8166420a1915e362.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-74ea07bb8c080fa4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# Match对象
Match对象是一次匹配的结果，包含匹配的很多信息


![image.png](http://upload-images.jianshu.io/upload_images/1234352-9a8ac3c1e4a9cb2f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-c2e8e2003b7d25ea.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-d9dd50f9f1b4454a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


# 贪婪匹配和最小匹配

![image.png](http://upload-images.jianshu.io/upload_images/1234352-822d47c4d2ec59e3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](http://upload-images.jianshu.io/upload_images/1234352-eced055df52095c4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-ca794ac572f790e0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

只要长度输出可能不同的，都可以通过在操作符后增加?变成最小匹配
