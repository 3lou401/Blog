> * 引入备忘录模式
* 备忘录模式的实例
* 备忘录模式的分析


# 引入备忘录模式
我们在使用文本编辑器的时候，一般如果不小心误操作了，按ctrl+z就可以恢复之前的状态，撤销（undo）操作。
撤销的操作，实际上有两步，一是要保存之前的状态，然后恢复保存的状态。
面向对象中，如果要实现相关功能，首先就要保存相关实例的信息，恢复的时候，根据状态信息在恢复。
想要恢复实例，就需要一个可以自由访问实例内部结构的权限。但是，如果稍不注意，就会将依赖于实力内部结构的代码，分散在程序中的各个地方，破坏了封装性。
所以，备忘录模式，就引入一个专门表示实例状态的角色，可以在保存和恢复实例的时候有效的防止对象的封装性遭到破坏。

备忘录模式主要可以实现一下几个功能：
* undo撤销
* redo重做
* history 历史记录
* snapshot快照

备忘录模式就像在某一个时刻给一个对象实例拍个照片，然后将以后有必要的时候，就可以将实例恢复到当时的状态。

# 备忘录模式的实例
我们实现一个实例，可以保存实例某个时间点的状态，并且恢复。

![](http://upload-images.jianshu.io/upload_images/1234352-0f2fc326eeca2d81.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
package Memento;

public class Life {
	
	private String time;
	
	public void set(String time) {
        System.out.println("Setting time to " + time);
        this.time = time;
    }
	
	public Memento saveToMemento() {
        System.out.println("Saving time to Memento");
        return new Memento(time);
    }
	
	public void restoreFromMemento(Memento memento) {
    	time = memento.getSavedTime();
        System.out.println("Time restored from Memento: " + time);
    }
}
```

```
package Memento;

import java.util.ArrayList;
import java.util.List;

public class Memento {
	private final String time;
	 
    public Memento(String timeToSave) {
    	time = timeToSave;
    }

    public String getSavedTime() {
        return time;
    }
}

```

```
package Memento;

import java.util.ArrayList;
import java.util.List;

public class You {

	public static void main(String[] args) {
		List<Memento> savedTimes = new ArrayList<>();
		 
        Life life = new Life();
 
        //time travel and record the eras
        life.set("2000 B.C.");
        savedTimes.add(life.saveToMemento());
        life.set("2000 A.D.");
        savedTimes.add(life.saveToMemento());
        life.set("3000 A.D.");
        savedTimes.add(life.saveToMemento());
        life.set("4000 A.D.");
        
        life.restoreFromMemento(savedTimes.get(0));

	}

}
```
运行结果

![image.png](http://upload-images.jianshu.io/upload_images/1234352-924b3bfd65f41084.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们可以根据需要恢复任意时刻的状态。

# 备忘录模式分析
备忘录模式的角色：
* Originator生成者
生成者会在保存自己状态的时候，new一个新的menmeto角色
当需要恢复的时候，只需要把以前的menmeto传给生成者，他就会将自己恢复至menmeto的状态。在示例中，由life类扮演这个角色

* 纪念品Menmeto
纪念品角色会将生成者的内部信息整合在一起，在menmeto中虽然保存了生成者的信息，但他不会向外部公开这些信息。纪念品主要有以下几种接口
宽接口：
就是暴露了全部自己的相关信息，使用宽接口的话，menmeto内部信息就会被暴露
窄接口：
窄接口一般只会暴露自己的部分信息

menmeto一般为了防止信息的暴露，会使用窄接口。

* 负责人
就是main，负责何时创建新实例合适保存menmeto，何时恢复memento。

备忘录模式的类图

![image.png](http://upload-images.jianshu.io/upload_images/1234352-6f3754f2b5e23d77.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
