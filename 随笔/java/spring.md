- #### 体系结构

  - ![image-20200911164048307](D:\notes\随笔\note-imgs\image-20200911164048307.png)

- #### 程序的耦合

  - 耦合：程序间的依赖关系
    - 包括：
      - 类之间的依赖
      - 方法之间的依赖
  - 解耦：降低程序间的依赖关系
  - 实际开发中
    - 应该做到：编译器不依赖，运行时才依赖
  - 怎么解决
    - 创建对象时使用反射，避免使用new关键字
    - 通过读取配置文件获取要创建对象的全限定类名

- #### Bean

  - 可重用组件
  - JavaBean不等于实体类，而是大于实体类的范围，是使用java语言实现的可重用组件
  - spring中一个创建Bean对象的工厂，就是用来创建service和dao对象的
  
- #### Bean工厂

  - 需要一个配置文件来配置service和dao,配置的内容：唯一标识=全限定类名(key=value)
  - 通过读取配置文件中配置的内容，反射创建对象
  - 配置文件可以是xml或properties

```java
//P11 03:05 ----2020年9月11日17:50:38
```

- #### 路径问题

  - **xxx.class.getClassLoader().getResource(“”).getPath();** 
    **获取src资源文件编译后的路径（即classes路径）** 
  - **xxx.class.getClassLoader().getResource(“文件”).getPath();** 
    **获取classes路径下“文件”的路径** 

  - **xxx.class.getResource(“”).getPath()；**
    **缺少类加载器，获取xxx类经编译后的xxx.class路径** 

  - **this.getClass().getClassLoader().getResource(“”).getPath()；** 
    **以上三种方法的另外一种写法** 

  - **request().getSession().getServletContext().getRealPath(“”)；** 
    **获取web项目的路径** 

- ##### 在spring中读取properties文件

  - 使用xxx.class.getClassLoader().getResourceAsStream("文件名")的方式获取输入流

- #### IOC

  - 本质上是降低依赖关系

  - 将APP对资源的直接依赖解除，使用工厂模式等方式替代

  - ##### 为什么叫控制反转

    - 因为调用者本可以手握独立控制权决定要使用哪个具体的类，但它将决定权交给了工厂或框架决定
    - 简单来说就是因为控制权转移

  - 并非面向对象编程的专用术语

  - 包括依赖注入（DI）和依赖查找（Dependency Lookup）

  - 明确作用：削减计算机程序的耦合（解除代码中的依赖关系）

```java
//P17 03:25 ----2020年9月12日10:52:06
```

- ##### 引用文档地址：https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/index.html

- #### Spring的IOC

  - 帮助解耦

  - 导入spring依赖

  - 添加配置

    - 导入依赖

      ```xml
  <dependency>
          <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
          <version>5.2.0.RELEASE</version>
      </dependency>
      ```
      
    - 在resources目录（即classpath目录）下添加xml类型配置文件，名字随意，比如bean.xml
    
  - 添加约束和轮廓
    
    ```xml
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
              https://www.springframework.org/schema/beans/spring-beans.xsd">
      </beans>
    ```
    
    - 在beans中配置需要创建的对象

      ```xml
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
              https://www.springframework.org/schema/beans/spring-beans.xsd">
          <bean id="唯一标识" class="全限定类名" name="别名1,别名2,别名3..."></bean>
          <!--name属性可以起多个别名代替id-->
          <bean id="唯一标识" class="全限定类名"></bean>
          <bean id="唯一标识" class="全限定类名"></bean>
          ......
      </beans>
      ```
    
    - ##### 1.在应用中使用ApplicationContext创建容器
    
      ```java
      ApplicationContext context = new ClassPathXmlApplicationContext("bean.xml"); //根据配置文件创建容器
      UserDao dao = (UserDao) context.getBean("userdao");  //根据bean标签中的id创建或获取对应类名的对象
      UserService service = (UserService)context.getBean("userservice"); //同上
      //或UserDao dao = context.getBean("userdao",UserDao.class);
      //或UserService service = context.getBean("userservice",UserService.class);
      System.out.println(dao);
      System.out.println(service);
      ```
    
      - ApplicationContext的三个实现类
    
        - ClassPathXmlApplicationContext : 可以加载类路径下的配置文件
    
        - FileSystemXmlApplicationContext ：可以加载磁盘任一路径下的配置文件，前提是有访问权限
    
        - AnnotationConfigApplicationContext ：读取注解创建容器
    
        - ##### 以上三个都可以用作ApplicationContext对象的创建
    
    - ##### 2.在应用中使用BeanFactory创建容器
    
      ```java
      Resource resource = new ClassPathResource("bean.xml");
      BeanFacotry factory = new XmlBeanFactory(resource);
      UserDao dao = (UserDao) factory.getBean("userdao");
      ```
    
    - ##### ApplicationContext容器和BeanFactory容器的区别
    
      - ApplicationContext创建容器时，创建对象是默认立即创建的，读取配置文件之后就会立即创建对象
    
        - ##### 适用于单例对象
    
      - BeanFactory创建容器时，创建对象是延迟的，即getBean方法执行时才创建对象
    
        - ##### 适用于多例对象

- #### spring对bean的管理细节

  - 如何构造bean的三种配置方式

    - 第一种方式：使用默认构造函数构建对象，即配置文件中的bean标签只有id和class属性，并且没有子标签

      ```xml
      <bean id="唯一标识" class="全限定类名"></bean>
      ```

    - 第二种方式：使用普通工厂中的方法构建对象（使用某个类中的方法创建对象，并存入spring容器）

      ```xml
      <bean id="factory" class="工厂类全类名"></bean>
      <bean id="product" factory-bean="factory" factory-method="工厂类中需要使用的方法名"></bean>
      ```

    - 第三种方式：使用工厂中的静态方法构建对象（使用某个类中的静态方法创建对象，并存入spring容器）

      ```xml
      <bean id="product" class="工厂类全类名" factory-method="工厂类中的静态方法名"></bean>
      ```

      

  - bean对象的作用范围（使用bean标签的scope属性调整）

    - 作用：指定bean的作用范围
    - 取值：
      - singleton：单例（默认值）
      - prototype：多例的
      - request：作用于web应用的请求范围
      - session：作用于web应用的会话范围
      - global-session：作用于全局会话范围（集群环境）

  - bean对象的生命周期

    - 单例对象：
      - 伴随容器出生
      - 伴随容器活着
      - 伴随容器死亡
    - 多例对象
      - 需要使用时创建
      - 在被使用时一直活着
      - 长时间不被使用且没有被其他对象引用时，会被垃圾回收机制销毁

  - bean标签的其他属性
    - init-method:指定初始化时执行的方法名
    - destroy-method:指定对象销毁时执行的方法名

- #### 手动销毁容器

  - (ClassPathXmlApplicationContext)applicationContext.close();

- #### 依赖注入

  - 能注入的数据类型有三类：
    - 基本类型和String
    - 其他bean类型(在配置文件中或者注解配置过的bean)
    - 复杂类型/集合类型
  - 注入的方式有三种：
    - 使用构造函数提供

      - 在bean标签内部添加constructor-arg标签，属性有：

        - type:参数类型
        - index:参数位置
        - name:参数名
        - value:参数值
        - ref:引用别的bean赋值给当前属性，有ref就不再需要value

      - ##### 注入的值为集合类型

        ```xml
        <constructor-arg name="friends"> 
            <!--set也可以换成list或array-->
            <set>
                <value type="java.lang.String">大头儿子</value>
                <value type="java.lang.String">小头爸爸</value>
                <value type="java.lang.String">七个葫芦娃</value>
            </set>
        </constructor-arg>
        ```

      - ##### 注入的值为映射类型

        ```xml
        <constructor-arg name="grade">
           <!--value属性也可以单拉出来作为entry的子标签-->
            <map>
                <entry key="数学" value="100"></entry>
                <entry key="语文" value="100"></entry>
                <entry key="英语">
                	<value>100</value>
                </entry>
            </map>
        </constructor-arg>
        ```

    - 使用set方法提供

      - 在bean标签内部添加property标签，属性有：
        - name:属性名
        - value:属性值
        - ref：引用别的bean赋值给当前属性，有ref就不再需要value
      - set方法注入在构造方法注入之后，会发生覆盖
      - 注入集合的方式和使用构造方法注入无异

    - ```java
  //P30 06:12 ----2020年9月14日18:01:27
      ```

    - 使用注解提供
    
      - 用于创建对象的
    
        - @Component 
          
             - 作用：创建当前类的对象存入Spring容器中
          - 属性：
          - value:指定当前bean的id，如果缺省，则默认为当前类名的首字母小写形式
            - 只有@Component注解是不够的，还需要让spring知道哪些包下有被@Component注解的类
          - 新创建配置文件，例如 bean-ano.xml
          
        - 配置文件结构和上面的bean.xml类似，但要修改beans标签的命名空间
          
        - 使用<context:component>标签注册需要被扫描的包
          
        ```xml
          <?xml version="1.0" encoding="UTF-8"?>
          <beans xmlns="http://www.springframework.org/schema/beans"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xmlns:context="http://www.springframework.org/schema/context"
              xsi:schemaLocation="http://www.springframework.org/schema/beans
                  https://www.springframework.org/schema/beans/spring-beans.xsd
                  http://www.springframework.org/schema/context
                  https://www.springframework.org/schema/context/spring-context.xsd">
              <context:component-scan base-package="org.example"/>
          </beans>
        ```
        
          - 其他三个注解
    
            - @Controller：用于表现层
    
            - @Service：用于业务层
    
            - @Repository：用于持久层
    
            - ##### 性质
    
              - 作用和属性与@Component一模一样
              - 是spring提供的三层使用的注解，使三层更加清晰
    
      - 用于注入数据的
    
        - @Autowired
    
          - 作用：自动按照类型注入，只要容器中有唯一的一个bean对象类型和要注入的变量类型匹配，就可以注入成功
    
          - 出现位置：可以是变量上，也可以是方法上
    
          - 如果容器中有两个及以上个类型符合的bean,则根据被注解的变量名判断，如果其中有bean的id与此变量名相同，则注入，如果没有，则报错
    
          - ##### 使用注解注入时，set方法不再必须
    
        - @Qualifier
    
          - 作用：在按照类型注入 (@Autowired) 的基础上再按照名称注入
          - 给类成员变量注入时不能单独使用，但是在给方法参数注入时可以
          - 属性：
            - value:用于指定bean的id
    
        - @Resource
    
          - 作用：直接按照名称注入
          - 不再依赖于@Autowired,可以单独使用
          - 属性：
            - name:用于指定bean的id
    
        - ##### 以上三个注入都只能注入其他bean类型的数据，而基本类型和String类型都不行
    
        - ##### 集合类型只能用xml注入
    
        - @Value
    
          - 作用：用于注入基本数据类型和String类型的数据
          - 属性：
            - value:用于指定数据的值，可以使用spring中的SpEL表达式 **${表达式}**
    
        - 用于改变作用范围的
          - @Scope
            - 作用：用于指定bean的作用范围
            - 属性：
              - value : 指定范围的取值
                - 常用取值：
                  - singleton
                  - prototype
        - 和生命周期相关的
          - @PreDestroy：用于指定销毁方法
          - @PostContruct：用于指定初始化方法
  
- #### 使用注解代替bean.xml

  - @Configuration

    - 作用：指定当前类是一个配置类，替代bean.xml

    - ##### 当被注解的类作为AnnotationConfigApplicationContext构造函数的参数时，该注解可以不写

    - 如果没有上一步骤，则需要添加@Configuration并且将所在的包添加到主配置类的扫描范围内 (@ComponentScan)

    - 上一步骤也可以使用@Import取代

  - @ComponentScan("包名")

    - 作用：指定要扫描的包名
    - 属性：
      - basePackages: 用于指定要扫描的包名
      - value: 与basePackages相同
    - 使用位置：配置类上

  - @Bean

    - 在配置类中使用替代bean标签
    - 作用：将被注解方法的返回值作为bean添加到容器中
    - 属性：
      - name:用于指定此bean的id,如果缺省，则id为此方法的方法名
    - 被注解的方法的参数由容器提供

  - @Import

    - 在相对的主配置类上使用
    - 作用：使其他配置类（没有被@Configuration注解的也算）生效,可以取代以下操作 {{需要添加@Configuration并且将所在的包添加到主配置类的扫描范围内 (@ComponentScan)}}
    - @Import(配置类的类名.class)

  - @PropertySource

    - 作用：用于指定properties文件的路径，并将里面的键值对添加到容器中
    - 属性：
      - value: 指定文件路径（需要有一个classpath前缀，例如classpath:xxxconfig.properties）
    - 可以与@Value合作注解，将properties配置文件中的值注入，但是@Value的value属性值需要是SpEL表达式

  - ##### 如果使用注解替代bean.xml,则在初始化容器时，需要使用ApplicationContext的另一个实现: AnnotationConfigApplicationContext(配置类的类名.class)

```java
//P45 0:00 ----2020年9月15日11:45:17
```

- #### Spring整合Junit

  - 添加依赖

    ```xml
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.2.0.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.10</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-test</artifactId>
        <version>5.2.0.RELEASE</version>
        <scope>test</scope>
    </dependency>
    ```

  - @RunWith:使用Spring提供的main方法替换Junit自带的main方法

  - 为测试类添加注解：@RunWith(SpringJUnit4ClassRunner.**class)** 

  - 为测试类添加注解：@ContextConfiguration

    - 作用：指定配置
    - 属性
      - locations: 指定xml位置，加上classpath关键字，表示在类路径下
      - classes: 指定配置类
    
  - ##### 注意点：当spring版本大于5.x.x时，junit版本要求4.12及以上

- #### ThreadLocal

  - 程内部的存储类，可以在指定线程内存储数据，数据存储以后，只有指定线程可以得到存储数据
  - 参考博客[https://www.jianshu.com/p/3c5d7f09dfbd]

- #### 线程与连接绑定

  - 目的：为了防止数据操作过程中产生异常后数据一致性被破坏

  - 基本原理：将连接与线程绑定在一起，一套操作都是用一个连接（这样就可以使用一次事务处理）来完成，若完成则统一提交，否则统一回滚

  - 实现

    - 定义一个连接工具类

      ```java
      @Component
      public class ConnectionUtils {
      
          private ThreadLocal<Connection> tl = new ThreadLocal<Connection>();
      
          @Autowired
          private DataSource dataSource;
      
          /**
           * 获取当前线程上的连接
           */
          public Connection getConnection(){
              //1.先从当前线程上获取
              Connection conn = tl.get();
              //2.判断当前线程上是否有连接
              try {
                  if (conn == null) {
                      //3.从数据源中获取一个连接并且存入tl中，与线程绑定
                      conn = dataSource.getConnection();
                      //4.把连接存入tl
                      tl.set(conn);
                  }
              }
              catch(Exception e){
                  throw new RuntimeException(e);
              }
              //5.返回当前线程上的连接
              return conn;
          }
      
          /**
           * 线程与连接解绑
           */
          public void removeConnection(){
              tl.remove();
          }
      }
      ```

    - 定义一个事务管理类

      ```java
      @Component("txManager")
      public class TransactionManager {
      
          @Autowired
          private ConnectionUtils connectionUtils;
      
          /**
           * 开启事务
           */
          public void beginTransaction() {
              try {
                  connectionUtils.getConnection().setAutoCommit(false);//首先取消自动提交
              } catch (SQLException e) {
                  e.printStackTrace();
              }
          }
      
          /**
           * 提交
           */
          public void commit() {
              try {
                  connectionUtils.getConnection().commit();
              } catch (SQLException e) {
                  e.printStackTrace();
              }
          }
      
          /**
           * 回滚
           */
          public void rollback(){
              try {
                  connectionUtils.getConnection().rollback();
              } catch (SQLException e) {
                  e.printStackTrace();
              }
          }
      
          /**
           * 释放
           */
          public void release(){
              try {
                  connectionUtils.getConnection().close();
                  connectionUtils.removeConnection();
              } catch (SQLException e) {
                  e.printStackTrace();
              }
          }
      }
      ```

    - 在操作数据库（dao层）时，使用连接工具类提供的连接

      ```java
      public List<Account> findAllAccount() throws SQLException {
          return runner.query(connectionUtils.getConnection(),"select * from account",new BeanListHandler<Account>(Account.class));
      }
      ```

- #### 使用动态代理实现事务管理

  - ##### 建立在线程已与连接绑定的基础上

  - 建立工厂类

  - 聚合service对象

  - 注入service对象

  - 动态代理service对象

    - 在被代理对象的方法调用之前开启事务
    - 在被代理对象的方法调用之后提交事务
    - 若发生异常，则事务回滚
    - 释放事务

- #### AOP

  - 面向切面编程

  - 通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术，AOP是OOP的延续

  - 术语

    - JoinPoint: 连接点
      - 指那些被拦截到的点，在spring中，这些点指的是方法，因为spring只支持方法类型的连接点
    - Pointcut: 切入点
      - 是指我们要对哪些JointPoint进行拦截的定义，被拦截并增强的方法为切入点
    - Advice: 通知/增强
      - 是指拦截到JoinPoint之后所要做的事情就是通知
      - 通知类型：
        - 前置通知，后置通知，异常通知，最终通知，环绕通知
    - Introduction: 引介
      - 是一种特殊的通知
      - 在不修改类代码的前提下，可以在运行期为类动态地添加一些方法或Field
    - Target: 目标对象
      - 被代理的对象
    - Weaving: 织入
      - 把增强应用到目标对象来创建新的代理对象的过程
      - spring采用动态代理织入，而AspectJ采用编译器织入和类装载织入
    - Proxy: 代理
      - 一个类被AOP织入增强后，就产生一个结果代理类
    - Aspect: 切面
      - 是切入点和通知（增强）的结合

  - 导入依赖

    ```xml
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.2.4.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>org.aspectj</groupId>
        <artifactId>aspectjweaver</artifactId>
        <version>1.9.6</version>
    </dependency>
    ```
    
    ```java
    //P59 0:00 ----2020年9月15日18:05:43
    ```
    
    
    
  - 使用AOP时bean.xml的约束

    ```xml
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:aop="http://www.springframework.org/schema/aop"
           xsi:schemaLocation="http://www.springframework.org/schema/beans
            https://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/aop
            https://www.springframework.org/schema/aop/spring-aop.xsd">
    </beans>
    ```

  - bean.xml内关于aop的配置步骤

    - 把通知bean也交给spring来管理

    - 使用aop:config标签表明开始AOP的配置

    - 使用aop:aspect标签表明开始配置切面

      - id属性：给切面提供一个唯一标识
      - ref属性：是指定通知类bean的id

    - 在aop:aspect内部使用对应的标签来配置通知的类型

      - aop:before表示配置前置通知

        - method属性：用于指定Logger类中哪个方法是前置通知

        - pointcut属性：用于指定切入点表达式，该表达式的含义指的是业务层中哪些方法增强

          - 切入点表达式标准写法

            - 访问修饰符 返回值 包名.包名.包名...类名.方法名(参数列表)

          - 任意位置均可以被通配符替代，各位置通配符对应为

            - ```
              * *..*.*(..)
              ```

          - demo

            ```xml
            <aop:before method="pringLog" pointcut="execution(* *..*.*(..))"></aop:before>
            ```

            ```xml
            <aop:before method="pringLog" pointcut="execution(public void per.yjx.service.IAccountServiceImpl.saveAccount(int))"></aop:before>
            ```

            

      - aop:after-returning:配置后置通知，用法与前置相同

      - aop:after-throwing:配置异常通知，用法与前置相同

      - aop:after:配置最终通知，用法与前置相同

      - ##### 可以把pointcut属性单拉出来写成aop:pointcut标签，再在上述标签中使用point-ref属性通过id引用

        ```xml
        <aop:pointcut id="func" expression="execution(* *..*.*(..))"></aop:pointcut>
        ```

        - 还可以把aop:pointcut标签写到aop:aspect标签外，但必须在aop:pointcut标签前，否则违反约束

      - ##### aop:around：环绕通知，配置方法与前置相同

        - 注意点：
          - 当配置了环绕通知之后，切入点没有执行，而通知方法执行了
          - 解决方案：spring提供了ProceedingJoinPoint,该接口有一个方法proceed(),此方法相当于明确调用切入点方法。该接口可以作为**环绕通知的方法参数**，在程序运行时，spring框架会提供该接口的实现类为我们使用

  - 使用注解版的aop

    - 需要用到的注解

      - @Component ：将切面类对象加入容器
      - @Aspect：说明这个类是一个切面类
      - @Before: 指明前置通知方法
      - @AfterReturning: 指明后置通知方法
      - @AfterThrowing: 指明异常通知方法
      - @After: 指明最终通知方法
      - @PointCut("execution(切入点表达式)"): 注解在一个空方法上，此方法名+() 便可以被上述通知型注解作为参数代表此表达式

    - 在bean.xml中开启注解AOP的支持

      - ```xml
        <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
        ```

    - ##### 使用注解版的AOP会有一个bug,最终通知会跑到后置通知的前面去，使用时需注意!

- #### 配置类和配置文件混合使用（一起使用，同时出现在一个项目里）

  - 第一种：可以让@ContextConfiguration指向bean.xml,然后xml文件中添加标签指向配置类所在的包

    ```xml
    <context:component-scan base-package="per.config"></context:component-scan>
    ```

    - 个人理解：配置类也算是一种组件，上述标签其实就是spring扫描组件时使用的标签

  - 第二种：可以让@ContextConfiguration指向配置类，然后配置类上添加扫描注解@ComponentScan指向配置文件

  - 等等

```java
//P66 0:00 ----2020年9月16日17:12:48
```

- #### 持久层总图

  ![image-20200917084701635](D:\notes\随笔\note-imgs\image-20200917084701635.png)

- #### JdbcTemplate

  - spring框架提供的一个对象，是对原始JDBC API的一个简单封装

  - spring框架提供了很多操作模板类

    - 操作关系型数据的
      - JDBCTemplate
      - HibernateTemplate
    - 操作nosql数据库的
      - RedisTemplate
    - 操作消息队列的
      - JmsTemplate

  - 需要多导入的依赖

    - spring-jdbc-x.x.x.RELEASE.jar
    - spring-tx-x.x.x.RELEASE.jar
    - 数据库l驱动

  - 作用：

    - 用于和数据库交互，实现对表的CRUD操作

  - DriverManagerDataSource

    - spring的内置数据源
    - DataSource的子类

  - DataSource和JdbcTemplate对象可以使用容器配置和注入

  - 简单使用demo

    ```java
    DriverManagerDataSource ds = new DriverManagerDataSource();
    
    ds.setDriverClassName("com.mysql.jdbc.Driver");
    ds.setUrl("jdbc:mysql://localhost:3306/study_spring?serverTimezone=UTC&characterEncoding=utf8");
    ds.setUsername("root");
    ds.setPassword("bighand");
    
    JdbcTemplate jt = new JdbcTemplate();
    
    jt.setDataSource(ds);
    
    jt.execute("insert into account(name,money) values('hhh',1000)");
    ```

  - 查询方法（query）

    ![image-20200917095103930](D:\notes\随笔\note-imgs\image-20200917095103930.png)

  - query常用重载：

    - ![image-20200917095248955](D:\notes\随笔\note-imgs\image-20200917095248955.png)

    - demo

      ```java
      List<Account> accounts = jt.query("select * from account where id>? and money = ?",new RowMapper<Account>(){
          public Account mapRow(ResultSet resultSet, int i) throws SQLException {
              Account account = new Account();
              account.setId(resultSet.getInt("id"));
              account.setName(resultSet.getString("name"));
              account.setMoney(resultSet.getFloat("money"));
              return account;
          }
      },3,1000);
      ```

    - 或使用spring提供的RowMapper实现

      ```java
      List<Account> accounts = jt.query("select * from account where id>? and money = ?",new BeanPropertyRowMapper<Account>(Account.class),3,1000);
      ```

  - 其他方法 ：...

- #### Spring 事务控制的一组API(需要先导入spring-tx-xxx依赖)

  - PlatformTransactionManager

    - 事务管理器
    - 里面提供了常用的操作事务的方法
      - TransactionStatus getTransaction(TransactionDefinition definition) : 获取事务状态信息
      - void commit(TransactionStatus status) : 提交事务
      - void rollback(TransactionStatus status) : 回滚
    - 开发中使用的是它的实现类
      - org.springframework.jdbc.datasource.DataSourceTransactionManager: 使用JDBC或iBatis持久化数据时使用
      - org.springframework.orm.hibernate5.HibernateTransactionManager: 使用hibernate版本持久化数据时使用

  - TransactionDefinition

    - 事物的定义信息对象

    - 提供了查看事务信息的方法

      - String getName() : 获取事务名称

      - int getIsolationLevel() ： 获取隔离级别

        - ##### 隔离级别有以下取值

          ![image-20200917124718784](D:\notes\随笔\note-imgs\image-20200917124718784.png)

      - int getPropagationBehavior() ： 获取事务传播行为

        - ##### 事务传播行为取值

          ![image-20200917124822620](D:\notes\随笔\note-imgs\image-20200917124822620.png)

      - int getTimeout() : 获取事务超时时间

      - boolean isReadOnly() : 获取事务是否可读 

  - TransactionStatus

    - 提供事务具体的运行状态
      - void flush() : 刷新事务
      - boolean hasSavepoint() : 是否存在存储点
      - boolean isCompleted() : 是否完成
      - boolean isNewTransaction() : 是否为新的事务
      - boolean isRollbackOnly() : 是否回滚
      - void setRollbackOnly() : 设置事务回滚

- #### 在spring中使用事务

  - 基于XML的声明式事务控制配置步骤

    - 导入依赖

      - spring-tx
      - aspectjweaver
      - spring-jdbc
      - xxx-connector-java
      - commons-dbutils
      - spring-context
      - 。。。

    - 配置数据源

    - ##### dao的实现用JdbcTemplate,数据源都可

    - 配置事务管理器

      ```xml
      <bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
          <property name="dataSource" ref="dataSource"></property>
      </bean>
      ```

    - 配置事务的通知

      - 导入事务的约束[https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/data-access.html#spring-data-tier]

        ```xml
        <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xmlns:aop="http://www.springframework.org/schema/aop"
            xmlns:tx="http://www.springframework.org/schema/tx"
            xsi:schemaLocation="
                http://www.springframework.org/schema/beans
                https://www.springframework.org/schema/beans/spring-beans.xsd
                http://www.springframework.org/schema/tx
                https://www.springframework.org/schema/tx/spring-tx.xsd
                http://www.springframework.org/schema/aop
                https://www.springframework.org/schema/aop/spring-aop.xsd">
        </beans>
        ```

      - 添加标签

        ```xml
        <tx:advice id="txAdvice" transaction-manager="txManager">
                <tx:attributes>
                    <tx:method name="方法名" isolation="隔离级别" 
                               no-rollback-for="什么异常不回滚" propagation="传播方式" 
                               read-only="是否只读" rollback-for="什么异常会回滚" timeout="超时时间"/>
                </tx:attributes>
        </tx:advice>
        <!--id:给通知一个唯一标识-->
        <!--transaction-manager:给通知提供一个事务管理器引用-->
        <!--
        	推荐写法
        	<tx:advice id="txAdvice" transaction-manager="txManager">
                <tx:attributes>
                    <tx:method name="*" propagation="REQUIRED" read-only="false"/>
        			<tx:method name="find*" propagation="SUPPORTS" read-only="true"/>
                	//find* 可以匹配所有以find开头的方法
        		</tx:attributes>
        	</tx:advice>
        -->
        ```

    - 配置AOP中的通用切入点表达式

      ```xml
      <aop:config>
          <aop:pointcut id="pointcut" expression="execution(* *..*.*(..))"></aop:pointcut>
          <aop:advisor advice-ref="txAdvice" pointcut-ref="pointcut"></aop:advisor>
      </aop:config>
      ```

  - 基于注解的事务管理控制

    - 配置事务管理器

    - 开启spring对注解事务的支持

      ```xml
      <tx:annotation-driven transaction-manager="txManager"></tx:annotation-driven>
      ```

    - 在需要事务支持的地方使用@Transactional注解（一般是在业务层实现类或具体方法上添加）

    - ##### @EnableTransactionManagement: 在配置类上此注解添加也可以开启spring对注解事务的支持

- ##### 小技巧

  - 导包时导入spring-webmvc就会把基础jar包一起导进来

- ##### bean的别名

  - ```xml
    <alias name="bean的原id" alias="bean的别名"></alias>
    ```

  - 配置后通过context使用新名也可以获取到想要的bean
  
- ##### import标签

  - 用于团队开发时将多个beans配置文件合并为一个
  
- ##### p命名空间注入

  - 使用p命名空间，可以在不使用property标签的情况下注入属性

  - 步骤

    - 引入命名空间

    - 使用

    - ```xml
      <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:p="http://www.springframework.org/schema/p"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          https://www.springframework.org/schema/beans/spring-beans.xsd">
      	<!---xmlns:p="http://www.springframework.org/schema/p"-->
          <bean id="myDataSource" class="org.apache.commons.dbcp.BasicDataSource"
              destroy-method="close"
              p:driverClassName="com.mysql.jdbc.Driver"
              p:url="jdbc:mysql://localhost:3306/mydb"
              p:username="root"
              p:password="misterkaoli"/>
      
      </beans>
      ```

      

- ##### c命名空间注入

  - 引入命名空间

  - 使用(前提是该bean必须有有参构造器)

  - ```xml
    <beans xmlns="http://www.springframework.org/schema/beans"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns:c="http://www.springframework.org/schema/c"
        xsi:schemaLocation="http://www.springframework.org/schema/beans
            https://www.springframework.org/schema/beans/spring-beans.xsd">
    	<!-- xmlns:c="http://www.springframework.org/schema/c"-->
        <bean id="beanTwo" class="x.y.ThingTwo"/>
        <bean id="beanThree" class="x.y.ThingThree"/>
    
        <!-- traditional declaration with optional argument names -->
        <bean id="beanOne" class="x.y.ThingOne">
            <constructor-arg name="thingTwo" ref="beanTwo"/>
            <constructor-arg name="thingThree" ref="beanThree"/>
            <constructor-arg name="email" value="something@somewhere.com"/>
        </bean>
    
        <!-- c-namespace declaration with argument names -->
        <bean id="beanOne" class="x.y.ThingOne" c:thingTwo-ref="beanTwo"
            c:thingThree-ref="beanThree" c:email="something@somewhere.com"/>
    
    </beans>
    ```

- ##### bean的作用域

  - ![image-20201214182937243](D:\notes\随笔\note-imgs\image-20201214182937243.png)
  
- ##### bean的自动装配

  - 自动装配是spring满足bean依赖的一种方式

  - spring会在上下文中自动寻找并自动给bean装配属性(autowire="byType/byName")

  - ```xml
    <bean id="xxx" class="xxx.xxx....Xxx" autowire="byType/byName">
    	<property name="xxx" value="xxx"></property>
        ...
    </bean>
    ```

  - byName需要属性名和目标bean的id相同

  - byType需要属性类型和bean的类型相同，且类型唯一

- ##### 使用注解注入

  - 导入注解的约束

    - (xmlns:context="http://www.springframework.org/schema/context")

    - (xsi:schemaLocation:"http://www.springframework.org/schema/context
              https://www.springframework.org/schema/context/spring-context.xsd")

    - ```xml
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
              https://www.springframework.org/schema/beans/spring-beans.xsd
              http://www.springframework.org/schema/context
              https://www.springframework.org/schema/context/spring-context.xsd">
      
          <context:annotation-config/>
      
      </beans>
      ```

  - #### 

