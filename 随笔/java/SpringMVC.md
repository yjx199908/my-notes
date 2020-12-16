```java
//P2 09:44 ----2020年9月17日18:05:04
```

- #### 搭建开发环境（idea）

  - File - New - Project - Maven - Create From Archetype - 

  - ![image-20200918091419829](D:\notes\随笔\note-imgs\image-20200918091419829.png)

  - Next - ....

  - 搭建完成后

    - 根据下条补全目录，并将java目录设置为Sources Root, 将resources目录设置为Resources Root
      - 右击目录
      - Mark Directory as
      - Sources Root / Resources Root

  - 配置依赖

    ```xml
      <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <spring.version>5.0.2.RELEASE</spring.version>
      </properties>
    
      <dependencies>
        <dependency>
          <groupId>junit</groupId>
          <artifactId>junit</artifactId>
          <version>4.12</version>
          <scope>test</scope>
        </dependency>
        <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-context</artifactId>
          <version>${spring.version}</version>
        </dependency>
        <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-web</artifactId>
          <version>${spring.version}</version>
        </dependency>
        <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-webmvc</artifactId>
          <version>${spring.version}</version>
        </dependency>
        <dependency>
          <groupId>javax.servlet</groupId>
          <artifactId>servlet-api</artifactId>
          <version>2.5</version>
          <scope>provided</scope>
        </dependency>
        <dependency>
          <groupId>javax.servlet</groupId>
          <artifactId>jsp-api</artifactId>
          <version>2.0</version>
          <scope>provided</scope>
        </dependency>
      </dependencies>
    ```

  - 配置前端控制器（web.xml）

    ```xml
    <web-app>
      <display-name>Archetype Created Web Application</display-name>
    
      <servlet>
        <servlet-name>dispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--为前端控制器配置配置文件-->
        <init-param>
          <param-name>contextConfigLocation</param-name>
          <param-value>classpath:springmvc.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
      </servlet>
      
      <servlet-mapping>
        <servlet-name>dispatcherServlet</servlet-name>
        <url-pattern>/</url-pattern>
      </servlet-mapping>
    </web-app>
    ```

  - 在resources目录下创建spring配置文件（名字随意，例如：springmvc.xml）

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:mvc="http://www.springframework.org/schema/mvc"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:context="http://www.springframework.org/schema/context"
           xsi:schemaLocation="
            http://www.springframework.org/schema/beans
            https://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/mvc
            https://www.springframework.org/schema/mvc/spring-mvc.xsd
            http://www.springframework.org/schema/context
            https://www.springframework.org/schema/context/spring-context.xsd">
        
        <!--包扫描，以后如果继续使用注解型，则需要逐步将需要被扫描的包都如此配置-->
    	<context:component-scan base-package="per.controller"></context:component-scan>
    
        <!--配置视图解析器，将被@RequestMapping注解的方法的返回值映射成视图文件地址-->
        <bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
            <!--路径从webapp目录的子目录起，value必须以/结尾-->
            <property name="prefix" value="/WEB-INF/pages/"/>
            <property name="suffix" value=".jsp"></property>
        </bean>
    
        <!--开启mvc中的注解-->
        <mvc:annotation-driven/>
        
    </beans>
    ```

  - 部署服务器

    

- #### 初始目录结构(不够的补全)

  - project
    - src
      - main
        - java(**存放类**)
        - resources(**存放配置文件**)
        - webapp
          - pages(**存放视图文件**)
          - WEB-INF
          - index.jsp
    - pom.xml
    - 。。。

- #### @RequestMapping

  - 请求映射注解
  - 添加位置1：方法上
    - 属性
      - path : 指定希望响应的路径
      - method: 枚举类型，指定请求的方法，可以有多个值（RequestMethod.POST|RequestMethod.GET|...）
      - params: 限定请求参数[以及参数值]，如果不符合，则不响应
      - headers: 限定请求中必须包含的请求头
      - ...
  - 添加位置2: 类上
    - 属性
      - path: 指定前置路径
      - method: 枚举类型，指定请求的方法，可以有多个值（RequestMethod.POST|RequestMethod.GET|...）
      - params: 限定请求参数[以及参数值]，如果不符合，则不响应
      - headers: 限定请求中必须包含的请求头
      - ...
  - 类上如果有此注解，则类上注解的路径和方法上路径拼接后才是能触发此方法的路径
  - 被此注解被标注的方法的返回值一般情况下会被spring容器中的视图解析器添加前缀和后缀，解析为完整文件路径，并将请求转换为对此文件的访问

- #### 执行流程图

  ![image-20200918110936613](D:\notes\随笔\note-imgs\image-20200918110936613.png)

  - ##### SpringMVC基于组件方式执行流程

- #### 请求参数的绑定

  - 设置被@RequestMapping注解的方法的参数列表

  - url上对应参数名的参数值会被相应地加载到形参列表中（按参数名匹配）

  - 在方法中直接使用即可

  - ##### 如果形参列表被配置为一个引用类型，则请求中的参数还会根据属性名与参数名的匹配自动生成相应类型的对象并赋值给形参

  - ##### 如果表单的name属性被写成param.param的形式，则还会生成上述对象的属性成员对象

  - ##### 如果希望直接封装到集合里，则将表单的name属性写成 集合名[索引值].属性名的形式, .属性名可有可无

  - ##### 如果希望直接封装到Map里，则将表单的name属性写成 集合名[key].属性名的形式, .属性名可有可无

- #### 配置解决中文乱码的过滤器(spring定义好的对象)

  ```xml
  <filter>
    <filter-name>characterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
      <param-name>encoding</param-name>
      <param-value>UTF-8</param-value>
    </init-param>
  </filter>
  
  <filter-mapping>
    <filter-name>characterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>
  ```

- #### 自定义类型转换器

  - 自定义一个类实现Convert接口(spring核心容器中的Convert：Convert<E,T> E->T )

  - 实现convert方法，待转换值作为参数，结果作为返回值

  - 在spring配置文件中配置bean

    ```xml
    <!--org.springframework.context.support.ConversionServiceFactoryBean为Spring定义好的转换器工厂-->
    <bean id="conversionService" class="org.springframework.context.support.ConversionServiceFactoryBean">
        <property name="converters">
            <set>
                <bean class="per.convert.StringToDateConverter"></bean>
            </set>
        </property>
    </bean>
    ```

  - 在spring配置文件中开启对此转换器的支持

    ```xml
    <mvc:annotation-driven conversion-service="conversionService"/>
    ```

- #### 在控制器方法中获取Servlet原生API

  - 需要获取什么API，就在方法形参列表直接写上，spring会自动赋值

- #### @RequestParam

  - 作用：如果请求参数名和控制器方法参数名不相同，但又希望使用此参数接收此请求参数的值，则需要使用@RequestParam来建立映射关系
  - 位置：用在控制器方法形参上
  - 属性：
    - name: 指定希望接收的请求参数名

- #### @RequestBody

  - 作用：接收post请求后，使控制器方法的一个参数获取【key1=value1&key2=value2】形式的请求体字符串
  - 位置：用在控制器方法形参上，但此形参不可以与任意请求参数名相同
  - 不可以在get方法中使用

- #### @PathVariable

  - 作用：用于绑定url中的占位符

  - demo

    ```java
    @RequestMapping(path = "/var/{sid}")
    public String varTest(@PathVariable(name = "sid") Integer id){
        System.out.println(id);
        return "success";
    }
    //此时如果有url过来，则对应位置上的值将会赋给id
    ```

  - 属性

    - name：指定占位符

- #### @RequestHeader

  - 作用：取出header中某一项的值并赋值给控制器方法形参
  - 属性
    - value: 指定header的某一个key

- #### @CookieValue

  - 作用：取出cookie中某一项的值并赋值给控制器方法形参
  - 属性
    - value: 指定cookie的某一个key

```java
//P23 0:00 ----2020年9月18日17:49:51
```



- #### @ModelAttribute

  - 作用
    - 注解在方法上
      - 则此方法在响应方法之前执行
      
      - 可以对请求进行预处理
      
      - 此方法的形参规则与控制器方法类似
      
      - 此方法的返回值会被后面的控制器方法接收
      
      - ##### 如果没有返回值
      
        - 还可以给此方法加一个Map类型的形参，然后将需要传递的数据添加到map里，此方式也可以进行数据传递
      
        - ##### 使用此方式传递的数据需要用特殊方式取出，见（注解在控制器方法参数列表中）
      
    - 注解在控制器方法参数列表中
    
      - 此时需要使用value属性指定传过来的map集合的key
      - 这样对应此key的value会被赋值给被注解的形参
  
- #### @SessionAttributes

  - 作用：只能作用在类上，作用是将指定的 Model 的键值对保存在 **session** 中。可以让其他请求共用 session 中的键值对。

    - ##### 指定保存的属性名

      - 作用是将 Model 中指定 **属性名** 的键值对保存在 session 中。
      - 使用 `@SessionAttributes(value = {"user1", "user2", "user3"})` 可以指定多个添加到 session 中的 Model 属性名。

    - ##### 指定保存的属性类型

      - 作用是将 Model 中指定 **类型** 的键值对保存在 session 中。

      - 同样的，可以使用 `@SessionAttributes(types = {User.class, Person.class})` 来指定多个类型。

  - demo

    ```java
    @Controller
    @SessionAttributes("user")
    public class SessionAttributesController {
    
    	@RequestMapping("/user-information")
    	public void setSessionModel(Model model) {
    		User user = new User("小明", 18);
    		model.addAttribute("user", user);
    	}
    
    	@RequestMapping("/testSession")
    	public String get() {
    
    		return "test";
    	}
    }
    ```

  - ##### 如果不使用此注解的话，存到model中的键值对会保存在request中，使用的话则request和session中都有一份

- ##### JSPpage指令中添加 isELIgnored="false" 后可以在页面中使用EL表达式

- #### 控制器方法的返回值类型

  - String
    - 常用情况
    - 使用返回字符串的形式转发或重定向
      - 返回值以  forward:  开头，后接项目内视角uri（不会经由视图解析器）: 启用转发
      - 返回值以  redirect:  开头，后接url（不会经由视图解析器）: 启用重定向
        - url不以项目名开头
  - void
    - 默认返回请求路径字符串
    - 也可以使用request对象请求转发，但手动转发不会执行视图解析器
    - 也可以使用resoponse对象进行重定向，但要写完整URL （包含虚拟路径 ）
      - 使用request.getContextPath()可以获取得到的是项目的虚拟路径进行拼接
    - 也可以使用response.getWriter()直接向流中写入数据，如果这样做，也不会触发默认返回值
  - ModelAndView
    - 在控制器方法中创建ModelAndView类型对象，例如mv
    - 使用mv.addObject(Object)方法存数据，底层也会把数据存入request中
    - 使用mv.setViewName(String)方法设置视图名称，后续会执行视图解析器将视图名称解析为目标视图位置，与直接返回String类型类似
    - 返回mv

```java
//P29 04:29 ----2020年9月19日15:06:11
```

- #### 响应json数据(将返回值转换为json字符串发送给前端)

  - 引入依赖

    ```xml
    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>fastjson</artifactId>
      <version>1.2.58</version>
    </dependency>
    ```

  - springmvc配置文件中添加配置

    ```xml
    <mvc:annotation-driven>
        <mvc:message-converters register-defaults="true">
            <!-- 配置Fastjson支持 -->
            <bean class="com.alibaba.fastjson.support.spring.FastJsonHttpMessageConverter">
                <property name="supportedMediaTypes">
                    <list>
                        <value>application/json</value>
                        <value>text/html;charset=UTF-8</value>
                    </list>
                </property>
                <!--            <property name="features">
                                    <list>
                                        <value>WriteMapNullValue</value>
                                        <value>QuoteFieldNames</value>
                                    </list>
                                </property>-->
            </bean>
        </mvc:message-converters>
    </mvc:annotation-driven>
    ```

- #### 实现json数据和javaBean对象的互转需要的依赖（后端转换ajax的post数据）

  - 可以将@RequestBody生成的post请求数据字符串转换成javaBean对象
- 在控制器方法参数中使用对象接收，且需要用@RequestBody注解
  
```xml
  <dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.11.2</version>
  </dependency>
  <dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-core</artifactId>
    <version>2.11.2</version>
  </dependency>
  <dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-annotations</artifactId>
    <version>2.11.2</version>
  </dependency>
```

- #### springmvc接收前端axios的post数据（前端转换ajax的post数据，转成正常post格式）

  - 前端设置请求头：'Content-Type':'application/x-www-form-urlencoded'

  - 前端将数据使用URLSearchParams格式化

    ```js
    export default function(path,data){
        return request({
            method:'post',
            url:path,
            data,
            headers:{'Content-Type':'application/x-www-form-urlencoded'}
        })
    }
    
    login_in(){
        let data = new URLSearchParams()
        for(let item in this.login_info){
            data.append(item,this.login_info[item])
        }
    
        post("/login-in",data).then(result=>{
            console.log(result.data);
        })
    }
    ```


- #### 告诉前端控制器，哪些资源请求不拦截（静态资源过滤）

  - 在springmvc.xml文件中配置<mvc:resources mapping="" location="" />标签
  - mapping和location都可以用通配符，例如 /js/** ：js目录下所有文件请求都不拦截

- #### springmvc文件正常上传

  - 导入依赖

    ```xml
    <dependency>
      <groupId>commons-fileupload</groupId>
      <artifactId>commons-fileupload</artifactId>
      <version>1.3.1</version>
    </dependency>
    <dependency>
      <groupId>commons-io</groupId>
      <artifactId>commons-io</artifactId>
      <version>2.2</version>
    </dependency>
    ```

  - 文件上传的前提
    - form表单的enctype属性必须是：multipart/form-data
      - 默认值是：application/x-www-form-urlencoded
      - enctype表示请求正文的类型
    - method属性的值必须是post
    - 提供一个文件域\<input type="file" /\>
    
  - 配置文件解析器
  
    ```xml
    <!--id不可以改-->
    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <property name="maxUploadSize" value="10485760"></property>
    </bean>
    ```
  
  - 在控制器方法中添加MultipartFile类型的参数
  
    ```java
    @RequestMapping("/path")
    public void func(...,MultipartFile upload,...){//类型为MultipartFile,变量名必须和表单中文件域的name相同才能对应上
        ...
    }
    ```
  
  - MultipartFile对象的使用
  
    ```java
    upload.getOriginalFilename();//获取文件名
    upload.transferTo(new File(...));//写入指定位置
    ```

```java
//P37 05:21 ----2020年9月29日11:50:55
```



- #### springmvc跨服务器方式的文件上传

  - 导入依赖

    ```xml
    <dependency>
      <groupId>com.sun.jersey</groupId>
      <artifactId>jersey-core</artifactId>
      <version>1.18.1</version>
    </dependency>
    <dependency>
      <groupId>org.glassfish.jersey.core</groupId>
      <artifactId>jersey-client</artifactId>
      <version>2.32</version>
    </dependency>
    ```

  - 创建客户端对象

    ```java
    Client client = Client.create();
    String path = "http://localhost:8099/uploads/"; 
    String filename = upload.getOriginalFilename();
    WebResource webResource = client.resource(path+filename);
    webResource.put(upload.getBytes()) //upload为上种方式中的upload
    ```

    

- #### UUID

  - UUID是Universally Unique Identifier的缩写，它是在一定的范围内（从特定的名字空间到全球）唯一的机器生成的标识符

  - 为了保证UUID的唯一性，规范定义了包括网卡MAC地址、时间戳、名字空间（Namespace）、随机或伪随机数、时序等元素，以及从这些元素生成UUID的算法。UUID的复杂特性在保证了其唯一性的同时，意味着只能由计算机生成

  - UUID是不能人工指定的，除非你冒着UUID重复的风险。UUID的复杂性决定了“一般人“不能直接从一个UUID知道哪个对象和它关联

  - UUID是1.5中新增的一个类，在java.util下，用它可以产生一个号称全球唯一的ID

    ```java
    public static String getUUID32(){
        String uuid = UUID.randomUUID().toString().replace("-", "").toLowerCase();
        return uuid;
    //  return UUID.randomUUID().toString().replace("-", "").toLowerCase();
    }
    ```

- #### springmvc的异常处理

  - 自定义一个异常类（继承Exception）
  - 自定义一个异常处理器类（实现HandlerExceptionResolver），实现其中的resolveException方法，执行一系列异常处理操作
  - 在方法最后创建并返回ModelAndView类型对象，指挥视图解析器
  - 将异常处理类加入容器

- #### springmvc中的拦截器

  - 只有在springmvc中才能使用
  - 只会拦截控制器方法
  - 链式执行
  - 使用步骤
    - 自定义一个拦截器类，实现HandlerInterceptor接口，重写其方法
      - boolean preHandle(..)：
        - 在控制器方法执行前执行
        - 返回值为true则放行，否则不放行
      - void postHandle(..):
        - 在控制其方法执行后执行
      - void afterCompletion(..):
        - 目标程序执行后执行
    - 配置拦截器
      - 在springmvc.xml文件中添加\<mvc:interceptors\>\</mvc:interceptors\>标签
      - 在mvc:interceptors标签中添加\<mvc:interceptor\>\</mvc:interceptor\>标签
      - 在mvc:interceptor标签中添加\<mvc:mapping  path=""\>或者\<mvc:exclude-mapping path=""\>标签
        - 二选一用
        - 前一个是配什么拦什么
        - 后一个是配什么不拦什么，其他都拦
        - path指明拦截的请求路径，可以使用通配符/\*或者/\*\*
      - 在mvc:interceptor标签中添加\<bean\>，将自定义的拦截器对象添加进容器

- #### 整合SSM

  - Controller放进springmvc容器，不放进Spring容器

    - 在spring包扫描时，排除controller

      ```xml
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
              https://www.springframework.org/schema/beans/spring-beans.xsd
              http://www.springframework.org/schema/context
              https://www.springframework.org/schema/context/spring-context.xsd">
          <context:component-scan base-package="org.example">
          	<context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller">
          </context:component-scan>
      </beans>
      ```

  - 如何加载spring容器

    ![image-20200929160230134](D:\notes\随笔\note-imgs\image-20200929160230134.png)

  - 在web.xml中配置上图中的监听器，由spring提供

    ```xml
    <listener>
    	<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
    <!--默认只加载WEB-INF目录下的applicationContext.xml配置文件-->
    <!--解决方案1：将配置文件剪切到WEB-INF目录下-->
    <!--解决方案2：设置配置文件的路径，如下：-->
    <context-param>
    	<param-name>contextConfigLocation</param-name>
        <param-value>classpath:applicationContext.xml</param-value>
    </context-param>
    ```

    

```
//完结 ----2020年9月29日19:15:42
```

