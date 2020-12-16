```java
//P1完事 ----2020年7月22日23:10:04
```

- #### Servlet

  - Servlet本身是一组接口
  - 在javax.servlet包里
  - 自定义一个类并且实现Servlet接口，这个类就具备了接收客户端请求以及作出响应的功能

- #### javax是java的扩展

- #### 浏览器没有权限直接访问WEB-INF文件夹里的内容，需要访问的话必须做映射

- #### 在web.xml中做映射

- #### 解决响应中文乱码问题

  - 在响应代码之前添加
  - servletResponse.setContentType("text/html;charset=UTF-8");

- #### 如果指定的参数没有传，则serlvetRequest.getParameter("参数名")的结果为null

```java
//P2完事 ----2020年7月23日23:22:16
```

- #### 两种配置servlet的方法

  - web.xml

    - ```xml
      <servlet>
          <servlet-name>firstservlet</servlet-name>
          <servlet-class>first.FirstServlet</servlet-class>
      </servlet>
      <servlet-mapping>
          <servlet-name>firstservlet</servlet-name>
          <url-pattern>/first</url-pattern>
      </servlet-mapping>
      ```

      

  - 注解

    - 在自定义servlet类声明上添加

      ```java
      @WebServlet("/path")
      ```

- #### servlet对象只在第一次被访问时init一次

```java
//P4 0:00 ----2020年7月24日20:31:47
```

- #### ServletConfig

  - 该接口是用来描述Servlet的基本信息的

  - getServletName()返回该Servlet的全类名

  - getInitParameter(String keyName);

    - 在使用web.xml配置servlet的同时可以配置参数，并使用此方法获取参数值

    - ```xml
      <servlet>
          <servlet-name>FirstServlet</servlet-name>
          <servlet-class>first.FirstServlet</servlet-class>
          <init-param>
              <param-name>name</param-name>
              <param-value>yjx</param-value>
          </init-param>
          <init-param>
              <param-name>psd</param-name>
              <param-value>123456</param-value>
          </init-param>
      </servlet>
      <servlet-mapping>
          <servlet-name>FirstServlet</servlet-name>
          <url-pattern>/first</url-pattern>
      </servlet-mapping>
      ```

  - getInitParametersNames();获取所有配置参数名
  
  - getServletContext();返回servletContext对象，它是servlet上下文，即servlet管理者
  
    - servletContext.getContextPath()取的是当前应用的全局虚拟路径
    - servletContext.getServerInfo();获取服务器信息
  
- #### 配置作用于web应用全局的参数

  - 在web.xml中添加标签

  - 要写在web-app标签下一层

    - ```java
      <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee 		http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
                  version="4.0">
          <context-param>
              <param-name>key</param-name>
              <param-value>value</param-value>
          </context-param>
      </web-app>
      ```

  - 在servlet.init()中获取：

    - ```java
      String value = servletConfig.getServletContext().getInitParameter("keyName");
      ```

```text
//P5 0:00 ----2020年7月24日22:50:06
```



- #### Servlet的层次结构

  - Servlet
    - (interface)
  - GenericServlet
    - (class)
  - HttpServlet
    - (class)

- #### http请求常用种类

  - GET
  - POST
  - PUT
  - DELETE

- #### JSP

  - jsp本质上就是一个servlet
  - 主要负责与用户进行交互
  - 将最终的界面提供给用户
  - 先转换成java文件，再转换成class文件

```java
//P7 0:00 ----2020年7月25日20:16:00
```

- #### JSP九个内置对象

  - request
    - 表示一次请求
    - HttpServletRequest实例
  - response
    - 表示一次响应
    - HttpServletResponse实例
  - pageContext
    - 页面上下文
    - 获取页面信息
    - PageContext实例
  - session
    - 表示一次会话
    - 保存用户信息
    - HttpSession实例
  - application
    - ServletContext实例
    - 表示当前Web应用
    - 可以保存所有用户共享信息
  - config
    - 当前jsp对应的servlet的ServletConfig对象
    - 获取当前页面配置信息
  - out
    - JspWriter实例
    - 向浏览器输出
    - 和<%=var%>作用类似
  - page
    - 当前jsp对应的servlet对象
    - Servlet实例
  - exception
    - 表示jsp页面发生的异常
    - Exception实例
  - 常用的
    - request
      - 常用方法
        - String getParameter(String key) 获取客户端传来的参数
        - void setAttribute(String key,Object value) 通过键值对的形式保存数据
        - Object getAttribute(String key) 通过key取出value
        - request.getRequestDispatcher("/target").forward(request,response) 服务器端跳转（转发）
        - String[] getParametersValues()
          - 如果参数中出现多个键值重复，则getParameter(String key)只能取出第一个
          - 而此方法可以获取所有值
        - request.setCharacterEncoding(String charset) 指定每个请求的编码
    - response
      - 常用方法
        - sendRedirect(String path)重定向
          - 客户端跳转（重定向）
    - session
      - 常用方法
        - String getId() 获取当前id
        - void setMaxInactiveInterval()  设置session的失效时间，单位为s
        - int getMaxInactiveInterval(int interval) 获取当前session的失效时间
        - void invalidate()  设置session立即失效
        - void setAttribute(String key,Object value) 通过键值对的形式来存储数据
        - Object getAttribute(String key)  通过键获取对应的数据
        - void removeAttribute(String key) 通过键删除对应的数据
    - application
    - pageContext

```java
//P8 8:00 ----2020年7月25日23:08:12
```

- ### Cookie

  - 在客户端和服务端之间来回传递

  - 存储在客户端

  - 对应的Java类:Cookie

    - 在javax.servlet包中

    - 创建

      ```java
      Cookie cookie = new Cookie("key","value");
      response.addCookie(cookie);
      ```

    - 读取

      ```java
      Cookie[] cookies = request.getCookies();
      for(Cookie cookie:cookies){
          String key = cookie.getName();
          String value = cookie.getValue();
      }
      ```

    - 设置有效时间

      ```java
      cookie.setMaxAge(int age);
      //单位s
      ```

    - 获取有效时间

      ```java
      int age = cookie.getMaxAge();
      //单位s
      //默认值-1 代表与会话生命期相同的时间
      ```

    - 对某一cookie做任意修改后必须要重新response.addCookie(cookie)一次才能有效果

  - 与session的区别

    - session保存在服务器，cookie保存在客户端
    - session保存的数据为Object类型，cookie保存字符串
    - session会话结束立即销毁，cookie与会话无关

```tex
//P13 0:00 ----2020年7月31日23:15:17
```

- ### JSP内置对象作用域

  - 拥有setAttribute和getAttribute类方法的内置对象才有讨论价值
  - page
    - 对应的内置对象是pageContext
  - request
    - 对应的内置对象是request
  - session
    - 对应的内置对象是session
  - application
    - 对应的内置对象是application
  - 以上为作用域名
  - 作用范围
    - page :在当前页面
    - request :在一次请求内有效
    - session :在一次会话中有效
    - application :在整个web应用有效

- ### EL表达式

  - Expression Language 表达式语言

  - 可以替代jsp页面中数据访问时的复杂编码

  - ${属性key} key为四大作用域对象的属性键值

  - 不指定作用域时，按照pageContext,request,session,application的顺序查找作用域对象中的属性

  - 指定作用域时，可以通过添加pageScope.，requestScope.,sessionScope.,applicationScope.前缀指定查找的作用域对象

  - 可以读取对象

    - demo

      ```java
      ${user.id}或者${user["id"]}
      //转换为servlet后变为((User)pageContext.getAttribute("user")).getId();
      ```

    - 也可以级联读取对象

  - 也可以赋值，但一般不使用
  - 也可以执行表达式
    - && || ! < > <= >= ==等等
      - && and
      - || or
      - ! not
      - == eq
      - != ne
      - < lt
      - \> gt
      - <= le
      - \>= ge
      - empty 判断是否为空 空则返回true
        - 引用类型null
        - 长度为0的字符串
        - size为0的集合
    - 操作对象为上述属性值

```text
//P15 0:00 ----2020年8月2日00:00:23
```

- ### JSTL

  - Jsp Standard Tag Library Jsp标准标签库

  - Jsp为开发者提供的一系列的标签，使用这些标签可以完成一些逻辑处理，比如循环遍历集合，让代码更简洁，不再出现jsp脚本穿插的情况

  - 只能在jsp中使用

  - 实际开发中和EL结合使用

  - 使用

    - 需要导入jar包
      - jstl.jar
      - standard.jar
      - 导入方法:
        - 在WEB-INF文件夹下新建文件夹lib
        - 将两个jar包粘贴到lib下
        - 导入到项目的library中

    - 在jsp页面头导入jstl标签库 demo:

      ```jsp
      <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
      ```

      

    - 在需要的地方使用

  - ##### 以下部分为核心标签库

    ```jsp
    <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
    ```

    

  - forEach用法 demo

    ```jsp
    <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
    <c:forEach items="${users}" var="user" begin="起始索引" end="结尾索引" step="步长" varStatus="sta">
          <tr>
            <td>${user.id}</td>
            <td>${user.name}</td>
          </tr>
    </c:forEach>
    
    //varStatus属性作用：存储遍历元素的索引等信息，可以使用EL表达式等方式取出
    ```

  - set用法

    ```jsp
    //对数据类型属性的设置
    <c:set var="key" value="value" [scope="scope"]></c:set>
    
    //对对象类型属性的设置
    <c:set target="${keyOfTargetObject}" property="name" value="value"></c:set>
    ```

  - out用法

    ```jsp
    <c:out value="${keyOfTargetObject}" default="默认值"></c:out>
    ```

  - remove用法

    ```jsp
    <c:remove var="keyOfTargetObject" scope="page"></c:remove>
    ```

  - catch用法

    ```jsp
    <c:catch var="customErrorName">
    	可能会出现异常的片段
    </c:catch>
    ${customErrorName}//输出
    ```

  - if用法 demo

    ```jsp
    <c:id test="${num1>num2}">
    	....
    </c:id>
    ```

  - choose when otherwise用法

    ```jsp
    <c:choose>
    	<c:when test="${num1>num2}">
        	。。。
        </c:when>
        <c:otherwise>
        	。。。
        </c:otherwise>
    </c:choose>
    ```

  - ##### 以下部分为格式化标签库

    ```jsp
    <%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
    ```

  - formatDate用法

    ```jsp
    <fmt:formatDate value="${keyOfTargetObject}" pattern="格式，例如(yyyy-MM-dd HH:mm:ss)"></fmt:formatDate>
    ```

  - formatNumber用法

    ```jsp
    <fmt:formatNumber value="原始值" maxIntegerDigits="" maxFractionDigits=""></fmt:formatNumber>
    ```

  - ##### 以下为函数标签库

    ```jsp
    <%@taglib prefix="func" uri="http://java.sun.com/jsp/jstl/functions" %>
    ```

  - contains用法

    ```jsp
    ${func:contains(keyofTarget,"Python")} //返回是否包含指定字符串
    ```

  - startsWith用法

    ```jsp
    ${func:startsWith(keyOfTarget,"Java")} //返回是否以指定串结尾
    ```

  - endsWith用法

    ```jsp
    ${func:endsWith(keyOfTarget,"C")} //返回是否以指定串结尾
    ```

  - indexOf用法

    ```jsp
    ${func:indexOf(keyOfTarget,"va")} //返回指定串的首次出现索引
    ```

  - replace用法

    ```jsp
    ${func:replace(keyOfTarget,"C","Python")} //将目标字符串(1)中的目标子字符串(2)替换为指定字符串(3),并返回结果
    ```

  - subString用法

    ```jsp
    ${func:subString(keyOfTarget,起始索引,终止索引)}  (1)为起始索引,(2)为长度,返回截取结果
    ```

  - split用法

    ```jsp
    ${func:split(keyOfTarget,"隔离符")[索引值]}
    ```

- ### 过滤器

  - Filter

  - 对请求进行过滤与预处理

  - 对响应进行过滤与处理

  - 或者说以某种方式处理正在客户端和服务端之间交换的数据流

  - 使用时与Servlet类似，Filter是Java Web提供的一个接口，开发者只需要自定义一个类并且实现该接口即可

  - 拦截请求和响应后需要将请求和响应转发到下一个拦截器或servlet

    ```java
    filterChain.doFilter(servletRequest,servletResponse);
    ```

    

  - 配置

    - 在web.xml中配置

      ```xml
      <filter>
          <filter-name>firstfilter</filter-name>
          <filter-class>filters.FirstFilter</filter-class>
      </filter>
      
      <filter-mapping>
          <filter-name>firstfilter</filter-name>
          <url-pattern>/path1</url-pattern>
          <url-pattern>/path2</url-pattern>
          <url-pattern>/path3</url-pattern>
      </filter-mapping>
      ```

    - 注解配置

      ```java
      @
      ```
    
  - 生命周期
  
    - 启动服务器时创建实例，同时调用init执行初始化（不同于servlet）
    - 每次收到请求时调用doFilter()方法
    - 服务器通过web.xml或注解识别到过滤器
  
  - 同时配置多个filter时，作用顺序由在web.xml中的配置顺序决定，由上到下
  
  - 使用注解配置时无法固定作用顺序
  
  - 常用使用场景
  
    - 统一处理中文乱码
    - 屏蔽敏感词
    - 控制资源的访问权限

``` tex
//P18 0:00 ----2020年8月2日16:46:55
```

​				

```tex
//P19 0:00 ----2020年8月2日22:52:12
```

- ### 文件上传

  - 表单提交

    - ##### 前端

    - file 类型的input

    - form表单的method设置为post

    - form表单的enctype设置为multipart/form-data,以二进制的形式传输数据

    - submit按钮提交

    - ##### 后台

    - 通过request.getInputStream()获取输入流

    - 获取文件夹的绝对路径

      - String path = request.getServletContext().getRealPath("file");
      - 使用idea时需要将文件夹建立在out或其子目录下

    - ##### fileupload组件

      - 导包	

        - commons-fileupload-xxx.jar
        - commons-io-xxx.jar

      - 把所有请求信息解析成FileItem对象，可以通过对FileItem对象的操作完成上传，面向对象的思想

      - 过程

        ```java
        public void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
                DiskFileItemFactory diskFileItemFactory = new DiskFileItemFactory();
                ServletFileUpload servletFileUpload = new ServletFileUpload(diskFileItemFactory);
                List<FileItem> files;
                try {
                    files = servletFileUpload.parseRequest(request);
                    for(FileItem fileItem:files){
                        if(!fileItem.isFormField()){
                            //是文件
                            String filename = new String(fileItem.getName().getBytes(),"UTF-8");
                            System.out.println(filename);//获取文件名
                            System.out.println(fileItem.getFieldName());//获取表单中input的name
                            long size = fileItem.getSize();//获取文件大小，以字节为单位
        
                            InputStream inputStream = fileItem.getInputStream();
                            OutputStream outputStream = new FileOutputStream(request.getServletContext().getRealPath("file/uploads/" + filename));
                            byte[] bytes = new byte[5000];
                            int len = 0;
                            while(true){
                                len = inputStream.read(bytes);
                                if(len == -1){
                                    break;
                                }
                                outputStream.write(bytes,0,len);
                            }
                            System.out.println(fileItem.getFieldName() + "域上传成功!");
                            outputStream.close();
                        }
                        else{
                            //不是文件
                            String inputName = fileItem.getFieldName();//获取表单中input的name
                            String inputValue = fileItem.getString("UTF-8");//获取表单中的值
        					
                        }
        
                    }
                } catch (FileUploadException e) {
                    e.printStackTrace();
                }
            }
        ```

- ### 文件下载

  - demo

    ```java
    public void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException{
            response.setContentType("application/x-msdownload");
            String fileName = "网页下载文件.mp3";
            response.setHeader("Content-Disposition","attachment;filename=" + fileName);
            OutputStream outputStream = response.getOutputStream();
            String path = request.getServletContext().getRealPath("file/downloads/didi.mp3");
            InputStream inputStream = new FileInputStream(path);
            byte[] bytes = new byte[5000];
            int len = 0;
            while(true){
                len = inputStream.read(bytes);
                if(len == -1){
                    break;
                }
                outputStream.write(bytes,0,len);
            }
            inputStream.close();
            outputStream.close();
        }
    ```

- ### 在idea项目结构中的web目录下建立文件夹或文件夹，会被打包进项目，且web目录对应request.getServletContext().getRealPath("")的结果

  

```tex
//P21 0:00 ----2020年8月3日23:31:14
```

- ### Ajax

  - Asynchronous JavaScript And XML :一步的JavaScript 和 XML

  - Ajax不是新的编程，指的是一种交互方式，异步加载，客户端和服务器的数据交互更新在局部页面中的技术

  - 示例1

    - 前台

      ```jsp
      <%@ page contentType="text/html;charset=UTF-8" language="java" %>
      
      <html>
        <head>
          <title></title>
          <link href="css/index/index.css" rel="stylesheet" type="text/css"/>
          <script src="js/jquery.3.2.1.min.js"></script>
        </head>
      
        <body>
          <div id="div"></div>
          <input type="text" id="input" />
          <input type="button" value="提交" id="btn"/>
          <script>
            $(function(){
              $("#btn").click(function () {
                var text = $("#input").val();
                $.ajax({
                  url:"/ajax",
                  type:"post",
                  data:"text=" + text,
                  dataType:"text",//服务器返回的类型
                  success:function(result){
                    $("#div").text(result);
                  }
                })
              })
            })
          </script>
        </body>
      </html>
      ```

    - 后台

      ```java
      @WebServlet("/ajax")
      public class AjaxServlet extends HttpServlet {
          public void doPost(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException{
              String text = request.getParameter("text");
              System.out.println(text);
              response.setContentType("text/plain;charset=UTF-8");
              response.getWriter().write(text);
              //之将需要的数据返回即可
          }
      }
      ```

      

- ### JSON工具类

  - json-lib

  - demo1

    ```java
    public void doPost(HttpServletRequest request, HttpServletResponse response) throws IOException {
            String text = request.getParameter("text");
            System.out.println(text);
            response.setCharacterEncoding("UTF-8");
            User user = new User(20,"张三",180.4);
            JSONObject jsonObject = JSONObject.fromObject(user);
            System.out.println(jsonObject.toString());
            response.getWriter().write(jsonObject.toString());
        }
    ```

- ### JDBC

  - Java DataBase Connectivity
  - 是一个独立于特定数据库的管理系统
  - 通用的SQL数据库存取和操作的公共接口
  - 定义了一组标准，为访问不同数据库提供了统一途径
  - 又分为
    - 面向应用
      - 由java提供者实现
    - 面向数据库
      - 由数据库厂商实现
  - java.sql和javax.sql
    - DriverManager接口
      - 提供者：java官方
      - 管理不同的JDBC驱动
    - Connection接口
    - Statement接口
      - 一般使用Statement的子类 PreparedStatement
    - ResultSet接口
  - 使用
    - 前提：引入驱动jar包
      - 是向下兼容的
    - 加载数据库驱动，java程序和数据库之间的桥梁
    - 获取Connection，java程序与数据库的一次连接
    - 创建Statement对象，有Connection产生，执行SQL语句
    - 如果需要接收返回值，创建ResultSet对象，保存Statement执行之后所查询到的结果

```tex
//P27 0:00 ----2020年8月5日00:38:12
```

- ### 数据库连接池

  - 装有很多Connection对象的缓冲池

  - 连接对象资源重复利用，减轻资源浪费

  - 使用javax.sql.DataSource接口实现，是java官方提供的接口，使用时开发者不需要自己实现，可以使用第三方工具

  - 实际开发中直接使用C3P0完成数据库连接池的操作

  - 使用

    - 导包

      ![image-20200805192038444](D:\notes\随笔\note-imgs\image-20200805192038444.png)

    - 代码

      ```java
      //创建连接池对象
              ComboPooledDataSource comboPooledDataSource = new ComboPooledDataSource([name]);//如果使用配置文件配置初始化参数，则构造方法中的参数必填,且值为配置文件中<named-config>标签的name属性值，且配置文件的文件名必须为 c3p0-config.xml
      
              //连接数据库
              comboPooledDataSource.setDriverClass("com.mysql.cj.jdbc.Driver");
              comboPooledDataSource.setJdbcUrl("jdbc:mysql://localhost:3306/users?serverTimezone=UTC&useUnicode=true&characterEncoding=UTF-8");
              comboPooledDataSource.setUser("root");
              comboPooledDataSource.setPassword("bighand");
              Connection connection = comboPooledDataSource.getConnection();
              System.out.println(connection);
      ```

    - close方法

      使用完成后依旧使用连接对象的close方法,但是此close方法会把连接对象还回连接池，而不是释放

    - 也可以指定初始化配置信息
      - 初始化连接个数 
      - 设置最大连接数
      - 连接个数不够时再次申请的连接个数
      - 设置最小剩余连接数
      - 以上配置可以使用xml配置 

- ### DBUtils

  - 将结果集映射成java对象

```tex
//Java Web基础完结 ----2020年8月5日21:34:49
```

