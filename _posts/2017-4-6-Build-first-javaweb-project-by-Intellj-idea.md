---
layout:     post
title:      Build first javaweb project by Intellj idea
category: blog
description: Build first javaweb project by Intellj idea
---

#[Download the Intellj idea](http://www.jetbrains.com/idea/)

  Clicking the title and jumping to the Official Website of Intellj idea . In this site , you can download the dmg file for Intellj idea . At this step you will find this software is not free . Don't worry , find poll code at [here](http://idea.lanyus.com).
  
#[Prepare the server enviroument]({{site.url}}/established-javaweb-development-enviroument)

  Click the title , do as my previous blog .
  
#Build a javaweb project

  Start your Intellj idea , you will see a interface like the figure downstairs.

![]({{site.url}}/assets/intelljStartFace.png)

  Select the "Plugins" , just like next figure .
  
![]({{site.url}}/assets/IntelljPlugins.png)

  Find the choice about Tomcat server , select it and apply .
  
![]({{site.url}}/assets/IntelljPluginsTomcat.png)

#Creat a new javaweb project

  Creat two directoys , "lib" and "classes" . 
 
   The next is a part from Official doc .
   
> /WEB-INF/classes/ - This directory contains any Java class files (and associated resources) required for your application, including both servlet and non-servlet classes, that are not combined into JAR files. If your classes are organized into Java packages, you must reflect this in the directory hierarchy under /WEB-INF/classes/. For example, a Java class named com.mycompany.mypackage.MyServlet would need to be stored in a file named /WEB-INF/classes/com/mycompany/mypackage/MyServlet.class. 

> /WEB-INF/lib/ - This directory contains JAR files that contain Java class files (and associated resources) required for your application, such as third party class libraries or JDBC drivers.

  So after build this directories , we should change the setting .
  
  First Step : Open Module Settings . 
  
![]({{site.url}}/assets/OpenModuleSetting.png)

  Second Step : Set the out path to "classes" directory.
 
![]({{site.url}}/assets/outPath.png)

  Third Step : Set the lib path to "lib" directory.
  
![]({{site.url}}/assets/libPath.png)
  
  Fourth Step : Put the jar file we need to lib . You can find it in the Tomcat doc which you download when you build tomcat server.
  
![]({{site.url}}/assets/jarfile.png)

# Run hello.java

  Build a class under src named "HelloWorld". Put the next code at this class.
```
  import javax.servlet.annotation.WebServlet;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;
@WebServlet("/HelloWorld")
public class HelloWorld extends HttpServlet {
    private String message;

    @Override
    public void init() throws ServletException {
        message = "Hello world, this message is from servlet!";
    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //设置响应内容类型
        resp.setContentType("text/html");

        //设置逻辑实现
        PrintWriter out = resp.getWriter();
        out.println("<h3>" + message + "</h3>");
    }

    @Override
    public void destroy() {
        super.destroy();
    }
}
```

  Then set the "Run Configurations". Just the pictures downstairs.
  
  ![]({{site.url}}/assets/SetRunConfigurations1.png)
  ![]({{site.url}}/assets/SetRunConfigurations5.png)
  ![]({{site.url}}/assets/SetRunConfigurations2.png)
  ![]({{site.url}}/assets/SetRunConfigurations3.png)
  ![]({{site.url}}/assets/SetRunConfigurations4.png)
  
  then run it . And Click [here](http://localhost:8080/HelloWorld)
  

 
  
