# 问题由来
*** 
在阅读google的开源项目zxing时，遇到以下代码：
***
```
public final String toString() {
    byte[] row = new byte[width];
    StringBuilder result = new StringBuilder(height * (width + 1));
    for (int y = 0; y < height; y++) {
      row = getRow(y, row);
      for (int x = 0; x < width; x++) {
        int luminance = row[x] & 0xFF;	
        char c;
        if (luminance < 0x40) {
          c = '#';
        } else if (luminance < 0x80) {
          c = '+';
        } else if (luminance < 0xC0) {
          c = '.';
        } else {
          c = ' ';
        }
        result.append(c);
      }
      result.append('\n');
    }
    return result.toString();
  }
```
***
阅读到上述源代码时，对于`int luminance = row[x] & 0xFF;	`最初不是很理解。查询之后，发现原来Java中是没有unsigned byte type的。也就是说Java中所有的byte类型都是signed类型。只能表达（-128~127）.而此处的代码为了读取像素值，所需要的值是（0~255），所以需要的是unsigned byte而不是signed byte。但是Java中所有的byte都是signed byte。那怎么处理呢？
# Java中unsigned byte 的转换
正如上述我们看到的代码所示：
`int luminance = row[x] & 0xFF;`
首先widening类型。将byte声明为short或者int类型。然后与0xFF取&即可。

下面，具体说明这样做的原理。
0xff 表示为二进制就是 1111 1111。在signed byte类型中，代表-1；但在short或者int类型中则代表255.
当把byte类型的-1赋值到short或者int类型时，虽然值仍然代表-1，但却由1111 1111变成1111 1111 1111 1111.
再将其与0xff进行掩码：
  -1: 11111111 1111111
0xFF: 00000000 1111111
 255: 00000000 1111111
所以这样，-1就转换成255.
# 测试程序
我们写了一个简单的程序对其进行Java unsigned byte 类型转换的测试：
`for (byte b = Byte.MIN_VALUE; b < Byte.MAX_VALUE; b++) {
    short s = b;
    s &= 0xff;
    System.out.println(b + " & 0xFF = " + s);
}`
将所有的byte值进行循环转换，输出结果如下：
`-128 & 0xFF = 128
-127 & 0xFF = 129
....
-2 & 0xFF = 254
-1 & 0xFF = 255
0 & 0xFF = 0
1 & 0xFF = 1
...
125 & 0xFF = 125
126 & 0xFF = 126`
# 小结
Java的unsigned byte 类型转换属于一个细节问题，由于java中没有内置unsigned byte类型，所以当我们需要使用其时，需要对signed byte 类型进行转换。而这种转换是比较简单的，首先将其扩大类型到short或者int，然后对0xff进行掩码即可。

## 备注
2016.7.5阅读zxing源码时的小问题
