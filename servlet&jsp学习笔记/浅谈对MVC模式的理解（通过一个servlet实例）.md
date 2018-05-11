通过将一个servlet程序进行改版，加入MVC的设计思想，浅谈对MVC模式的理解与运用
***
# 创建一个简单的啤酒顾问servlet程序

## 版本一的servlet啤酒顾问程序
新建表单页面HTML：
```
<html>
  <head>
    <title>forml.html</title>
  </head>
  <body>
  <h1 align="center">Beer Selection Page</h1>
    <form name="f1" id="f1" action="SelectBeer.do" method="post">
		Select beer characteristics<p>
		Color:
		<select name="color" size="1">
			<option value="light">light</option>
			<option value="amber">amber</option>
			<option value="brown">brown</option>
			<option value="dark">dark</option>
		</select>
		<br><br>
		<center>
			<input type="submit">
		</center>
    </form>
  </body>
</html>
```
建立servlet控制器：
```
package com.example.web;

import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
public class BeerSelection extends HttpServlet {
	@Override
	protected void doPost(HttpServletRequest req, HttpServletResponse resp)
			throws ServletException, IOException {
		// TODO Auto-generated method stub
		resp.setContentType("text/html");
		PrintWriter out = resp.getWriter();
		out.println("Beer selection advice<br>");
		String color=req.getParameter("color");
		out.println("<br>Got beer color " + color);
	}
}
```
第一版的测试结果如下：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-9f9d6fb1cab5a074.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击提交后，提交给servlet进行处理，并显示处理结果：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-1c3be4f2c6cf14d2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##版本二的啤酒顾问程序
在MVC中，模型是指应用的“后台”。大多数情况下，这只是普通的java代码，其根本不知道会被servlet调用，不能把模型限制为某个web应用的工具，这就是MVC中复用的思想。

### 模型规范
建立模型的包，包名命名为model。
实现构建模型的代码并测试模型。在MVC中，MVC三部分是互相独立的，所以，模型的测试需要达到无需启用tomcat就能测试的独立性，也就是应该是简单的java类。
```
public class BeerExpert {
	public List<String> getBrands(String color) {
		List<String> brands = new ArrayList();
		if(color.equals("amber")) {
			brands.add("jack amber");
			brands.add("red moose");
		}
		else {
			brands.add("jail pale ale");
			brands.add("gout stout");
		}
		return(brands);
	}
}
```
实现一个BeerExpert类，作为后台的模型，专注于根据color得出啤酒建议。

修改之前的BeerSelection：
```
protected void doPost(HttpServletRequest req, HttpServletResponse resp)
			throws ServletException, IOException {
		// TODO Auto-generated method stub
		String color=req.getParameter("color");
		BeerExpert be = new BeerExpert();
		List<String> result = be.getBrands(color);`

		resp.setContentType("text/html");
		PrintWriter out = resp.getWriter();
		out.println("Beer selection advice<br>");
		
		java.util.Iterator<String> it = result.iterator();
		while(it.hasNext()) {
			out.print("<br>try: " + it.next() );
		}
	}
```

##回顾前两版的啤酒顾问程序
我们实现的是：
* 浏览器把请求数据发送给容器。
* 容器根据URL找到对应的servlet，并把请求传递给这个servlet
* servlet调用BeerExpert寻求帮助
* servlet输出响应
* 容器把响应输出给用户
到此为止，我们分离了model但并没有分离view，所以至此MVC实现的是不完整的。进一步完整的MVC的应该如下：
***
* 浏览器把请求数据发送给容器。
* 容器根据URL找到对应的servlet，并把请求传递给这个servlet
* servlet调用BeerExpert寻求帮助
* ** 这个专家类返回一个回答，servlet把这个回答增加到请求对象。**
* ** servlet把请求类转发给jsp**
* ** jsp从请求对象中得到回答**
* ** jsp为容器生成一个页面**
* 容器将页面返回
这才是完整MVC模式的实现。

##版本三实现MVC完整的啤酒顾问程序

servlet此时只需要完成转发的控制，是作为一个controller 的角色存在，与model（Beerexpert）和view（jsp）是完全分离的，这样就实现了完整的MVC模式。

```
String color=req.getParameter("color");
		BeerExpert be = new BeerExpert();
		List<String> result = be.getBrands(color);
		req.setAttribute("styles", result);
		RequestDispatcher view = req.getRequestDispatcher("result.jsp");
		view.forward(req, resp);
```
```
<body>
    <h1 align="center">Beer Recommendations JSP</h1>
    <p>
    <%
    	List styles = (List)request.getAttribute("styles");
    Iterator it = styles.iterator();
    while(it.hasNext())
    	out.print("<br>try: " + it.next());
    %>
  </body>
```
# 总结
至此第三版的啤酒顾问程序就实现了一个简单的MVC框架的web应用。
servlet专注于controller的角色负责利用model得到数据并转发给view，view专注于界面的显示，model专注于业务逻辑的实现。
