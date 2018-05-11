# Servlet、GenericServlet、HttpServlet

## Servlet
对于web容易来说，所有servlet都必须有的行为，规范在servlet接口中：
```
package javax.servlet;

import java.io.IOException;

public interface Servlet {
    public void init(ServletConfig config) throws ServletException;
    public ServletConfig getServletConfig();
    public void service(ServletRequest req, ServletResponse res) 
                   throws ServletException, IOException;
    public String getServletInfo();
    public void destroy();
}
```

当我们所实例化的一个servlet被容器载入后，容器会建立该servlet的接口的实例，根据servlet设定的一些初始化和其他信息（一般在web.xml中），创建一个servletConfig（稍后会介绍servletConfig接口）的实例，然后调用servlet接口的init方法并且传入一个servletconfig实例，完成servlet的初始化。

servlet初始化完成之后，如果这时候收到了对于某个servlet的请求，那么这时候容器就会先通过urlPattern找到这个servlet，然后调用这个servlet的service方法，传入ServletRequest、ServletResponse的实例，注意这两个接口并不是http开头的，这好似因为最初设计的时候，并没有限制servlet只能用于http协议上，但实际上大到现在servlet还是只用在了http上哈哈。

当一个servlet已经没用了，容器准备回收它之前，容器会呼叫它的destroy方法，来执行一些资源的回收与释放。

## GenericServlet
GenericServlet是一个抽象类，它实现了servlet接口
```
package javax.servlet;

import java.io.IOException;
import java.util.Enumeration;
public abstract class GenericServlet 
    implements Servlet, ServletConfig, java.io.Serializable {
    private transient ServletConfig config;
    public GenericServlet() {}
    public void destroy() {}
    public String getInitParameter(String name) {
        return getServletConfig().getInitParameter(name);
    }
    public Enumeration<String> getInitParameterNames() {
        return getServletConfig().getInitParameterNames();
    }      
    public ServletConfig getServletConfig() {
        return config;
    }
    public ServletContext getServletContext() {
        return getServletConfig().getServletContext();
    }
    public String getServletInfo() {
        return "";
    }
    public void init(ServletConfig config) throws ServletException {
        this.config = config;
        this.init();
    }
    public void init() throws ServletException {}
    public void log(String msg) {
        getServletContext().log(getServletName() + ": "+ msg);
    }
    public void log(String message, Throwable t) {
        getServletContext().log(getServletName() + ": " + message, t);
    }
    public abstract void service(ServletRequest req, ServletResponse res)
                             throws ServletException, IOException;
    public String getServletName() {
        return config.getServletName();
    }
}
```

GenericServlet实现了servlet的接口，其中有一个接受servletconfig参数的init方法，会传入servletconfig的实例，之后才会调用无参数的init方法，所以** 如果我们重新定义servlet的初始化，不建议直接重定义接受参数的init方法，而是建议重新定义无参数的init方法 **
GenericServlet對Servlet介面的service()方法沒有實作，僅標示為abstract，service()方法的實作由子類別

## HttpServlet
我们看到GenericServlet对于servlet接口的service方法没有具体实现，标记为抽象方法，这是因为service方法交给了它的子类去实现，比如具体的httpservlet：
```
package javax.servlet.http;

略...

public abstract class HttpServlet extends GenericServlet {
    public HttpServlet() {}

    略...

    @Override
    public void service(ServletRequest req, ServletResponse res)
        throws ServletException, IOException {

        HttpServletRequest  request;
        HttpServletResponse response;
        
        try {
            request = (HttpServletRequest) req;
            response = (HttpServletResponse) res;
        } catch (ClassCastException e) {
            throw new ServletException("non-HTTP request or response");
        }
        service(request, response);
    }

    protected void service(HttpServletRequest req, HttpServletResponse resp)
        throws ServletException, IOException {

        String method = req.getMethod();

        if (method.equals(METHOD_GET)) {
            long lastModified = getLastModified(req);
            if (lastModified == -1) {
                doGet(req, resp);
            } else {
        略...          
    }
}
```
HttpServlet实现了javax.servlet.http接口，同时又继承实现了GenericServlet未实现的service方法。在web容器的环境下，实际上传入的请求和响应对象的是HttpServletRequest、HttpServletResponse接口。他们其实都是ServletRequest、ServletResponse的子接口，增加了一些关于http的操作。

![image.png](http://upload-images.jianshu.io/upload_images/1234352-27e1f20fc96c9d31.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# ServletConfig

在web容易启动后，会读取（Annotation）或web.xml里面的设定，根据其中对每个servlet的设定将servlet载入并且实例化，并未每个servlet的配置产生一个servletconfig的对象，随后调用servlet的接口的init方法，并且将产生的servletconfig的对象当作参数传入。

servletconfig其实就是将（Annotation）或web.xml中为servlet配置的信息整合保存到一个对象中，容器会为每个servlet产生一个servlet对象和一个servletconfig对象。genericservlet同时实现了这两个接口，servlet和servletconfig。

GenericServlet主要的目的，就是将servlet调用init方法传入的config对象封装起来：
```
private transient ServletConfig config;
public void init(ServletConfig config) throws ServletException {
    this.config = config;
    this.init();
}
public void init() throws ServletException {
}
```
GenericServlet在實作Servlet的init()方法時，也呼叫了另一個無參數的init()方法，基本上你在撰寫Servlet時，如果有一些初始時所要執行的動作，可以重新定義這個無參數的init()方法，而不是直接重新定義有ServletConfig參數的init()方法。

对于一般的对象，我们如果想在初始化之后，执行一些要执行的动作，我们一般需要重新定义构造函数，在构造函数中完成后续的动作即可。但在写servlet的时候，却不一样，如果我们想要执行一些与web资源相关的初始化操作时，我们要重新定义init方法。举个例子，如果我们想要使用servletconfig来做一些事情，我们不能在构造函数中定义，因为我们实例化servlet的时候，容器还没有执行init方法传入servletconfig对象，所以我们这时候还没有servletconfig的实例

GenericServlet也包括了Servlet與ServletConfig所定義方法的簡單實作，實作內容主要是透過ServletConfig來取得一些相關資訊，例如：
```
public ServletConfig getServletConfig() {
    return config;
}
public String getInitParameter(String name) {
    return getServletConfig().getInitParameter(name);
}
public Enumeration getInitParameterNames() {
    return getServletConfig().getInitParameterNames();
}   
public ServletContext getServletContext() {
    return getServletConfig().getServletContext();
}
```

所以当我们继承httpservlet实现一个servlet的时候，就可以直接利用这些方法取得相关的servletconfig的信息，而不用意识到servletconfig的存在。
若要使用標註設定個別Servlet的初始參數，可以在@WebServlet中使用@WebInitParam設定initParams。例如：
```
...
@WebServlet(name="ServletConfigDemo", urlPatterns={"/conf"},
            initParams={
                @WebInitParam(name = "PARAM1", value = "VALUE1"),
                @WebInitParam(name = "PARAM2", value = "VALUE2")
            }
)
public class ServletConfigDemo extends HttpServlet {
    private String PARAM1;
    private String PARAM2;
    
    public void init() throws ServletException {
        super.init();
        PARAM1 = getServletConfig().getInitParameter("PARAM1");
        PARAM2 = getServletConfig().getInitParameter("PARAM2");
    }
    ....
}
```
若要在web.xml中設定個別Servlet的初始參數，可以在<servlet>標籤之中，使用<init-param>進行設定，web.xml中的設定會覆蓋標註的設定。例如：
```
...
<servlet>
    <servlet-name>ServletConfigDemo</servlet-name>
    <servlet-class>cc.openhome.ServletConfigDemo</servlet-class>
    <init-param>
        <param-name>PARAM1</param-name>
        <param-value>VALUE1</param-value>
    </init-param>
    <init-param>
        <param-name>PARAM2</param-name>
        <param-value>VALUE2</param-value>
    </init-param>
</servlet>
...
```
 由於ServletConfig必須在Web容器將Servlet實例化後，呼叫有參數的init()方法再將之傳入，是與Web應用程式資源相關的物件，所以在繼承HttpServlet後，通常會重新定義無參數的init()方法以進行Servlet初始參數的取得。GenericServlet定義了一些方法，將ServletConfig封裝起來，便於取得設定資訊，所以取得Servlet初始參數的程式碼也可以改寫為：
```
...
@WebServlet(name="ServletConfigDemo", urlPatterns={"/conf"},
            initParams={
                @WebInitParam(name = "PARAM1", value = "VALUE1"),
                @WebInitParam(name = "PARAM2", value = "VALUE2")
            }
)
public class AddMessage extends HttpServlet {
    private String PARAM1;
    private String PARAM2;
    public void init() throws ServletException {
        super.init();
        PARAM1 = getInitParameter("PARAM1");
        PARAM2 = getInitParameter("PARAM2");
    }
    ….
}
```
下面這個範例簡單地示範如何設定、使用Servlet初始參數，其中登入成功與失敗的網頁，可以由初始參數設定來決定：
```
import java.io.*;
import javax.servlet.*;
import javax.servlet.annotation.*;
import javax.servlet.http.*;

@WebServlet(name = "LoginServlet", urlPatterns = {"/login.do"},
            initParams = {
                @WebInitParam(name = "SUCCESS", value = "success.view"),
                @WebInitParam(name = "ERROR", value = "error.view")
            }
)
public class LoginServlet extends HttpServlet {
    private String SUCCESS_VIEW;
    private String ERROR_VIEW;

    @Override
    public void init() throws ServletException {
        super.init();
        SUCCESS_VIEW = this.getInitParameter("SUCCESS");
        ERROR_VIEW = this.getInitParameter("ERROR");
    }

    protected void doPost(HttpServletRequest request,
                          HttpServletResponse response)
                            throws ServletException, IOException {
        response.setContentType("text/html;charset=UTF-8");
        String name = request.getParameter("name");
        String passwd = request.getParameter("passwd");
        if ("caterpillar".equals(name) && "123456".equals(passwd)) {
            request.getRequestDispatcher(SUCCESS_VIEW)
                    .forward(request, response);
        } else {
            request.getRequestDispatcher(ERROR_VIEW)
                    .forward(request, response);
        }
    }
}
```
# ServletContext
ServletContext接口定义了Servlet所运行程序的一个总体的行为与信息，你可以使用ServletContext实例对象來取得所请求资源的URL、应用程序的初始化參數，甚至动态的设定Servlet实例。

ServletContext本身的名字让人产生误解，因为它以Servlet作为开头，容易被误认为只是一个单独的Servlet的代表对象。但实际上，当整个Web应用程序加载进入Web容器之后，容器会生成一个ServletContext对象作为整个应用程序的代表，并设定给ServletConfig，你只要通过调用ServletConfig的getServletContext()方法就可以取得ServletContext对象。
以下我们先介绍几个容易搞错，需要注意的方法：
* getRequestDispatcher()方法可以取得RequestDispatcher实例对象，使用时路徑的指定必须以"/"作为开头，这个斜杠代表应用程序的根目录（Context Root）。
```
context.getRequestDispatcher("/pages/some.jsp").forward(request, response);
```
以"/"作为开头有时候也可以理解为程序环境的初始化路径（Context-relative），沒有以"/"作为开头的（Request-relative）路径则成为（Request-relative）请求相对路径。事实上，HttpServletRequest 的getRequestDispatcher()方法在实例化的时候，若是环境相对路径，就直接交给ServletContext的 getRequestDispatcher()；若是请求相对路径，则会先转换为环境相对路径再转发给ServletContext的 getRequestDispatcher()來取得RequestDispatcher。

* 如果想要知道Web程序中的某个目录中有哪些档案，则可以使用getResourcePaths()方法。例如：
for(String avatar : getServletContext().getResourcePaths("/")) {
    // 顯示 avatar 文字...
}

使用時指定路徑必須以"/"作為開頭，表示相對於應用程式環境根目錄，傳回的路徑會像是：
```
/welcome.html
/catalog/
/catalog/index.html
/catalog/products.html
/customer/
/customer/login.jsp
/WEB-INF/
/WEB-INF/web.xml
/WEB-INF/classes/com.acme.OrderServlet.class
```

可以看到，这个方法会将WEB-INF的目录信息文件信息也列出来。如果是目录信息，会以“/”作为结尾。

如果想在Web程序中读取程序目录中某个文档的内容，则可以使用getResourceAsStream()方法，使用时指定路径以"/"作为开头，表示相对于应用程序的环境根目录，或者相对是/WEB-INF/lib中JAR档案里META-INF/resources的路徑（Web程序中，JAR檔案必須放在/WEB-INF/lib中，這是規定），执行结果会返回一个InputStream对象，接下来我们就可以用这个对象来读取文档的内容。底下是個讀取PDF並傳送給客戶端的範例：
```
import java.io.*;
import javax.servlet.*;
import javax.servlet.annotation.*;
import javax.servlet.http.*;

@WebServlet(name="Ebook", urlPatterns={"/ebook.view"},
            initParams={
                @WebInitParam(name="PDF_FILE", value="/WEB-INF/jdbc.pdf")
            }
)
public class Ebook extends HttpServlet {
    private String PDF_FILE;

    @Override
    public void init() throws ServletException {
        super.init();
        PDF_FILE = getInitParameter("PDF_FILE");
    }
    
    protected void doGet(HttpServletRequest request,
                          HttpServletResponse response)
                      throws ServletException, IOException {
        String passwd = request.getParameter("passwd");
        if("123456".equals(passwd)) {
            response.setContentType("application/pdf");
            InputStream in = this.getServletContext()
                                 .getResourceAsStream(PDF_FILE);
            OutputStream out = response.getOutputStream();
            byte[] buffer = new byte[1024];
            int length = -1;
            while((length = in.read(buffer)) != -1) {
                out.write(buffer, 0, length);
            }
            in.close();
            out.close();
        }
    }
}
```
每个Web程序都会有一个相对应的ServletContext，针对「应用程序」初始化时所需用到的一些参数的信息，你可以在web.xml中摄制应用程序的初始化参数，設定時使用<context-param>标签来定义。例如：
```
<web-app ...>
    <context-param>
       <param-name>MESSAGE</param-name>
       <param-value>/WEB-INF/messages.txt</param-value>
    </context-param>
    …
</web-app>
```

在程序中可以通过调用ServletContext的getInitParameter()方法來读取初始化参数。因此Web應用程式初始參數常被稱為ServletContext初始參數。

 在整個Web程序的生命周期中，Servlet所需共用的信息参数，則可以设定为ServletContext属性。由于ServletContext在Web应用程序存活的时候会一直存在，所以設定為ServletContext属性的信息，除非你主动移除，否则也是一直存在web程序中的。

可以透過ServletContext的setAttribute()方法設定物件為ServletContext屬性，之後可透過ServletContext的getAttribute()方法取出該屬性。若要移除屬性，則透過ServletContext的removeAttribute()方法。

注意！ServletContext不是线程安全的，所以必要时我们还需要注意存取变量属性时的线程安全问题。
