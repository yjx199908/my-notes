#### 三层架构

- 表现层

  - 用于展示数据

- 业务层

  - 处理业务需求

- 持久层

  - 是和数据库交互的

- 图解

  ![image-20200904172608885](D:\notes\随笔\note-imgs\image-20200904172608885.png)

- #### 使用ORM思想实现了结果集的封装

  - ORM:
    - Object Relational Mapping 对象关系映射
    - 就是把数据库表和实体类及实体类的属性对应起来
    - 让我们可以操作实体类就操作数据库表

- #### \<packaging\>jar\</packaging\>

  - 打包方式

- #### 创建项目

  - 创建空maven项目

  - 添加依赖

    ```xml
    <dependencies>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.5</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.21</version>
        </dependency>
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.10</version>
        </dependency>
    </dependencies>
    ```

    

```java
//P5 06:55 ----2020年9月4日17:58:18
```

- #### 环境搭建

  - 创建实体类

    - 在java目录下创建
    - 必须实现Serializable接口
    - 实体类的属性和表对应
    - get/set和toString

  - 创建DAO接口（数据访问接口）

    - 在java目录下创建
    - 在里面编写用于数据访问的抽象方法

  - 在resources目录下创建主配置文件

    - 文件名叫什么都可以

    - 通常叫做SqlMapConfig.xml

    - 添加约束

      ```xml
      <?xml version="1.0" encoding="UTF-8"?>
      <!DOCTYPE configuration
              PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
              "http://mybatis.org/dtd/mybatis-3-config.dtd">
      ```

    - 添加主配置

      ```xml
      <configuration>
          <!--配置环境-->
          <environments default="mysql">
              <!--配置mysql的环境-->
              <environment id="mysql">
                  <!--配置事务类型-->
                  <transactionManager type="JDBC"></transactionManager>
                  <!--配置数据源（连接池）-->
                  <dataSource type="POOLED">
                      <!--配置连接数据库的四个基本信息-->
                      <property name="driver" value="com.mysql.jdbc.Driver"/>
                      <property name="url" value="jdbc:mysql://localhost:3306/newbase?serverTimezone=UTC"/>
                      <property name="username" value="root"/>
                      <property name="password" value="bighand"/>
                  </dataSource>
              </environment>
              
               <!--可以配置多套环境，使用environment标签的default属性调用-->
              <environment id="mysql">
                ...
              </environment>
          </environments>
      
          <!--指定映射配置文件的位置，映射配置文件指的是每个dao独立的配置文件-->
          <!--使用配置文件式时，mapper标签需要resource属性，而使用注解版mybatis时，则使用class属性代替-->
          <!--resource属性的值为映射配置文件相对resources目录的位置-->
          <!--class属性的值为dao的全限定类名-->
          <mappers>
              <mapper resource="entities/dao/IUserDao.xml"></mapper>
          </mappers>
      </configuration>
      ```

    - 在resources下创建每个dao的映射配置文件（如果使用注解版则忽略本步骤）

      - ##### 注意事项：映射配置文件位置（以resources目录为根目录）必须和对应的dao的包结构相同

      - 根据主配置文件中自己配置的映射文件位置来创建路径和文件

      - 添加映射配置文件约束

        ```xml
        <!DOCTYPE mapper
            PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
            "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
        ```

      - 添加映射

        ```xml
        <mapper namespace="entities.dao.IUserDao">
            <!--配置数据库操作-->
            <!--select/update/delete/insert等标签的id必须是对应的dao接口中的方法名-->
            <select id="findAll" resultType="entities.f.User"> <!--resultType为指定返回的结果集的每一个元素要封装到哪个实体类中--> 
                select * from user;
            </select>
            <!-- ... -->
        </mapper>
        ```

  - 对于以上步骤的总结图

    ![image-20200907092231832](D:\notes\随笔\note-imgs\image-20200907092231832.png)

- ##### 在mybatis中把持久层的操作接口名称和映射文件也叫做: Mapper

- #### 使用log4j

  - 将log4j.properties文件拷贝到resources目录下

- #### 创建测试类

  - 在test/java目录下创建一个类
  - 在类中添加main方法
  - 在main方法中
    - 读取配置文件
    
    - 创建SqlSessionFactory工厂
    
    - 使用工厂创建一个SqlSession对象
    
    - 使用SqlSession对象创建dao接口的代理对象
    
    - 使用代理对象执行方法
    
    - 释放资源
    
    - ```java
      public static void main(String[] args) throws IOException {
          // 读取配置文件
          InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");
          // 创建SqlSessionFactory工厂
          SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
          SqlSessionFactory factory = builder.build(in);
          // 使用工厂创建一个SqlSession对象
          SqlSession session = factory.openSession();
          // 使用SqlSession对象创建dao接口的代理对象
          IUserDao dao = session.getMapper(IUserDao.class);
          // 使用代理对象执行方法
          List<User> users = dao.findAll();
          for(User user:users){
              System.out.println(user);
          }
          // 释放资源
          session.close();
          in.close();
      }
      ```

```java
//P8 03:35 ----2020年9月7日15:07:16
```

- #### 代码分析

  - ```java
    public static void main(String[] args) throws IOException {
        // 读取配置文件
        InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");//路径问题，开发时一般使用：
        																		//1.类加载器,它只能读取类路径的配置文件
        																		//2.使用ServletContext对象的getRealPath()方法
        // 建造SqlSessionFactory工厂
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        SqlSessionFactory factory = builder.build(in);
        // 使用工厂生产一个SqlSession对象
        SqlSession session = factory.openSession();
        // 使用SqlSession对象创建dao接口的代理对象
        IUserDao dao = session.getMapper(IUserDao.class);
        // 使用代理对象执行方法
        List<User> users = dao.findAll();
        for(User user:users){
            System.out.println(user);
        }
        // 释放资源
        session.close();
        in.close();
    }
    ```

- #### 使用注解开发

  - 环境搭建同上

  - 在主配置文件中的mapper标签上添加属性class,值为dao类全限定类名

  - 在dao的方法名上添加注解

    ```java
    @Select("select * from user")
    List<User> findAll();
    ```

  - 创建测试类同上

- ##### mybatis支持自定义dao实现，但意义不大，且实际开发中很少使用

```java
//P11 09:45 ----2020年9月8日11:45:24
```

- #### dao中的方法传参

  - 使用映射配置时
    - 在mapper标签中添加属性parameterType,值为参数的全限定类名
    - 在mapper标签内部使用**#{属性名}**的方式取数据（严格来说是getter方法的名称改造版，和属性名并无关系）

- ##### 在测试类中，@Before注解的方法会在@Test注解的方法执行之前执行，@After注解的方法会在@Test注解的方法之后执行

- ##### xml文件转义字符对照表

  ![image-20200908170747736](D:\notes\随笔\note-imgs\image-20200908170747736.png)

- ##### 如果需要执行插入数据操作，则执行完毕后需要调用session的commit()方法提交操作

- ##### \<delete\>标签的parameterType属性的值如果是一个基本数据类型或者基本数据类型的包装类，则标签内的#{}内的属性名可以随意写

```java
//P24 01:19 ----2020年9月8日18:10:39
```

- #### 模糊查询

  - 如果使用#{}，则不可以把百分号写在映射配置文件里
  - 还可以使用${value},但value这个名称是写死的，使用这种占位符则可以使用'%${value}%'的形式模糊占位，但这种占位使用的是字符串的拼接

- #### 插入操作后取出自增id

  - 映射配置方法

    - 在insert标签内部添加子标签\<selectKey\>标签

      ```xml
      <selectKey resultType="int" keyColumn="id" keyProperty="id" order="AFTER">
          select last_insert_id()
      </selectKey>
      ```

- #### OGNL表达式

  - 在映射配置文件中使用

  - 可以把 **object.property** 的形式内部转化为 **object.getProperty()** 的形式，且如果insert/select/update/delete标签的parameterType已经指定了类名，则可以直接**#{property}**

  - 例如

    ```xml
    <insert parameterType="entities.User" id="insertUser">
    	insert into user(username,sex) values(#{username},#{sex})
        <!--其中#{username} 会被内部转化为 user.getUsername(),#{sex} 转化为 user.getSex()-->
    </insert>
    ```

  - 如果property也是一个对象，则对于它的属性可以这样调用，以此类推，可以一直链式调用下去

- #### 实体类属性名和数据表内列名不一样

  - 增删改可以通过修改OGNL表达式对应

  - 查操作可以采用

    - 起别名

    - 还可以在mapper标签下配置结果映射

      ```xml
      <!--id属性可以随便写,但不可以重复.type属性值为被对应的实体类的全限定类名-->
      <resultMap id="userMap" type="entities.User">
          <!--主键字段的对应-->
      	<id property="userId" column="id"></id>
          
          <!--其他字段的对应-->
          <result property="userName" column="username"></result>
          <result property="userSex" column="sex"></result>
          <result property="userAddress" column="address"></result>
      </resultMap>
      ```

      - 需要使用时，需要将select标签的resultType属性修改为resultMap属性，值为对应的resultMap的id值

- #### SqlMapConfig主配置文件中使用properties标签

  - 在configuration标签下其他标签之前添加properties标签

  - 在properties标签中添加多个property标签，其中包含name属性和value属性

  - 在下面的标签中可以使用${property的id属性值}的方式引用之前定义的对应name的property标签的value值

  - demo

    ```xml
    <configuration>
        <properties>
            <property name="driver" value="com.mysql.jdbc.Driver"/>
            <property name="url" value="jdbc:mysql://localhost:3306/newbase?serverTimezone=UTC"/>
            <property name="username" value="root"/>
            <property name="password" value="bighand"/>
        </properties>
        <!--配置环境-->
        <environments default="mysql">
            <!--配置mysql的环境-->
            <environment id="mysql">
                <!--配置事务类型-->
                <transactionManager type="JDBC"></transactionManager>
                <!--配置数据源（连接池）-->
                <dataSource type="POOLED">
                    <!--配置连接数据库的四个基本信息-->
                    <!--${...}引用上方定义过的property标签-->
                    <property name="driver" value="${driver}"/>
                    <property name="url" value="${url}"/>
                    <property name="username" value="${username}"/>
                    <property name="password" value="${password}"/>
                </dataSource>
            </environment>
        </environments>
    
        ......
        
    </configuration>
    ```

  - ##### 也可以把属性配置单拉出来写在外部配置文件中，在SqlMapConfig.xml中使用propertyies标签的resource属性或url属性引用外部配置文件

    - 如果使用resource属性，则路径要以类路径（resources目录)为参考

    - 外部配置文件可以是properties后缀的

      - 外部配置文件格式

        ```properties
        driver=com.mysql.jdbc.Driver
        url=jdbc:mysql://localhost:3306/newbase?serverTimezone=UTC&characterEncoding=utf8
        username=root
        password=bighand
        ```

    - ${}引用时，属性名必须一致

- #### URL和URI

  - URL
    - 统一资源定位符，可以在整个网络定义一个资源
    - 由协议、域名（或ip）、端口、URI组成
  - URI
    - 统一资源标识符，可以在一个应用中标识一个资源

- #### 在配置文件中给domain层的类配置别名

  - 在主配置文件（SqlMapConfig.xml）的configuration标签中添加标签typeAliases标签

    - 使用typeAlias子标签

      ```xml
      <typeAliases>
          <typeAlias type="entities.User" alias="user"></typeAlias>
      </typeAliases>
      ```

      - 配置完别名后，在映射配置文件中可以使用别名代替这个类的类名，且别名不区分大小写

    - 或者使用package标签

      ```xml
      <typeAliases>
          <package name="entities"></package>
      </typeAliases>
      ```

      - 使用package标签配置完后，被配置的包下所有的类都具有了别名，即类名，且不区分大小写,但如果类上有@Alias('...')注解，则别名为注解值

    - package标签扩展

      - 在mappers标签下使用package标签，可以不用挨个指定每个mapper的位置，mybatis会去指定的包内查找

```java
//P40 0:37 ----2020年9月9日14:13:45
```

- #### mybatis连接池

  - mybatis连接池提供了三种方式的配置
    - 配置的位置：
      - 主配置文件SqlMapConfig.xml中的dataSource标签，其type属性就是表示采用何种连接池方式
        - 取值有
          - POOLED：采用传统的javax.sql.DataSource规范中的连接池，mybatis中有针对其的实现
          - UNPOOLED：采用传统的获取连接的方式，虽然也实现了javax.sql.DataSource接口，但是并没有池的思想
          - JNDI：采用服务器提供的JNDI技术实现，来获取DataSource对象，不同服务器所能拿到的DataSource是不一样的
            - 注意：如果不是web或者maven的war工程，是不能使用的
            - tomcat采用的是dbcp连接池

- #### mybatis中的事务

  - 通过sqlsession对象的commit方法和rollback方法实现事务的提交和回滚
  - openSession(true)可以创建自动提交的会话

- 映射文件中的动态sql语句

  - 使用if标签动态改变sql语句，且可以使用多个

    - ```xml
      <select id="findAll" resultType="entities.User" parameterType="entities.User">
          select * from user
          <if test="username!=null">
              where username=#{username}
          </if>
      </select>
      ```

    - ```xml
      <select id="findAll" resultType="entities.User" parameterType="entities.User">
          select * from user
          <if test="username!=null">
              where username=#{username}
          </if>
          <if test="sex!=null">
          	and sex=#{sex}
          </if>
      </select>
      ```

  - 也可以使用where标签

    - mybatis会将where标签及其内容翻译为where子句

    - ```xml
      <select id="findAll" resultType="entities.User" parameterType="entities.User">
          select * from user
          <where>
            <if test="username!=null">
                username=#{username}
            </if>
            <if test="sex!=null">
                sex=#{sex}
            </if>
          </where>
      </select>
      ```

```java
//P46 0:00 ----2020年9月11日14:59:38
```

- ##### 三个核心类

  - SqlSessionFactoryBuilder：会话工厂建造者
  - SqlSessionFactory：会话工厂类
  - SqlSession：会话

- ##### SqlSession对象应该是多例的，因为它是线程不安全的，应该使其作用域为方法作用域或请求作用域

- ##### 修改数据后需要提交才会生效

- ##### 默认事务管理器：JDBC,默认的连接池：POOLED

- ##### 开启设置

  - mapUnderscoreToCamelCase：设置是否开启驼峰命名规则映射，即从经典数据库列名A_COLUMN到经典Java属性名aColumn的类似映射
  - logImpl：指定MyBatis所用日志的具体实现，未指定时将自动查找
  - cacheEnabled：全局开启或关闭配置文件中的所有映射器已经配置的任何缓存
  - lazyLoadingEnabled：延迟加载的全局开关，当开启时，所有关联对象都会延迟加载。特定关联关系中可通过设置fetchType属性来覆盖该项的开关状态。

- ##### 映射器：

  - 注册绑定Mapper文件

    - 方式一：使用resource属性

      ```xml
      <mappers>
          <mapper resource=".../xxx.xml"></mapper?>
      </mappers>
      ```

    - 方式二：使用class属性

      ```xml
      <mappers>
          <mapper class="xx.xxx.Xxx"></mapper?>
      </mappers>
      ```

      - 注意点：接口和Mapper配置文件必须同包同名

    - 方式三：使用包扫描进行注入绑定：要求同上

- ##### SqlSessionFactory工厂全局拥有一个就足够

- ##### SqlSession需要有多个，且每次用完必须关闭！

- ##### 属性名和字段名不一致的问题

  - 结果集映射

    - 在mapper配置文件的mapper标签中添加resultMap标签

      ```xml
      <mapper>
          <resultMap id="UserMap" type="User">
          	<!--<result column="id" property="id"/>-->如果字段名和属性名一样，则不需要写
              <!--<result column="name" property="name"/>-->
              <result column="password" property="pwd"/> //column为字段名，property为属性名
              ...
          </resultMap>
          <select id="findAll" resultMap="UserMap">
          	select * from user where id=#{id}
          </select>
      	...
      </mapper>
      ```

      

  

- ##### 日志

  - 日志工厂

    - 如果一个数据库操作出现了异常，我们需要排错，日志就是最好的助手

    - 可以在设置中指定Mybatis所用日志的具体实现，未指定时将自动查找

      ```xml
      <settings>
          ...
      	<setting name="logImpl" value="STDOUT_LOGGING"></setting>
          ...
      </settings>
      ```

      - ##### 标准工厂输出日志(STDOUT_LOGGING)不需要引入依赖，其他的日志实现需要

  - log4j的使用

    - 先导入依赖

      ```xml
      <dependency>
          <groupId>log4j</groupId>
          <artifactId>log4j</artifactId>
          <version>1.2.17</version>
      </dependency>
      ```

    - 在resources目录下添加文件：log4j.properties

      ```properties
      #############
      # 输出到控制台
      #############
      
      # log4j.rootLogger日志输出类别和级别：只输出不低于该级别的日志信息DEBUG < INFO < WARN < ERROR < FATAL
      # WARN：日志级别     CONSOLE：输出位置自己定义的一个名字       logfile：输出位置自己定义的一个名字
      log4j.rootLogger=WARN,CONSOLE,logfile
      # 配置CONSOLE输出到控制台
      log4j.appender.CONSOLE=org.apache.log4j.ConsoleAppender 
      # 配置CONSOLE设置为自定义布局模式
      log4j.appender.CONSOLE.layout=org.apache.log4j.PatternLayout 
      # 配置CONSOLE日志的输出格式  [frame] 2019-08-22 22:52:12,000  %r耗费毫秒数 %p日志的优先级 %t线程名 %C所属类名通常为全类名 %L代码中的行号 %x线程相关联的NDC %m日志 %n换行
      log4j.appender.CONSOLE.layout.ConversionPattern=[frame] %d{yyyy-MM-dd HH:mm:ss,SSS} - %-4r %-5p [%t] %C:%L %x - %m%n
      
      ################
      # 输出到日志文件中
      ################
      
      # 配置logfile输出到文件中 文件大小到达指定尺寸的时候产生新的日志文件
      log4j.appender.logfile=org.apache.log4j.RollingFileAppender
      # 保存编码格式
      log4j.appender.logfile.Encoding=UTF-8
      # 输出文件位置此为项目根目录下的logs文件夹中
      log4j.appender.logfile.File=logs/root.log
      # 后缀可以是KB,MB,GB达到该大小后创建新的日志文件
      log4j.appender.logfile.MaxFileSize=10MB
      # 设置滚定文件的最大值3 指可以产生root.log.1、root.log.2、root.log.3和root.log四个日志文件
      log4j.appender.logfile.MaxBackupIndex=3  
      # 配置logfile为自定义布局模式
      log4j.appender.logfile.layout=org.apache.log4j.PatternLayout
      log4j.appender.logfile.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %F %p %m%n
      
      ##########################
      # 对不同的类输出不同的日志文件
      ##########################
      
      # club.bagedate包下的日志单独输出
      log4j.logger.club.bagedate=DEBUG,bagedate
      # 设置为false该日志信息就不会加入到rootLogger中了
      log4j.additivity.club.bagedate=false
      # 下面就和上面配置一样了
      log4j.appender.bagedate=org.apache.log4j.RollingFileAppender
      log4j.appender.bagedate.Encoding=UTF-8
      log4j.appender.bagedate.File=logs/bagedate.log
      log4j.appender.bagedate.MaxFileSize=10MB
      log4j.appender.bagedate.MaxBackupIndex=3
      log4j.appender.bagedate.layout=org.apache.log4j.PatternLayout
      log4j.appender.bagedate.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %F %p %m%n
      ```

      

- ##### 内建的一些类型别名[https://mybatis.org/mybatis-3/zh/configuration.html#typeAliases]

- ##### 使用注解开发时，如果方法有多个参数且没有被组合为一个对象，则需要在每个参数前添加注解@Param('参数名')，

- ##### 注解开发时的sql语句和配置文件开发时的sql语句都可以使用EL表达式

- ##### 注解开发时事务默认自动提交

- ##### #{}和${}区别：

  - #{}为预编译，${}为字符串拼接
  - $一般用于传入数据库对象，例如数据库表

- ##### Lombok

  - idea插件

  - 自动生成bean的重要方法（setter,getter,equals等等）

  - 在setting菜单下的plugins选项中添加

  - 使用

    - 在项目中导入lombok的依赖

      ```xml
      <dependency>
          <groupId>org.projectlombok</groupId>
          <artifactId>lombok</artifactId>
          <version>1.18.12</version>
      </dependency>
      ```

    - 在bean或bean的属性上添加注解

- ##### 多对一的关系

  - 按照查询嵌套处理

    - 使用association标签

    - 将association的javaType属性设置为需要映射成的结果类型

    - 将association的select属性设置为本mapper中其他查询的id

    - ```xml
      <select id="getStudent" resultMap="StudentTeacher">
      	select * from student
      </select>
      <resultMap id="StudentTeacher" type="Student">
      	<result property="id" column="id"/>
          <result property="name" column="name"/>
          <association property="teacher" column="tid" javaType="Teacher" select="getTeacher" />
      </resultMap>
      <select id="getTeacher" resultType="Teacher">
      	select * from teacher where id=#{id}
      </select>
      ```

  - 按照结果嵌套处理

    - 使用association标签

    - 将association的javaType属性设置为需要映射成的结果类型

    - 在association标签内部添加result标签进行内层映射

    - ```xml
      <select id="getStudent" resultMap="StudentTeacher">
      	select s.id id,s.name name,t.name tname
          from student s,teacher t
          where s.tid = t.tid
      </select>
      <resultMap id="StudentTeacher" type="Student">
      	<result property="id" column="id"/>
          <result property="name" column="name"/>
          <association property="teacher" javaType="Teacher">
          	<result property="name" column="tname" />
          </association>
      </resultMap>
      ```

- ##### 一对多的关系

  - 按照结果嵌套处理

    - 使用collection标签

    - 将collection的ofType属性设置为需要映射成的集合的元素类型

    - 在collection标签内部添加result标签进行内层映射

    - ```xml
      <select id="getTeacher" resultMap="TeacherStudent">
      	select s.id sid,s.name sname,t.name tname,t.id tid
          from student s,teacher t
          where s.tid=t.id and t.id=#{id}
      </select>
      <resultMap id="TeacherStudent" type="Teacher">
      	<result property="id" column="tid"/>
          <result property="name" column="tname"/>
          <collection property="students" ofType="Student">
          	<result property="id" column="sid"/>
              <result property="name" column="sname"/>
              <result property="tid" column="tid"/>
          </collection>
      </resultMap>
      ```

  - 按照查询嵌套处理

    - 使用collection标签
    - 设置collection标签的属性
      - property设置为需要被映射成的成员名
      - javaType设置为ArrayList
      - ofType属性设置为需要映射成的集合的元素类型
      - select设置为另一查询的id
      - column设置为可以作为另一查询的查询条件的本次查询结果集列名

```java
//P22
```

- ##### where标签

  - 在映射文件中使用

  - 作用：

    - 自动判定此次sql需不需要where子句（根据内部的if标签判定结果）
    - 内部的if标签的内容，如果是第一个判定为需要嵌入sql语句的，则自动将头部的and关键字去除，如果本来就没有则直接下一步
    - 内部的if标签的内容，如果不是第一个判定为需要嵌入sql语句的，则自动在头部添加and关键字，本来就有则无需此步

  - ```xml
    <select id="findMusic" resultType="cam.pojo.Music" parameterType="cam.pojo.Music">
        select * from music
        <where>
            <if test="id > 0">
              id=#{id}
            </if>
            <if test="name != null">
              and name = #{name}
            </if>
            <if test="author != null">
              and author=#{author}
            </if>
        </where>
    </select>
    ```

- ##### choose,when,where

  - 逻辑结构相当于swtich语句

  - ```xml
    <select id="findMusic" resultType="cam.pojo.Music" parameterType="cam.pojo.Music">
        select * from music
        <where>
            <if test="id > 0">
                and id=#{id}
            </if>
            <if test="name != null">
                and name = #{name}
            </if>
            <if test="author != null">
                and author=#{author}
            </if>
            <choose>
                <when test="id > 0">
                    and id=#{id}
                </when>
                <when test="name != null">
                    and name = #{name}
                </when>
                <otherwise>
                    author = #{author}
                </otherwise>
            </choose>
        </where>
    </select>
    ```

- ##### set标签

  - 在update标签中使用

  - 作用：如果内部的if标签有至少一个需要生效，则将set关键字前置

    - 内部的if标签的内容，如果是最后一个判定为需要嵌入sql语句的，则自动将尾部的','去除，如果本来就没有则直接下一步
    - 内部的if标签的内容，如果不是最后一个判定为需要嵌入sql语句的，则自动在尾部添加 ','，本来就有则无需此步

  - ```xml
    <update id="updateMusic" parameterType="cam.pojo.Music">
        update music
        <set>
            <if test="name != null">
                name=#{name},
            </if>
            <if test="author != null">
                author=#{author},
            </if>
            <if test="path != null">
                path=#{path}
            </if>
        </set>
    
        <where>
            <if test="id != -1">
                id=#{id}
            </if>
        </where>
    </update>
    ```

- ##### trim标签

  - 作用：用于去除sql语句中多余的and关键字，逗号，或者给sql语句前拼接 “where“、“set“以及“values(“ 等前缀，或者添加“)“等后缀，可用于选择性插入、更新、删除或者条件查询等操作

  - 涉及到的属性

    ![image-20201124154829022](D:\notes\随笔\note-imgs\image-20201124154829022.png)

- ##### sql标签和include标签

  - 可以实现一些sql片段的复用

  - 可以将配置文件中的一些sql片段抽取出来，需要的时候使用include标签引用对应sql的id

  - 注意：不要将where标签抽取出来

    ```xml
    <sql id="if-name-author-path">
        <if test="name != null">
            name=#{name},
        </if>
        <if test="author != null">
            author=#{author},
        </if>
        <if test="path != null">
            path=#{path}
        </if>
        <if test="playTimes != null">
            playTimes=#{playTimes}
        </if>
    </sql>
    
    <sql id="if-id-account">
        <if test="id != -1">
            id=#{id}
        </if>
        <if test="account != null">
            account=#{account}
        </if>
    </sql>
    <update id="updateMusic" parameterType="cam.pojo.Music">
        update music
        <set>
            <include refid="if-name-author-path"></include>
        </set>
    
        <where>
            <include refid="if-id-account"></include>
        </where>
    </update>
    ```

- ##### foreach标签

  - 用在同种判断多次出现的情况

    ```xml
    <select id="selectTest" resultType="Music" parameterType="map">
        select * from music
        <where>
            <!--ids是在整体传入的map中存在的一个key为ids的一个集合，item为每个元素起一个变量名，方便引用-->
            <foreach collection="ids" item="id" open="and (" close=")" separator="or">
                id=#{id}
            </foreach>
        </where>
    </select>
    ```

- ##### 缓存

  - 一级缓存默认打开，二级缓存需要手动配置才能开启

  - 查看博客：[https://www.cnblogs.com/happyflyingpig/p/7739749.html]

  - 缓存查找顺序：二级缓存->一次缓存->查询数据库

  - 二级缓存存放在mapper中，一级缓存存放在session中

  - ##### 自定义缓存

    - ehcache

    - 导包

      - ```xml
        <dependency>
        	<groupId>org.mybatis.caches</groupId>
            <atifactId>mybatis-ehcache</atifactId>
            <version>1.1.0</version>
        </dependency>
        ```

    - 在配置文件中使用（Mapper配置文件，非主要配置文件）,顶替二级缓存

      - ```xml
        <cache type="org.mybatis.caches.ehcache.EhcacheCache"></cache>
        ```

    - 创建ehcache专属配置文件

      - ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                 xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd"
                 updateCheck="false">
            <!--
               diskStore：为缓存路径，ehcache分为内存和磁盘两级，此属性定义磁盘的缓存位置。参数解释如下：
               user.home – 用户主目录
               user.dir  – 用户当前工作目录
               java.io.tmpdir – 默认临时文件路径
             -->
            <diskStore path="java.io.tmpdir/Tmp_EhCache"/>
            <!--
               defaultCache：默认缓存策略，当ehcache找不到定义的缓存时，则使用这个缓存策略。只能定义一个。
             -->
            <!--
              name:缓存名称。
              maxElementsInMemory:缓存最大数目
              maxElementsOnDisk：硬盘最大缓存个数。
              eternal:对象是否永久有效，一但设置了，timeout将不起作用。
              overflowToDisk:是否保存到磁盘，当系统当机时
              timeToIdleSeconds:设置对象在失效前的允许闲置时间（单位：秒）。仅当eternal=false对象不是永久有效时使用，可选属性，默认值是0，也就是可闲置时间无穷大。
              timeToLiveSeconds:设置对象在失效前允许存活时间（单位：秒）。最大时间介于创建时间和失效时间之间。仅当eternal=false对象不是永久有效时使用，默认是0.，也就是对象存活时间无穷大。
              diskPersistent：是否缓存虚拟机重启期数据 Whether the disk store persists between restarts of the Virtual Machine. The default value is false.
              diskSpoolBufferSizeMB：这个参数设置DiskStore（磁盘缓存）的缓存区大小。默认是30MB。每个Cache都应该有自己的一个缓冲区。
              diskExpiryThreadIntervalSeconds：磁盘失效线程运行时间间隔，默认是120秒。
              memoryStoreEvictionPolicy：当达到maxElementsInMemory限制时，Ehcache将会根据指定的策略去清理内存。默认策略是LRU（最近最少使用）。你可以设置为FIFO（先进先出）或是LFU（较少使用）。
              clearOnFlush：内存数量最大时是否清除。
              memoryStoreEvictionPolicy:可选策略有：LRU（最近最少使用，默认策略）、FIFO（先进先出）、LFU（最少访问次数）。
              FIFO，first in first out，这个是大家最熟的，先进先出。
              LFU， Less Frequently Used，就是上面例子中使用的策略，直白一点就是讲一直以来最少被使用的。如上面所讲，缓存的元素有一个hit属性，hit值最小的将会被清出缓存。
              LRU，Least Recently Used，最近最少使用的，缓存的元素有一个时间戳，当缓存容量满了，而又需要腾出地方来缓存新的元素的时候，那么现有缓存元素中时间戳离当前时间最远的元素将被清出缓存。
           -->
            <defaultCache
                    eternal="false"
                    maxElementsInMemory="10000"
                    overflowToDisk="false"
                    diskPersistent="false"
                    timeToIdleSeconds="1800"
                    timeToLiveSeconds="259200"
                    memoryStoreEvictionPolicy="LRU"/>
          
            <cache
                    name="cloud_user"
                    eternal="false"
                    maxElementsInMemory="5000"
                    overflowToDisk="false"
                    diskPersistent="false"
                    timeToIdleSeconds="1800"
                    timeToLiveSeconds="1800"
                    memoryStoreEvictionPolicy="LRU"/>
          
        </ehcache>
        ```

      - 