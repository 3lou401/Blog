> The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)
P   A   H   N
A P L S I I G
Y   I   R
And then read line by line: "PAHNAPLSIIGYIR"
Write the code that will take a string and make this conversion given a number of rows:
string convert(string text, int nRows);
convert("PAYPALISHIRING", 3) should return "PAHNAPLSIIGYIR".

# 分析
这道题就是要根据z字形遍历，我们模拟一遍过程可以发现遍历的规律，可以用循环解决，先遍历下去，又向上。然后重复这个步骤，向下，向上！

# 代码
```
public String convert(String s, int nRows) {
		char[] c = s.toCharArray();
		int len = c.length;
		StringBuilder[] sb = new StringBuilder[nRows];
		for(int i=0;i<sb.length;i++)
			sb[i] = new StringBuilder();
		int i=0;
		while(i<len) {
			for(int index=0;index<nRows && i<len;index++) {
				sb[index].append(c[i]);
				i++;
			}
			for(int index=nRows-2;index>=1 && i<len;index--) {
				sb[index].append(c[i]);
				i++;
			}
		}
		for(int index=1;index<sb.length;index++) {
			sb[0].append(sb[index]);
		}
		return sb[0].toString();
    }
```


![image.png](http://upload-images.jianshu.io/upload_images/1234352-3477a83e5161ab25.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
