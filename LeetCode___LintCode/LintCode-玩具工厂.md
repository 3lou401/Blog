# 题目
工厂模式是一种常见的设计模式。请实现一个玩具工厂 ToyFactory
 用来产生不同的玩具类。可以假设只有猫和狗两种玩具。
**样例**

```
ToyFactory tf = ToyFactory();
Toy toy = tf.getToy('Dog');
toy.talk(); 
>> Wow

toy = tf.getToy('Cat');
toy.talk();
>> Meow
```

# 分析
这个题目较为简单，考察设计模式中工厂模式的使用，可以参考我的文集设计模式中的工厂模式

# 代码
```
/**
 * Your object will be instantiated and called as such:
 * ToyFactory tf = new ToyFactory();
 * Toy toy = tf.getToy(type);
 * toy.talk();
 */
interface Toy {
    void talk();
}

class Dog implements Toy {
    // Write your code here
    public void talk() {
        System.out.println("Wow");
   }
}

class Cat implements Toy {
    // Write your code here
    public void talk() {
        System.out.println("Meow");
   }
}

public class ToyFactory {
    /**
     * @param type a string
     * @return Get object of the type
     */
    public Toy getToy(String type) {
        // Write your code here
        if (type == null) {
            return null;
        }		
        if (type.equalsIgnoreCase("Dog")) {
            return new Dog();
        } else if(type.equalsIgnoreCase("Cat")) {
            return new Cat();         
        }
        return null;
    }
}
```
