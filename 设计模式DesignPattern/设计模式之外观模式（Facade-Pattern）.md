外观模式外部访问内部复杂代码的一个接口，举个例子，我们知道打开一台电脑很简单，只要按开机键，但实际上在后台我们看不到的地方，计算机进行了很多复杂的工作，比如，cpu。内存。硬盘等的启动。但我们不需要亲自去启动这些复杂的步骤，我们只需要知道按下开机键，电脑就会启动。
实际上这里就是使用了外观模式，外观模式提供了一个简单的接口，为我们封装好了访问内部代码的复杂操作，有了外观模式，我们只需要简单的按下开机键，就可以自动调用cpu。硬盘。内存的方法帮我们启动电脑。


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-c44872003d3206eb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

参看外观模式的类图，我们可以看到外观模式将多个复杂的操作封装起来，只对外提供一个简单的接口。

下面我们就简单的实现一个外观模式，以电脑的启动为例：
```
 
class CPU {
    public void processData() { }
}
 
class Memory {
    public void load() { }
}
 
class HardDrive {
    public void readdata() { }
}
 
/* Facade */
class Computer {
    private CPU cpu;
    private Memory memory;
    private HardDrive hardDrive;
 
    public Computer() {
        this.cpu = new CPU();
        this.memory = new Memory();
        this.hardDrive = new HardDrive();
    }
 
    public void run() {
        cpu.processData();
        memory.load();
        hardDrive.readdata();
    }
}
 
 
class User {
    public static void main(String[] args) {
        Computer computer = new Computer();
        computer.run();
    }
}
```

> 外观模式将子系统的方法封装起来，提供一个更上层的接口，提供更简单的访问


![Upload Paste_Image.png failed. Please try again.]

# 外观模式的优缺点

## 优点

* 减小系统间的相互依赖
* 提高灵活性
* 减小系统依赖
* 提高安全性

## 缺点
* 不符合开闭原则，对修改关闭，对扩展开放
我们知道外观模式将子系统封装起来，我们无法修改子系统，只能外部扩展
