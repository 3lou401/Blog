面向对象编程中，类用来表示对象，一般情况下，我们需要考虑用类来表示什么具体的东西。类对应的东西可能存在于真实世界中，也可能不存在于真实世界中。
状态模式所表示的类，一般就不存在真实世界的某个东西，因为状态模式中的类是用来表示状态的。状态一般都是抽象的，所以往往没有具体对应于真实世界的对象。
我们用类来表示状态，那么不同的状态就用不同的类来表示，我们只要通过切换不同的类就可以切换不同的状态。

# 状态模式的具体实例

我们考虑设计一个金库警报系统，这个系统会根据白天晚上做出不同的响应。

有一个金库
金库与警报中心相连
金库里有警铃和电话
金库里有时钟

金库只能在白天使用
白天使用金库，会在警报中心留下记录
晚上使用金库，会向警报中心发送紧急事态通知

警铃白天晚上都能用
使用警铃，会向警报中心发送紧急事态通知

电话都可以使用
白天使用电话，会呼叫警报中心
晚上使用电话，会呼叫警报中心的留言电话

基本就是以上的需求逻辑。

如果我们不使用状态模式
那就是大概伪码如下：
```
使用金库调用的方法（） ｛
  if(白天) {
} else if(晚上) {
}
｝

正常通话时（） ｛
if（白天） ｛｝
else if(晚上) ｛
｝
｝
```

显然这样可以实现，也并没有什么错误。

但是状态模式确实从不同的角度来考虑问题。

状态模式会发现，这些不同的行为，主要依赖于两个状态，就是白天和晚上。所以状态模式会抽象出这两种状态，每个状态就会有自己的行为实现，比如白天这个状态会实现自己的使用金库的方法，通话的方法，晚上的类也会实现自己的行为逻辑，最后我们只要取得状态对象的委托调用他们的方法就行了，不管他们具体是怎么实现的。
我们看一下使用状态模式的伪码：
```
白天的状态类 {
      使用金库的方法
      使用警铃的方法
     通话的方法
}


晚上的状态类 {
      使用金库的方法
      使用警铃的方法
     通话的方法
}
```

我们看到普通方法和状态模式的区别就是状态模式中，定义了状态类，就不需要if语句来判断了。所以当我们遇到很多个ifelse语句的时候，往往可以考虑状态模式，阿里最新的java开发手册里面就有一条相关的推荐


![image.png](http://upload-images.jianshu.io/upload_images/1234352-3de163a345234648.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

下面我们就来具体实现代码

定义状态类的接口：
```
package State;

public interface State {
    public abstract void doClock(Context context, int hour);    // 设置时间
    public abstract void doUse(Context context);                // 使用金库
    public abstract void doAlarm(Context context);              // 按下警铃
    public abstract void doPhone(Context context);              // 正常通话
}
```

实现两个具体的白天和晚上的状态类
```
package State;

public class DayState implements State {
	private static DayState singleton = new DayState();
    private DayState() {                                // 构造函数的可见性是private
    }
    public static State getInstance() {                 // 获取唯一实例
        return singleton;
    }
    public void doClock(Context context, int hour) {    // 设置时间
        if (hour < 9 || 17 <= hour) {
            context.changeState(NightState.getInstance());
        }
    }
    public void doUse(Context context) {                // 使用金库
        context.recordLog("使用金库(白天)");
    }
    public void doAlarm(Context context) {              // 按下警铃
        context.callSecurityCenter("按下警铃(白天)");
    }
    public void doPhone(Context context) {              // 正常通话
        context.callSecurityCenter("正常通话(白天)");
    }
    public String toString() {                          // 显示表示类的文字
        return "[白天]";
    }
}

```

```
package State;

public class NightState implements State {
	private static NightState singleton = new NightState();
    private NightState() {                              // 构造函数的可见性是private
    }
    public static State getInstance() {                 // 获取唯一实例
        return singleton;
    }
    public void doClock(Context context, int hour) {    // 设置时间
        if (9 <= hour && hour < 17) {
            context.changeState(DayState.getInstance());
        }
    }
    public void doUse(Context context) {                // 使用金库
        context.callSecurityCenter("紧急：晚上使用金库！");
    }
    public void doAlarm(Context context) {              // 按下警铃
        context.callSecurityCenter("按下警铃(晚上)");
    }
    public void doPhone(Context context) {              // 正常通话
        context.recordLog("晚上的通话录音");
    }
    public String toString() {                          // 显示表示类的文字
        return "[晚上]";
    }
}

```

Context类是负责管理状态和联系警报中心的接口，它定义了基本的警报中心的行为，
```
package State;

public interface Context {

    public abstract void setClock(int hour);                // 设置时间
    public abstract void changeState(State state);          // 改变状态
    public abstract void callSecurityCenter(String msg);    // 联系警报中心
    public abstract void recordLog(String msg);             // 在警报中心留下记录
}

```

safeframe类实现了context接口
```
package State;

import java.awt.Frame;
import java.awt.Label;
import java.awt.Color;
import java.awt.Button;
import java.awt.TextField;
import java.awt.TextArea;
import java.awt.Panel;
import java.awt.BorderLayout;
import java.awt.event.ActionListener;
import java.awt.event.ActionEvent;

public class SafeFrame extends Frame implements ActionListener, Context {
    private TextField textClock = new TextField(60);        // 显示当前时间
    private TextArea textScreen = new TextArea(10, 60);     // 显示警报中心的记录
    private Button buttonUse = new Button("使用金库");      // 金库使用按钮
    private Button buttonAlarm = new Button("按下警铃");    // 按下警铃按钮
    private Button buttonPhone = new Button("正常通话");    // 正常通话按钮
    private Button buttonExit = new Button("结束");         // 结束按钮

    private State state = DayState.getInstance();           // 当前的状态

    // 构造函数
    public SafeFrame(String title) {
        super(title);
        setBackground(Color.lightGray);
        setLayout(new BorderLayout());
        //  配置textClock
        add(textClock, BorderLayout.NORTH);
        textClock.setEditable(false);
        // 配置textScreen
        add(textScreen, BorderLayout.CENTER);
        textScreen.setEditable(false);
        // 为界面添加按钮
        Panel panel = new Panel();
        panel.add(buttonUse);
        panel.add(buttonAlarm);
        panel.add(buttonPhone);
        panel.add(buttonExit);
        // 配置界面
        add(panel, BorderLayout.SOUTH);
        // 显示
        pack();
        show();
        // 设置监听器
        buttonUse.addActionListener(this);
        buttonAlarm.addActionListener(this);
        buttonPhone.addActionListener(this);
        buttonExit.addActionListener(this);
    }
    // 按钮被按下后该方法会被调用
    public void actionPerformed(ActionEvent e) {
        System.out.println(e.toString());
        if (e.getSource() == buttonUse) {           // 金库使用按钮
            state.doUse(this);
        } else if (e.getSource() == buttonAlarm) {  // 按下警铃按钮
            state.doAlarm(this);
        } else if (e.getSource() == buttonPhone) {  // 正常通话按钮
            state.doPhone(this);
        } else if (e.getSource() == buttonExit) {   // 结束按钮
            System.exit(0);
        } else {
            System.out.println("?");
        }
    }
    // 设置时间
    public void setClock(int hour) {
        String clockstring = "现在时间是";
        if (hour < 10) {
            clockstring += "0" + hour + ":00";
        } else {
            clockstring += hour + ":00";
        }
        System.out.println(clockstring);
        textClock.setText(clockstring);
        state.doClock(this, hour);
    }
    // 改变状态
    public void changeState(State state) {
        System.out.println("从" + this.state + "状態变为了" + state + "状态。");
        this.state = state;
    }
    // 联系警报中心
    public void callSecurityCenter(String msg) {
        textScreen.append("call! " + msg + "\n");
    }
    // 在警报中心留下记录
    public void recordLog(String msg) {
        textScreen.append("record ... " + msg + "\n");
    }
}
```
main类启动
```
package State;

public class Main {
	public static void main(String[] args) {
        SafeFrame frame = new SafeFrame("State Sample");
        while (true) {
            for (int hour = 0; hour < 24; hour++) {
                frame.setClock(hour);   // 设置时间
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                }
            }
        }
    }
}

```


![image.png](http://upload-images.jianshu.io/upload_images/1234352-add11163ce0fb567.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


# 状态模式的分析

状态模式的角色：
* state状态
表示状态，定义了根据不同状态进行不同处理的接口，该接口是那些处理内容依赖于状态的方法集合，对应实例的state类

* 具体的状态
实现了state接口，对应daystate和nightstate

* context
context持有当前状态的具体状态的实例，此外，他还定义了供外部调用者使用的状态模式的接口。

状态模式的类图：

![image.png](http://upload-images.jianshu.io/upload_images/1234352-270f1ebebe466b80.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
