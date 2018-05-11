> * 引入中介者模式
* 中介者模式实例
* 中介者模式分析

# 引入中介者模式
大家想象一下有十个人要共同完成一个工作，他们要互相合作和沟通，并且根据对方的通知可能要改变自己的状态，但这通常会带来很多问题，流程过于复杂，使得每个人不仅要专注于自己的事情，还要与他人进行沟通，得到通知，需要兼顾很多状态的变化。这时候，我们考虑可以引入一个类似上帝视角的角色，就是引入一个中介者，他来负责接受每个人的通知，并将变化发送所需要的人去，就是要他来控制并调节工作的进度和细节，这个人往往是从整体考虑的，所以使得每个人工作者只需要考虑自己的问题，一旦有了变化，就通知仲裁者，交给仲裁者去决定就可以了。
所以最后就变成了，整个团队的交流过程，组员向中介者报告，中介者向组员下达只命令。
这在现实生活中也是常见的，每个部门通常都会有一个领导人，每个班级有一个班长，往往是班长负责接受同学们的信息，然后将上面的信息从班长这里发给同学们，这里的班长就相当于一个仲裁者，同学们就相当于组员。

这就是中介者模式的基本思想，mediator有仲裁者和中介者的意思，一方面，当麻烦事情发生的时候，通知仲裁者，当发生设计全体组员的事情时，也通知仲裁者。当仲裁者下达指示的时候，组员会立即执行。团队组员之间不再互相沟通并私自做决定，而是发生任何事情都像仲裁者报告。另一方面，仲裁者在整个团队的角度上对组员报告的事情进行决定。

# 中介者模式的实例

我们通过一个gui程序来实现一个简单的仲裁者模式

![image.png](http://upload-images.jianshu.io/upload_images/1234352-0cdda0682fc2fab7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

看上图是一个我们经常见的登录框。通常登录框就设计很多状态的变化，比如我们这个例子就要实现如下的逻辑

首先，如果选择用户登录，那么用户名输入框处于启用状态，但密码输入框处于禁用状态，如下图


![image.png](http://upload-images.jianshu.io/upload_images/1234352-f285a2be6deefeb1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


如果用户输入了用户名，密码框就启用，如下图


![image.png](http://upload-images.jianshu.io/upload_images/1234352-079a71f9f765ae40.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


如果又删掉了用户名，那么密码又变回禁用状态

![image.png](http://upload-images.jianshu.io/upload_images/1234352-6730a4816b3f83be.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果选择游客登录，就都处于禁用状态。

![image.png](http://upload-images.jianshu.io/upload_images/1234352-1968455709959a23.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

假设我们有这样的需求，那么我们应该如何编写程序呢？
最先的想法就是，每个控件分别分别接受相关控件的状态，根据不同的状态在改变自己的状态。这样做是可以实现功能的，但是假设这时候我们新加一个控件进来，那么由于这个逻辑分散在各个控件的代码中，所以我们修改起来非常麻烦，而且每个控件都充斥着状态控制的代码，很臃肿，控件应该专注于实现自己的逻辑。
因为这些控件都相互关联，相互制约的，全部耦合在一起。
根据面向对象的设计原则，我们应该尽量对象过度耦合，实现松耦合的状态。

于是，我们就可以利用中介者模式，每个控件发生了变化，我们就把变化发给中介者，中介者统一来处理，这样控件就只需要专注于自己的实现就行了。

首先看类图：

![image.png](http://upload-images.jianshu.io/upload_images/1234352-f898397cea3382dd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

首先，定义Mediator接口
```
public interface Mediator {
    public abstract void createColleagues();
    public abstract void colleagueChanged();
}

```
定义Colleague接口
```
public interface Colleague {
    public abstract void setMediator(Mediator mediator);
    public abstract void setColleagueEnabled(boolean enabled);
}

```
实现三个colleague类：
```
import java.awt.Button;

public class ColleagueButton extends Button implements Colleague {
    private Mediator mediator;
    public ColleagueButton(String caption) {
        super(caption);
    }
    public void setMediator(Mediator mediator) {            // 保存Mediator
        this.mediator = mediator;
    }
    public void setColleagueEnabled(boolean enabled) {      // Mediator下达启用/禁用的指示 
        setEnabled(enabled);
    }
}

```

```
import java.awt.Checkbox;
import java.awt.CheckboxGroup;
import java.awt.event.ItemListener;
import java.awt.event.ItemEvent;

public class ColleagueCheckbox extends Checkbox implements ItemListener, Colleague {
    private Mediator mediator;
    public ColleagueCheckbox(String caption, CheckboxGroup group, boolean state) {  // 构造函数 
        super(caption, group, state);
    }
    public void setMediator(Mediator mediator) {            // 保存Mediator
        this.mediator = mediator;
    }
    public void setColleagueEnabled(boolean enabled) {      // Mediator下达启用/禁用指示
        setEnabled(enabled);
    }
    public void itemStateChanged(ItemEvent e) {             // 当状态发生变化时通知Mediator
        mediator.colleagueChanged();
    }
}

```

```
import java.awt.TextField;
import java.awt.Color;
import java.awt.event.TextListener;
import java.awt.event.TextEvent;

public class ColleagueTextField extends TextField implements TextListener, Colleague {
    private Mediator mediator;
    public ColleagueTextField(String text, int columns) {   // 构造函数
        super(text, columns);
    }
    public void setMediator(Mediator mediator) {            // 保存Mediator
        this.mediator = mediator;
    }
    public void setColleagueEnabled(boolean enabled) {      // Mediator下达启用/禁用的指示
        setEnabled(enabled);
        setBackground(enabled ? Color.white : Color.lightGray);
    }
    public void textValueChanged(TextEvent e) {             // 当文字发生变化时通知Mediator
        mediator.colleagueChanged();
    }
}

```
最后实现具体的中介者对象
```
import java.awt.Frame;
import java.awt.Label;
import java.awt.Color;
import java.awt.CheckboxGroup;
import java.awt.GridLayout;
import java.awt.event.ActionListener;
import java.awt.event.ActionEvent;

public class LoginFrame extends Frame implements ActionListener, Mediator {
    private ColleagueCheckbox checkGuest;
    private ColleagueCheckbox checkLogin;
    private ColleagueTextField textUser;
    private ColleagueTextField textPass;
    private ColleagueButton buttonOk;
    private ColleagueButton buttonCancel;

    // 构造函数。
    // 生成并配置各个Colleague后，显示对话框。
    public LoginFrame(String title) {
        super(title);
        setBackground(Color.lightGray);
        // 使用布局管理器生成4×2窗格
        setLayout(new GridLayout(4, 2));
        // 生成各个Colleague
        createColleagues();
        // 配置
        add(checkGuest);
        add(checkLogin);
        add(new Label("Username:"));
        add(textUser);
        add(new Label("Password:"));
        add(textPass);
        add(buttonOk);
        add(buttonCancel);
        // 设置初始的启用起用/禁用状态
        colleagueChanged();
        // 显示
        pack();
        show();
    }

    // 生成各个Colleague。
    public void createColleagues() {
        // 生成
        CheckboxGroup g = new CheckboxGroup();
        checkGuest = new ColleagueCheckbox("Guest", g, true);
        checkLogin = new ColleagueCheckbox("Login", g, false);
        textUser = new ColleagueTextField("", 10);
        textPass = new ColleagueTextField("", 10);
        textPass.setEchoChar('*');
        buttonOk = new ColleagueButton("OK");
        buttonCancel = new ColleagueButton("Cancel");
        // 设置Mediator
        checkGuest.setMediator(this);
        checkLogin.setMediator(this);
        textUser.setMediator(this);
        textPass.setMediator(this);
        buttonOk.setMediator(this);
        buttonCancel.setMediator(this);
        // 设置Listener
        checkGuest.addItemListener(checkGuest);
        checkLogin.addItemListener(checkLogin);
        textUser.addTextListener(textUser);
        textPass.addTextListener(textPass);
        buttonOk.addActionListener(this);
        buttonCancel.addActionListener(this);
    }

    // 接收来自于Colleage的通知然后判断各Colleage的启用/禁用状态。
    public void colleagueChanged() {
        if (checkGuest.getState()) { // Guest mode
            textUser.setColleagueEnabled(false);
            textPass.setColleagueEnabled(false);
            buttonOk.setColleagueEnabled(true);
        } else { // Login mode
            textUser.setColleagueEnabled(true);
            userpassChanged();
        }
    }
    // 当textUser或是textPass文本输入框中的文字发生变化时
    // 判断各Colleage的启用/禁用状态
    private void userpassChanged() {
        if (textUser.getText().length() > 0) {
            textPass.setColleagueEnabled(true);
            if (textPass.getText().length() > 0) {
                buttonOk.setColleagueEnabled(true);
            } else {
                buttonOk.setColleagueEnabled(false);
            }
        } else {
            textPass.setColleagueEnabled(false);
            buttonOk.setColleagueEnabled(false);
        }
    }
    public void actionPerformed(ActionEvent e) {
        System.out.println(e.toString());
        System.exit(0);
    }
}
```
Main类
```
import java.awt.*;
import java.awt.event.*;

public class Main {
    static public void main(String args[]) {
        new LoginFrame("Mediator Sample");
    }
}
```
运行结果

![image.png](http://upload-images.jianshu.io/upload_images/1234352-af1f5bfc8720ef80.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


# 中介者模式分析
中介者模式主要有几个角色
* 中介者
就是负责定义控制逻辑，接受来自组员的消息并处理的接口，对应实例中的Mediator接口
* 具体的中介者
实现接口，并根据不同的需求，做出不同的逻辑
* 同事组员Colleague
组员的接口，定义相应的方法
* 具体的组员
负责实现具体的组员逻辑，并将通知直接交给中介者执行

中介者模式的类图：

![image.png](http://upload-images.jianshu.io/upload_images/1234352-4ce1e6dea9bf8248.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
