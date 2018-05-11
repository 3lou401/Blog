# 题目
给定两个二进制字符串，返回他们的和（用二进制表示）。

**样例**
a =11

b =1

返回100

# 分析
二进制进行求和，思想较为简单，就是需要搞清楚进位之间的关系。

具体看代码就明白了
 # 代码
```
public class Solution {
    /**
     * @param a a number
     * @param b a number
     * @return the result
     */
    public String addBinary(String a, String b) {
        
        char ch_a[] = a.toCharArray();      //获得a字符串的字符数组  
        char ch_b[] = b.toCharArray();     //获得b字符串的字符数组  
        List<Integer> list_a = new ArrayList<Integer>();//定义一个List记录a中的每一位  
        List<Integer> list_b = new ArrayList<Integer>();//定义一个List记录b中的每一位  
        //获得a中的每一位  
        for(int i=ch_a.length-1;i>=0;i--){  
            list_a.add(Integer.parseInt(String.valueOf(ch_a[i])));  
        }  
  
        //获得b中的每一位  
  
        for(int i=ch_b.length-1;i>=0;i--){  
            list_b.add(Integer.parseInt(String.valueOf(ch_b[i])));  
        }  
         
        if(list_a.size()>=list_b.size()){  
            return getValue(list_a,list_b);  
        }else{  
            return getValue(list_b,list_a);  
        }  
   }  
  
   //将两个列表中的每一位相加，根据结果判断是否有进位，以及在list_c中记录相加的结果  
  
   private String getValue(List<Integer> list_a,List<Integer> list_b){  
       ArrayList<Integer> list_c = new ArrayList<Integer>();  
       int carry=0;//进位标志  
         
           for(int i=0;i<list_b.size();i++){  
               if((list_a.get(i)+list_b.get(i)+carry)==2){  
                   list_c.add(i, 0);  
                   carry = 1;  
               }else if((list_a.get(i)+list_b.get(i)+carry)==3){  
                   list_c.add(i, 1);  
                   carry = 1;  
               }  
               else if((list_a.get(i)+list_b.get(i)+carry)==1){  
                   list_c.add(i, 1);  
                   carry = 0;  
               }else if((list_a.get(i)+list_b.get(i)+carry)==0){  
                   list_c.add(i, 0);  
                   carry = 0;  
               }  
           }  
           for(int i=list_b.size();i<list_a.size();i++){  
               if((list_a.get(i) +carry)==2){  
                   list_c.add(i, 0);  
                   carry = 1;  
               }else if((list_a.get(i)+carry)==1){  
                   list_c.add(i, 1);  
                   carry = 0;  
               }else if((list_a.get(i)+carry)==0){  
                   list_c.add(i, 0);  
                   carry = 0;  
               }  
           }  
           if(carry==1){  
               list_c.add(1);  
           }  
             
           String string = "";  
           for(int i = list_c.size()-1;i>=0;i--){  
               string+=list_c.get(i);  
           }  
            return string;
        
    }
}
```   
