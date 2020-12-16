- #### 优点

  - 快速创建独立运行的Spring项目以及与主流框架集成
  - 使用嵌入式的Servlet容器，应用无需打成war包
  - starters自动依赖与版本控制
  - 大量的自动配置，简化开发，也可修改默认值
  - 无需配置XML,无代码生成，开箱即用
  - 准生产环境的运行时应用监控
  - 与云计算的天然集成

- #### 缺点

  - 入门容易，精通难

- #### 简介

  - 简化Spring应用开发的一个框架
  - 是Spring框架的一个抽象

- #### 微服务

  - 是一种架构风格
  - 一个应用应该是一组小型服务，可以通过HTTP的方式进行沟通
  - 每一个功能元素最终都是一个可独立替换与升级的软件单元
  - 缺点：部署和运维成本很高

```java
//P3 0:00 ----2020年8月26日08:53:14
```

- #### idea 创建maven项目爆红多半是pom文件里配置的版本号有问题

- #### 创建项目

  - 需要先创建一个maven项目，前提是把settings.xml和仓库配置好，仓库最好不要放在系统盘

  - 然后在pom文件中配置springboot依赖

    ```xml
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.5.RELEASE</version>
    </parent>
    
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>1.5.9.RELEASE</version>
        </dependency>
    </dependencies>
    ```

  - 然后配置java编译版本

- #### 配置java编译版本

  - 在pom文件中添加

    ```xml
    <profiles>
        <profile>
            <id>jdk-1.8</id>
            <activation>
                <activeByDefault>true</activeByDefault>
                <jdk>1.8</jdk>
            </activation>
            <properties>
                <maven.compiler.source>1.8</maven.compiler.source>
                <maven.compiler.target>1.8</maven.compiler.target>
                <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
            </properties>
        </profile>
    </profiles>
    ```

- #### 添加主程序

  - 在main/java目录下创建java类，并添加注解@SpringBootApplication,说明此类是主程序类

  - 在主程序类中添加main方法

  - 在main方法中添加语句

    ```java
    SpringApplication.run(主程序类.class,args);
    ```

  - 编写相关的controller
    - 与创建主程序类似，创建controller也是在类上使用注解 ：@Controller
    - controller类中添加方法，方法上添加注解
      - @RequestMapping("/path") :/path为请求路径
      - @ResponseBody : 此方法返回值直接为响应数据

- #### 简化部署

  - 在pom文件中dependencies标签之后添加build

    ```xml
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
    ```

  - 在idea界面右边缘找到Maven窗口

  - 展开项目名

  - 展开Lifecycle

  - 双击package

  - 之后会出现一个红色的输出文件夹

  - 里面有一个jar包，把jar包随便复制到一个地方，然后使用cmd运行

    ```
    java -jar jar包名.jar
    ```

- #### 版本仲裁

  - pom文件中的parent代表SpringBoot的父项目
  - 但其实副项目的依赖中还有一个父项目，此项目为SpringBoot的管理者
  - 在祖父项目中，各个依赖规划都已经写好了，所以一般来讲，在我们自己的pom文件中添加除父项目之外的依赖时，是不需要添加版本号的
  - 但maven拉取失败的情况除外，这时候便需要手动添加版本号覆盖默认版本号，不断尝试，直到maven拉取成功

- #### spring-boot-starter-web

  - spring-boot-starter:spring-boot场景启动器

    - 帮我们导入web模块正常运行所依赖的组件

    - ##### springboot将所有的功能模块都抽取出来，做成一个个的starters(启动器),只需要在项目里面引入这些starter，相关场景的所有依赖都会导入进来，想用什么功能就导入什么场景的启动器

- #### 所有的controller和service等只能写在主类所在的包以及其所有的子包中，否则无法被扫描，也就无法起作用

- #### 其他目录结构

  - resources
    - static: 用来保存静态资源
    - templates: 保存所有的模板页面
      - 不支持JSP，但支持模板引擎
    - application.properties: 默认的配置文件



- #### 配置文件（全局）

  - application.properties

  - application.yml

  - 作用：修改SpringBoot自动配置的默认值

  - YAML A Markip Language: 是一个标记语言

  - YAML isn't Markup Language: 不是一个标记语言

  - YAMK :

    - 以数据为中心

    - 基本语法

      - k:(空格)v 表示键值对,冒号后的空格必须有
      - 以空格的缩进来控制层级关系，只要是左对齐的数据，都是同一层级的
      - 属性和值是大小写敏感的

    - 值的写法

      - 字面量：普通的值（数字，字符串，布尔）

        - 字符串默认不用加引号
        - "":不会转义特殊字符，特殊字符会作为本身想表达的的意思
          - "zhangsan \n lisi"=>zhangsan 换行 lisi
        - '':会转义特殊字符,特殊字符原样输出
          - 'zhangsan \n lisi'=>zhangsan \n lisi

      - 对象、Map（属性和值）（键值对）

        ![image-20200827094825763](D:\notes\随笔\note-imgs\image-20200827094825763.png)

      - 数组（List、Set）

        - 用-(空格)值来表示一个元素
        - 或者用[v1,v2,v3...]

- #### @ConfigurationProperties

  - 告诉SpringBoot将本类中的所有属性和配置文件中相关的配置进行绑定

  - @ConfigurationProperties(prefix = "配置名")

    - prefix: 代表配置文件中哪个下面的所有属性进行一一映射

  - 导入配置文件处理器，配置文件进行绑定就会有提示：

    - 在pom文件中添加

      ```xml
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-configuration-processor</artifactId>
          <optional>true</optional>
      </dependency>
      ```

  - 只有将bean类注册为容器中的组件，才能提供容器中的功能
    
    - 为此bean类添加注解@Component
  - 使用这种方式注入需要实现给bean的需要注入的属性添加setter方法（必须按照bean规范书写）
  - 在bean中取值时，在属性上添加注解@Value("${配置文件中的key}")

- #### application.properties

  - 需要进行编码转换
  - 在bean中取值时，在属性上添加注解@Value("${配置文件中的key}")

- #### application.properties和application.yml配置的区别

  - application.properties
    - 不支持松散语法绑定
    - 不支持JSR303
    - 不支持复杂数据类型
  - application.yml
    - 与上面相反

- #### 使用自定义配置文件

  - 非application配置文件
  - 需要手动加载
  - @PropertySource(value = {"classpath:文件名"} )
  - @ImportResource：导入Spring的配置文件，让配置文件里面的内容生效

- #### 配置类

  - 使用@Configuration标注
  - @Bean 
    - 是一个方法注解
    - 作用是将方法的返回值添加到容器中
    - 容器中这个组件默认的id就是方法名

- #### 配置文件占位符

  - 在properties类型配置文件中
    - value中可以嵌入${key}表达式
      - key可以是上面的配置
      - 也可以是random.xxx

- #### Profile

  - 是Spring对不同环境提供不同配置功能的支持，可以通过激活，指定参数等方式快速切换环境(开发环境，测试环境，产品环境)

  - 多profile文件形式：

    - 格式：application-{profile}.properties:
      - application.properties
      - application-dev.properties
      - application-prod.properties
    - 默认使用application.properties文件中的配置

  - 多profile文档块模式

    - 在application.yml文件中

    - 多个document使用---分隔

    - 在每个文档块中添加

      ```yml
      spring:
      	profiles: dev/prod
      ```

      

  - 激活方式：

    - 命令行 --spring.profiles.active=dev (在java命令后添加的参数)
    - 配置文件 spring.profiles.active=dev
    - jvm参数 -Dspring.profiles.active=dev

- #### 配置文件加载位置

  ![image-20200827221240365](D:\notes\随笔\note-imgs\image-20200827221240365.png)
  - /为resources目录或项目根目录
  - classpath指的是resources目录
  - 优先级从高到低，互补配置
  - 项目打包好以后，我们可以使用命令行参数的形式，启动项目的时候来指定配置文件的新位置，指定配置文件和默认加载的这些配置文件会共同起作用
    - java -jar 打包后的项目.jar --spring.config.location=配置文件绝对路径
    - 也可以--后直接加配置，并且可以一次加多个配置，用空格分开
  - 外部配置加载位置
    - ![image-20200827222450871](D:\notes\随笔\note-imgs\image-20200827222450871.png)
    - 优先级从高到低
    - 如果有属性重复，则高优先级覆盖低优先级

- #### 配置项目的访问路径（即根路径）

  - server.context.path=/path
  - 配置后项目中的资源访问根路径为/path

- #### 自动配置的原理

  - 所有配置
    - https://docs.spring.io/spring-boot/docs/current/reference/html/appendix-application-properties.html#core-properties
  - springboot启动时加载主配置类，开启了自动配置功能@EnableAutoConfiguration
    - 作用：
      - 利用EnableAutoConfigurationImportSelector给容器中导入一些组件

- #### debug=true

  - 开启springboot的debug模式

- #### SpringBoot与日志

  - 市面上的日志框架

    - JUL
    - JCL
    - Jboss-logging
    - logback
    - log4j
    - log4j2
    - slf4j

  - 关系图

    ![image-20200827233901717](D:\notes\随笔\note-imgs\image-20200827233901717.png)

  - 日志门面：抽象层

  - 左面选一个门面，右边来选一个实现

  - 日志门面选SLF4J,日志实现选Logback 

  #### 


```java
//P21 0:00 ----2020年8月27日23:44:51
```

- #### 在系统中使用SLF4J

  - 官方文档[http://www.slf4j.org/docs.html]

  - 给系统里面导入slf4j的jar和logback的实现jar

  - ```java
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    
    public class HelloWorld {
      public static void main(String[] args) {
        Logger logger = LoggerFactory.getLogger(HelloWorld.class);
        logger.info("Hello World");
      }
    }
    ```

  - 每一个日志的实现框架都有自己的配置文件，使用slf4j以后，配置文件还是做成日志实现框架自己本身的配置文件

  - 针对每个框架都会有自己的日志框架的不统一问题，解决方案是偷换jar包

    ![image-20200829091027882](D:\notes\随笔\note-imgs\image-20200829091027882.png)

  - 系统一点说，如何让系统中所有日志都统一到slf4j

    - 将系统中其他日志框架先排除出去
    - 用中间包来替换原有的日志框架
    - 我们导入slf4j其他的实现

  - 总结

    - springboot底层也是使用slf4j+logback的方式进行日志记录
    - springboot也把其他的日志都替换成了slf4j
    - 中间替换包

  - 我们如果要引入其他框架，一定要把这个框架的默认日志依赖移除掉

  - 日志使用

    demo

    ```java
    Logger logger = LoggerFactory.getLogger(getClass());
    
    //日志的级别由低到高
    //可以调整需要输出的日志级别,日志就只会在这个级别和以后的高级别生效
    //默认使用info级别
    //可以在配置文件调整，在application.properties中添加配置： logging.level.+包 = 级别
    logger.trace("这是trace日志....");
    logger.debug("这是debug日志....");
    logger.info("这是info日志....");
    logger.warn("这是warn日志....");
    logger.error("这是error日志....");
    ```

  - 配置logging.file=文件名:当前项目下生成指定文件名的日志文件
  - 配置logging.path=路径:在当前磁盘根目录下创建指定路径，可以多重，使用spring.log作为默认文件名
  - logging.pattern.console:在控制台输出的日志格式
  - logging.pattern.file:指定文件中日志输出的格式
  - 日志输出格式
    - %d 表示日期时间
    - %thread表示线程名
    - %-5level 级别从左显示5个字符宽度
    - %logger{50} 表示logger名字最长50个字符，否则按照句点分隔
    - %msg 表示日志消息
    - %n 换行符

- #### web开发

  - 创建一个springboot应用
  - 选中所有希望用到的模块
  - springboot已经默认将这些场景配置好了，只需要在配置文件中指定少量配置就可以运行起来
  - 自己编写业务代码

- #### springboot对静态资源的映射规则

  - 所以/webjars/**,都去classpath:/META-INF/resources/webjars/找资源

    - webjars: 以jar包的方式引入静态资源
      - www.webjars.org
      - 添加依赖
      - 从resources目录的下一级开始书写访问路径

  - "/**"访问当前项目的任何资源（静态资源的文件夹）

    - classpath:/META-INF/resources/
    - classpath:/resources/
    - classpath:/static/
    - classpath:/public/
    - /:当前项目的根路径

    - demo
      - 如果url为localhost:8080/aa
      - 如果没有程序处理这个请求路径，则会自动去上述的路径下寻找资源aa

  - 欢迎页；静态资源文件夹下的所有index.html页面；被"/**"映射

    - localhost:8080/

  - 所有的**/favicon.ico 都是在静态资源文件夹下找

  - 可以手动配置静态资源文件夹，例如：

    - spring.resources.static-locations=classpath:/hello/,classpath:/XXX/
    - 会屏蔽默认的静态资源文件夹

- #### 模板引擎

  - JSP
  - Velocity
  - Freemarker
  - Thymeleaf(*)

- #### Thymeleaf

  - 使用
    - 引入依赖[https://www.cnblogs.com/jpfss/p/11039382.html]
    
    - (非必要)使用thymeleaf3[https://blog.csdn.net/daqiang012/article/details/80234504]
    
    - 语法
      
      - 把html页面放在classpath:/template/中
      
    - (非必要,选择性配置)添加thymeleaf配置
    
      ```properties
      spring.thymeleaf.cache=false
      spring.thymeleaf.prefix=classpath:/templates/
      spring.thymeleaf.check-template-location=true
      spring.thymeleaf.suffix=.html
      spring.thymeleaf.encoding=UTF-8
      spring.thymeleaf.content-type=text/html
      spring.thymeleaf.mode=HTML5
      ```
    
    - 文档
      - [https://blog.csdn.net/zhangcc233/article/details/80831056]
      - [https://www.jianshu.com/p/4453368eb716]
    - 常用语法
      - 首先在html页面的<html>标签上添加属性
        - <html xmlns:th="http://www.thymeleaf.org"></html>
        - 作用是编写时有语法提示
      - th:text:改变当前元素里面的文本内容
      - 可以使用‘th:任意html属性’来代替原生属性

- #### 映射页面（用于模板引擎）

  - 在被@Controller注解的类中（控制器中）

  - 在上述类中，如果被@RequestMapping("/path")注解的方法如果没有被@ResponseBody注解，则返回的值会被映射成页面路由

  - demo

    ```java
    @RequestMapping("/path")
    public String toPath(){
        return "hahah";
    }
    //访问'/path'后，路由指向classpath:/templates/hahah.html
    ```

```java
//P30 0:00 ----2020年8月30日18:24:24
```

- #### 将数据传递给模板引擎

  - 在Controller类的路由方法添加Map<String,Object>类型的参数
  - 在方法体中向参数中添加键值对
  - return 模板名
  - 在模板里使用${key}取出数据（必须在th:前缀改造的html属性的值中使用，否则原样输出）

- #### Thymeleaf 中的*{...}

  - 有${...}的功能
  - 除此之外，还可以在拥有th:object=${...}属性的标签的子标签中使用
  - 附加功能：*{对象属性名}即可访问到希望得到的数据

- #### Thymeleaf中的#{...}

  - 用来获取国际化内容

- #### Thymeleaf中的@{...}

  - 用来定义URL

- #### Thymeleaf中的内嵌表达式

  - 如果想在标签体内部添加表达式
  - 则使用[[表达式]]的形式即可

- #### 定制错误响应

  - 定制错误页面
    - 有模板引擎
      - 在模板引擎目录下添加error目录，在error目录下添加错误响应页面
      - 页面命名规则为状态码.html
      - 或者命名为4xx.html或5xx.html 可以通配
      - 优先寻找精确命名
      - 页面中能获取的信息
        - timestamp:时间戳
        - status:状态码
        - error:错误提示
        - exception:异常对象
        - message:异常信息
        - errors:JSR303数据校验的错误都在这里
    - 没有模板引擎
      - 在静态资源文件夹下找
      - 命名规则同上
    - 也没有静态资源
      - 使用springboot默认的错误页面
  - 定制错误的json数据
    - 继承DefaultErrorAttributes类，并用@Component注解
    - 重写getErrorAttributes方法
    - 先调用super.getErrorAttibutes方法把原来的数据取出放到新map里，再向新map里面添加键值对

- #### 从请求参数中获取值

  - 在控制器的路由方法参数中添加参数
  - 将参数使用@RequestParam("参数名")注解
  - 在方法中使用即可

- #### @ControllerAdvice

  - 被注解的类为异常处理器

- #### @ExceptionHandler(类名.class)

  - 被注解的方法为异常处理方法

- #### 页面转发

  - 控制器的方法的返回值如果以forward:为前缀，则进行转发

- #### 处理错误时对客户端和浏览器的自适应

  - 将请求转发到/error
  - 有时会需要先设置一下状态码
  - /error会进行自适应

- #### 配置嵌入式Servlet容器

  - 可以修改和server有关的配置

  - 也可以编写一个EmbeddedServletContainerCustomizer(嵌入式的Servlet容器的定制器)

  - 在配置类中添加返回值类型为EmbeddedServletContainerCustomizer的方法，并用@Bean注解

  - 返回一个匿名对象

    ```java
    public EmbeddedServletContainerCustomizer embeddedServletContainerCustomizer(){
        return new EmbeddedServletContainerCustomizer(){
            public void customize(ConfigurableEmbeddedServletContainer container){
                //配置
                //例如：container.setPort(8083)
            }
        }
    }
    ```

- #### WebMvcConfigurerAdapter

  - 可以用来扩展SpringMVC的功能

- #### 注册Servlet/Filter/Listener

  - 三个RegistrationBean
    - ServletRegistrationBean
    - FilterRegistrationBean
    - ServletListenerRegistrationBean
  - 注册
    - 先编写一个Servlet/Filter/Listener
    - 在配置类中添加返回值类型为XXXRegistrationBean的方法，并用@Bean注解
    - 在方法体中创建一个类型为XXXRegistrationBean的新对象，构造函数参数按顺序为
      - 自己编写的Servlet/Filter/Listener对象
      - 匹配路径

- #### Docker

  - 是一个开源的应用容器引擎
  - 支持将软件编译成一个镜像，然后在镜像中各种软件做好配置，将镜像发布出去，其他使用者可以直接使用这个镜像，运行中的这个镜像称为容器，容器启动时非常快速的
  - 核心概念
    - Docker主机：安装了Docker程序的机器，Docker是直接安装在操作系统上的
    - Docker客户端：连接Docker主机进行操作
    - Docker仓库：用来保存各种打包好的软件镜像
    - Docker镜像：软件打包后的镜像，放在Docker仓库中
    - Docker容器：镜像启动后，就是一个容器，是独立运行的一个或一组应用
  - 使用步骤
    - 安装Docker
    - 去Docker仓库按照软件对应的镜像
    - 使用Docker运行这个镜像，这个镜像就会生成一个Docker容器
    - 对容器的启动停止就是对软件的启动停止

- #### 重启虚拟机网络

  - ```shell
    service network restart
    ```

- #### 查看linux系统ip地址

  - ```shell
    ip addr
    ```

- #### 数据访问

  - JDBC

    - 配置(yml)

      ```yml
      spring:
        datasource:
          username: root
          password: bighand
          url: jdbc:mysql://127.0.0.1:3306/newbase?serverTimezone=UTC&characterEncoding=utf8
          driver-class-name: com.mysql.cj.jdbc.Driver
          type: com.alibaba.druid.pool.DruidDataSource <!--切换数据源类型-->
          #-------------------------------------------------------------------------------------------
          initialSize: 5 <!--初始连接数-->
          minIdle: 5 <!--最小连接数-->
          maxActive: 20 <!--最大活动连接数-->
          maxWait: 60000 <!--最长等待时间(毫秒)-->
          timeBetweenEvictionRunsMillis: 60000
          minEvictableIdleTimeMillis: 300000
          validationQuery: SELECT 1 FROM DUAL
          testWhileIdle: true
          testOnBorrow: false
          testOnReturn: false
          poolPreparedStatements: true
      #   配置监控统计拦截的filters，去掉后监控界面sql无法统计，'wall'用于防火墙  
          filters: stat,wall,log
          maxPoolPreparedStatementPerConnectionSize: 20
          useGlobalDataSourceStat: true  
          connectionProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500
      ```

    - 数据库的配置都在DataSourceProperties里面
    
    - 上述代码块拦截线下的配置需要另行注入才能生效
      
      - 引入log4j依赖
        
        ```xml
        <!-- https://mvnrepository.com/artifact/log4j/log4j -->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>
        ```
        
      - 自定义一个配置类
      
      - 在类中添加被@Bean注解的方法，返回值类型为DataSource,但返回值需要是其实现类，例如DruidDataSource实例
      
      - 上述方法之上，需要加一层@ConfigurationProperties(prefix="spring.datasource")注解，作用是会将配置文件中的相关配置注入到属性中
      
    - 配置Druid的监控
      
      - 使用Druid数据源
        
        ```jxml
        <!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.1.8</version>
        </dependency>
        ```
        
        
        
      - 配置中必须有
      
        ```
        #   配置监控统计拦截的filters，去掉后监控界面sql无法统计，'wall'用于防火墙
        filters: stat,wall,log4j
        ```
      
      - 否则无法打开
      
      - 配置一个管理后台的Servlet
      
        - StatViewServlet：控制进入管理后台的Servlet
        - 配置时和普通Servlet一样配置（使用ServletRegistrationBean），但url映射必须为"/druid/*"
        - 在配置时可以修改默认配置（登录账户，密码等）
      
      - 配置一个监控的filter
        
        - WebStatFilter：用于控制Web和Druid数据源之间的管理关联监控统计
  
- #### 整合MyBatis

  - 涉及到的bean一定要按照规定将getter和setter写的规范，否则会出现很多错误
  - 属性名也要和数据库中的属性名对应

- #### MyBatis（注解版）

  - @Mapper

    - 用来注解类，说明这个类是一个操作数据库的mapper

      demo

      ```java
      @Mapper
      //指定这是一个操作数据库的mapper
      public interface DepartmentMapper {
      
          @Select("select * from department where id=#{id}")
          public Department getDepartmentById(Integer id);
      
          @Delete("delete from department where id=#{id}")
          public int deleteDepartment(Integer id);
      
          @Options(useGeneratedKeys=true,keyProperty="id") //一些选项开关
          												//设置@Options属性userGeneratedKeys的值为true，并指定实例对象中主键的属性													//名keyProperty以及在数据库中的字段名keyColumn。这样在gendar插入数据后，														//gendarId属性会被自动赋值。
          @Insert("insert into department(departmentName)")
          public int insertDepartment(Department department);
      
          @Update("update department set departmentName = #{departmentName} where id=#{id}")
          public int updateDepartment(Department department);
      }
      ```

    - 使用mapper

      demo

      ```java
      @RestController
      public class DeptController {
      
          @Autowired
          DepartmentMapper departmentMapper;//Mapper注入
      
          @GetMapping("/dept/{id}") //将url的一部分取出与下一行@PathVariable的参数对应
          public Department getDepartment(@PathVariable("id") Integer id){
              return departmentMapper.getDepartmentById(id);
          }
      
          @GetMapping("/dept")
          public Department insertDepartment(Department department){
              System.out.println(department);
              departmentMapper.insertDepartment(department);
              return department;
          }
      }
      ```

- #### MyBatis（配置版）

  - 自定义一个Mapper类AMapper，并使用@Mapper注解，或在主程序上添加注解@MapperScan("包名")

    ```
    @Mapper
    public interface EmployeeMapper {
        public Employee getEmployeeById(Integer id);
    }
    ```

  - 在类中自定义数据库操作方法

  - 在resources目录下（即classpath下）新建目录mybatis

  - 然后在mybatis目录下新建mybatis-config.xml文件

    ```xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <!DOCTYPE configuration
            PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
            "http://mybatis.org/dtd/mybatis-3-config.dtd">
    <configuration>
    
    </configuration>
    ```

  - 在mybatis目录下新建目录mapper

  - 在mapper目录下新建一个mapper配置文件对应自定义的Mapper类，例如AMapperConfig.xml

    ```xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <!DOCTYPE mapper
            PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
            "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
    <mapper namespace="com.example.mybatis.demo.mapper.EmployeeMapper"> //自定义Mapper类
        <select id="getEmployeeById" resultType="com.example.mybatis.demo.bean.Employee"> //此Mapper类对应的Bean
          SELECT * FROM employee WHERE id = #{id}
        </select>
    </mapper>
    ```

  - 将配置文件注册到项目配置文件中，例如application.yml

    ```
    mybatis:
      config-location: classpath:mybatis/mybatis-config.xml
      mapper-locations: classpath:mybatis/mapper/*.xml //可以使用通配符，也可以使用列表写法一一列出
    ```

- #### @AutoWired

  - 实例注入
  - 一般情况下
    - 无需显式为属性赋值，只需要为其添加@AutoWired注解即可使用
  - 被注解的类有子类时
    - 需要特别决定属性名来区分到底注入哪个子类
    - 

- #### @GetMapping是一个组合注解，是@RequestMapping(method = RequestMethod.GET)的缩写。

- #### @PostMapping是一个组合注解，是@RequestMapping(method = RequestMethod.POST)的缩写

- #### @PathVariable(key) 可以将url中的路径值注入到被注解的变量中 

  

```java
//P66 01:16 ----2020年9月1日18:05:21
```

- #### JPA

  - 创建项目时可引入依赖

  - 在application.yml文件中配置数据源

    ```yml
    spring:
      datasource:
        url: jdbc:mysql://localhost:3306/newbase?serverTimezone=UTC&characterEncoding=utf8
        driver-class-name: com.mysql.jdbc.Driver
        username: root
        password: bighand
    ```

  - 创建实体类

    ```java
    @Entity //表明这个类是一个实体类（和数据表映射的类）
    @Table(name = "tbl_user")//还可以指定各种规则，如果省略，默认表名就是user
    public class User {
    
        @Id //表明这是一个主键
        @GeneratedValue(strategy = GenerationType.IDENTITY)// 用于标注主键的生成策略，通过strategy 属性指定
        private Integer id;
    
        @Column(name = "last_name") //表明这是和数据表对应的一个列
        private String lastName;
    
        @Column //省略属性赋值，则列名默认是属性名
        private String email;
    
    
        public Integer getId() {
            return id;
        }
    
        public void setId(Integer id) {
            this.id = id;
        }
    
        public String getLastName() {
            return lastName;
        }
    
        public void setLastName(String lastName) {
            this.lastName = lastName;
        }
    
        public String getEmail() {
            return email;
        }
    
        public void setEmail(String email) {
            this.email = email;
        }
    }
    ```

  - 编写一个DAO接口来操作实体类对应的数据表（Repository）

    ```java
    public interface UserRepository extends JpaRepository<User,Integer> {
    }
    ```

  - 继续配置application.yml

    ```yml
    spring:
      datasource:
        url: jdbc:mysql://localhost:3306/newbase?serverTimezone=UTC&characterEncoding=utf8
        driver-class-name: com.mysql.jdbc.Driver
        username: root
        password: bighand
      jpa:
        hibernate:
          ddl-auto: update #自动更新数据表
        show-sql: true #控制台显示sql语句
    ```

  - *此时如果启动项目，此时如果数据库中没有这个数据表，springboot会自动创建表*

  - 大体配置完毕，使用：

    - 将respository注入

      ```java
      @AutoWired
      UserRepository userRepository;
      ```

    - 常用方法

      - userRepository.getOne(id)
      - User user = userRepository.save(user) //返回的user里会带有自增主键

  - com.fasterxml.jackson.databind.exc.InvalidDefinitionException异常解决方案

    - 在实体类上添加注解@JsonIgnoreProperties(value = { "hibernateLazyInitializer"})

- #### 缓存

  - JSR-107和springboot的缓存抽象的底层概念

    - ![image-20200902103000440](D:\notes\随笔\note-imgs\image-20200902103000440.png)
    - ![image-20200902103329646](D:\notes\随笔\note-imgs\image-20200902103329646.png)

  - 使用JSR-107需要导入

    - ![image-20200902103426377](D:\notes\随笔\note-imgs\image-20200902103426377.png)

  - springboot缓存抽象的概念和注解

    ![image-20200902103743986](D:\notes\随笔\note-imgs\image-20200902103743986.png)

  - 

- #### @MapperScan

  - ##### 作用：指定要变成实现类的接口所在的包，然后包下面的所有接口在编译之后都会生成相应的实现类
    添加位置：是在Springboot启动类上面添加

```java
//P75 0:00 ----2020年9月2日14:41:48
```

- #### 使用缓存

  - 开启基于注解的缓存
    
    - 在主类上标注@EnableCaching
  - 使用缓存注解标注
    - @Cacheable
      - 将方法的运行结果进行缓存，以后再用相同的数据，直接从缓存中获取，不用调用方法
      - 几个属性
        - value/cacheNames:指定缓存组件的名字，可以指定多个（必填项）
        
        - key: 用以指定缓存数据使用的key,默认为方法参数的值，可以使用spEL指定
        
          ![image-20200903085427337](D:\notes\随笔\note-imgs\image-20200903085427337.png)
        
        - keyGenerator:key的生成器，可以自己指定key的生成器组件id（和key互斥作用，只能选一个）
        
        - condition: 指定boolean类型值，只有为true时才缓存
        
        - unless:和condition相反，不同的是可以获取结果进行判断
        
        - sync:是否使用异步模式
      
    - @CacheEvict
    
      - 缓存清除
      - 用途：删除一个数据后，将对应的缓存也删除掉
      - 属性
        - value
        - key:按key清除
    
    - @CachePut
    
      - 既调用方法，又更新缓存数据，修改了数据库的某个数据，同时更新缓存
      - 运行时机：
        - 先调用方法
        - 再把方法的结果缓存起来
      - 用来更新缓存数据
      - 属性
        - value
        - key：指定需要更新的缓存id
    
    - @Caching
    
      - 前面几种的组合

```java
//P81 0:00 ----2020年9月3日09:39:18
```

- ##### new start---------------------------------------------------------------------------------------

- 