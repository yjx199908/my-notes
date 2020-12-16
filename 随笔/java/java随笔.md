### java随笔 - 2020年7月16日15:45:23

- #### 静态代码块
  - 是sun公司给java程序员提供的时机（类似于类的生命周期钩子）

  - 格式

    ```java
    class Demo{
        static{
            System.out.println("静态代码块1执行");
        }
        
        ...
            
        static{
            System.out.println("静态代码块2执行");
        }
        
        ...
            
        static{
            System.out.println("静态代码块3执行");
        } 
        
        
    }
    ```

  - 执行时机:类加载时执行

  - 执行顺序:从上到下 在main方法之前

  - 注意点:

    - 静态代码块和静态变量的定义都在类加载时执行，属于同一时机，所以需要注意书写时的顺序 ------即静态代码块中若想使用类的静态变量，需要将此变量写在此静态代码块之前。

- #### 实例代码块

  - 格式

    ```java
    class Demo{
        {
            System.out.println("静态代码块1执行");
        }
        
        ...
            
        {
            System.out.println("静态代码块2执行");
        }
        
        ...
            
        {
            System.out.println("静态代码块3执行");
        } 
    }
    ```

  - 执行时机:在实例初始化时执行

  - 执行顺序:在构造方法之前执行

- ### this

  - this是当前对象的引用

  - this只能使用在实例方法和构造方法中

  - this.正常情况下可省略

  - 当前构造方法调用本类其他的构造方法

    ```java
    this(参数列表);//必须是当前构造方法的第一条语句
    ```

- #### 多态

  - 转型(必须有继承关系)

    - 向上转型
      - Demo

    ```java
    父类 父类对象名 = new 子类();//子转父
    ```

    - 向下转型
      - Demo

    ```java
    子类 子类对象名 = (子类名)父类类型对象;//父转子
    ```

  - 引用类型转换称为向上转型和向下转型，不叫自动转换或强转

  - java程序执行分为编译阶段和运行阶段

    - 编译阶段:
      - 对于编译器而言，编译器会根据引用的类型（也就是父类型）去对应的字节码文件中找对应的方法，找到并绑定上，编译成功（ ~~静态绑定~~）
    - 运行阶段
      - 运行阶段的时候，实际上在堆内存中创建的是子类对象，所以调用方法时，会调用属于调用者类型的方法，也就是子类的方法，这个过程属于运行阶段绑定（~~动态绑定~~）
    - 多态表示多种形态
      - 编译的时候一种形态
      - 运行的时候另一种形态

- #### instanceof运算符

  - ```java
    对象名 instanceof 类名 //是true 否false
    ```

  - 运行时动态判断

- #### 静态方法不存在方法覆盖

  - 静态方法调用只看类型，所以一般说静态方法不存在方法覆盖

- #### 私有方法无法覆盖

  

- #### 构造方法无法被继承



- #### 方法重写时返回值类型

  - 如果是基本类型，必须一模一样
  - 如果是引用类型
    - 父->子，可以
    - 子->父，不可以
  
- #### this和super

  - supers是一个关键字，全部小写
  - 对比
    - this
      - this能出现在实例方法和构造方法中
      - this不能在静态方法中使用
      - this()只能出现在构造方法第一行
    - super
      - super也能出现在实例方法和静态方法中
      - super.大部分时候也可以省略
      - super()只能出现在构造方法第一行 先初始化父类型特征
      - super也不能在静态方法中使用
      - super()在子类构造方法中调用父类构造方法 模拟现实世界中要想先有儿子必须先有父亲
      - 构造方法中会自动存在super()
      - super在堆中指向当前对象中的父类特征
      - super()的作用是初始化当前对象的父类型特征,不是用来创建对象
      - super关键字指向当前对象的父类特征部分
    - super和this只能用一个
    - 在实例方法中var_name和this.var_name和super.var_name都可以访问到父类中的不重名属性和方法
    - 如果子类中存在同名属性或方法，则var_name和this.var_name只能访问到子类中的var_name,此时若想访问父类特征中的var_name，则super.不能省略

- #### 发生继承后虽然子类构造方法执行过程中显示或隐式执行了一系列this()和super(),但实际上只在堆中创建了一个对象



- #### 父类的静态方法和属性可以被子类继承




- #### 父类静态方法不可以被子类实例方法重写，否则会报异常

  
  
- #### 方法重写时，如果访问权限低于父类对应方法的访问权限，会报异常

  
  
- #### 实例属性不会被重写，调用时根据对象类型调用，而不受堆内存中对象类型的影响

  

```tex
//P318 ----2020年7月17日20:22:41
```



- #### idea

  - 组织方式 project->module
  - 自动保存，不需要ctrl + s
  - 快捷键
    - 快速生成main方法：psvm
    - 快速生成System.out.println(): sout
    - 删除一行：ctrl + y
    - 新增/新建/重写: alt+insert,然后直接敲字母
    - 窗口变大变小: ctrl+shift+F12
    - 打开关闭功能功能窗口: alt + 窗口标号

```tex
//P327 ----2020年7月18日08:13:16
```



- #### final

  - final可以修饰变量、方法和类
  - final修饰的类不可以被继承
  - final修饰的方法不可以被重写，如果重写会报异常
  - final修饰的变量
    - final修饰的实例属性和类属性必须从以下赋值方式中二选一
      - 在构造方法中赋值
      - 在定义时赋值
    - final修饰的局部变量只能赋值一次
    - 不可以更改（但如果是引用类型，可以更改其指向对象的特征）
    - 因为final修饰的实例变量永远也不会改变，且所有对象都有一份，浪费内存，所以一般情况下final修饰的属性一般添加static修饰
    - static final修饰的属性称为常量，所有字母大写，单词之间用下划线隔开

- #### 抽象类

  - 抽象类也属于引用数据类型
  - 定义语法:
    - [修饰符列表] abstract class 类名 {类体}
  - 抽象类无法实例化
  - 天生就是用来继承的
  - abstract 不可以和final联合使用，否则会报异常
  - 抽象类的子类可以是抽象类
  - 抽象类有构造方法，构造方法是共子类使用的（在子类构造方法中使用super）
  - 抽象方法
    - 可以存在于抽象类和接口中
    - 使用abstract修饰
  - 非抽象类继承抽象类时必须将所有的抽象方法实现，否则报错
  - 一个类只能继承一个抽象类

- #### 抽象方法

  - ```java
    [修饰符列表] abstract 返回值类型 方法名();
    ```

- #### 可以使用抽象类作为父类实现多态

- #### 接口

  - 接口也属于引用数据类型
  - 接口是特殊的抽象类（完全抽象）
  - 定义语法:
    - [修饰符列表] interface 接口名{}
  - 一个接口可以使用extends继承多个接口
  - 接口中只包含两部分内容 
    - 常量
    - 抽象方法
  - 接口中所有的成员都是public修饰的
  - 方法前的public abstract 可以省略
  - 常量前的public static final也可以省略
  - 接口也可以作为父类实现多态
  - 接口的祖先也是Object
  - 接口中可以实现方法，但必须用default修饰

- #### 实现接口和继承类的写法区别

  - 类继承抽象类用extends，类实现接口用implements
  - 类和类单继承，类实现接口时可以实现多个

- #### 坑:同时实现多个接口(a,b,c,d....)的类的对象可以赋值给单个接口类型的引用，并且可以a,b,c,d....之间直接进行强制转换（即使接口之间并无继承关系）,但运行时可能会出现ClassCastException异常

- #### 同时继承类和实现接口时先extends再implements

- #### 包机制

  - 作用: 方便程序的管理

  - 不同功能的类放在不同的包下

  - package和import

    - package用法: package 包名

    - package语句只允许出现在源代码第一行

    - 包名命名规范:

      - 公司域名倒叙 + 项目名 + 模块名 + 功能名

    - 带有package编译后需要手动或自动放到对应的目录下，且访问时要用包名.类名

    - 当两个类在同一个包内时，不import也可以省略包名，否则必须import或者写完整类名(包名.类名)

    - ##### import是根据运行者位置(运行程序的位置)去寻找需要导入的包和类，而不是从写有import的类的位置，所以不用顾虑太多

- #### 访问权限

  - protected:只有本类,同包和子类的类体中可以访问
  - private:只有本类类体中可以访问
  - 默认:只有同一包内可以访问
  - public:任何地方都可以访问
  - 访问控制权限可以修饰：
    - 属性（public,protected,private,默认）
    - 方法（public,protected,private,默认）
    - 类（public,默认）
    - 接口（public,默认）
    - ...

```tex
//P377 ----2020年7月18日20:40:25
```



- #### finalize方法

  - 原型:protected void finalize()
  - 是java对象销毁时机
  - 当一个java对象即将被垃圾回收器回收的时候，垃圾回收器负责调用对象的finalize方法
  - Object中定义的finalize是一个空方法，程序员可以重写

- #### 建议垃圾回收器启动

  - System.gc()
  - 只是建议，有可能不听建议

- #### clone

  - 可以克隆对象，但需要重写

- #### hashCode

  - 定义于Object
  - 原型:public native int hashCode();
  - 不是抽象方法，带有native关键字，底层调用C++程序
  - 返回的是哈希码
  - 实际上就是一个java对象的内存地址，经过哈希算法，得出的一个值，所以执行结果可以等同看做一个java对象的内存地址

- #### 深克隆和浅克隆

  - 浅克隆只会克隆基本数据类型和引用类型地址
  - 深克隆会克隆基本数据类型和引用数据类型对象

- #### 内部类

  - 在类的内部定义的类

  - 编译后的class文件名为 外部类名$内部类名.class

  - 能不用尽量不用

  - 分为：

    - 静态内部类

      - 如果访问权限允许,可以在外部类的外部使用 外部类名.内部类名访问到
      - 可以使用访问权限修饰
      - 可以在类体内访问外部类的静态成员
      - 可以拥有静态成员
    - 外部类体内可以访问内部类中的静态私有成员
    
  - 实例内部类
    
      - 先实例化外部类对象，才能使用
      - 可以使用访问权限修饰
      - 不可以拥有静态成员
    - 可以访问外部类成员
    
  - 局部内部类
    
    - 出了此局部便无法访问
    
      ```java
      class OutClass{
          static class StaticInnerClass{    
          	//静态内部类
          }
          
          class InsInnerClass{
              //实例内部类
          }
          
          public void dosome(){
              class LocalInnerClass{
                  //局部内部类
              }
          }
          
      }
      ```
    ```
    
      
    - 匿名内部类
    
      - 属于局部内部类
    
      - 没有名字
    
        - ```java
          public class Test {
              public static void main(String[] args) {
                  new MyMath().showSub(new Compute() {
                      @Override
                      public int subtract(int a, int b) {
                          return a-b;
                      }
                  },100,200);
              }
          }
          
          interface Compute{
              int subtract(int a,int b);
          }
          
          class MyMath{
              void showSub(Compute c,int a,int b){
                  System.out.println(c.subtract(a,b));
              }
          }
    ```
    
      - 形式如 
    
          - new 接口名(){对接口中方法的实现}
          - new 抽象类名(){对抽象类中所有抽象方法的实现或非抽象方法的重写，其中所有的抽象方法必须被实现}
        - new 普通类名(){对方法的重写}
    
        - 匿名类必须实现接口和抽象类中所有的抽象方法

- #### 垃圾回收器不会回收常量池中的数据

- #### String 

  - String类型的引用保存在堆中的String类型对象地址
  - 堆中的String类型对象中保存指向方法区常量池的地址
  - 字符串数据保存在常量池中

- #### 枚举类型

  - enum关键字

  - ```java
    enum Weather{
        W,
        E,
        H
    }
    ```

```tex
//P470 ----2020年7月19日14:16:53
```



- #### 异常

  - Exception类

  - java中异常以类的形式存在，每一个异常类都可以创建异常对象

  - 如果程序中发生异常，JVM会new出相应的异常对象并抛出

  - 所有异常的类的父类为Throwable

  - 所有异常都是发生在运行阶段！

  - 子类分为:

    - RuntimeException类簇：自身及所有子类都属于运行时异常
      - 在编写程序阶段可处理也可不处理
      - 只有运行时才会发生
      - 也叫非受控异常或未受检异常
    - Exception的直接子类：编译时异常
      - 编译时异常指必须在编译阶段或编写阶段预先处理，否则无法运行
      - 也叫受控异常或受检异常

  - 异常处理两种方式：

    - 在方法声明的位置上使用throws关键字，抛给上一级
    - 使用try..catch..语句进行异常的捕捉
    - 如果异常一直上抛，最终抛给了JVM，则JVM终止java程序
    - 也可以手动创建异常并使用 throw 异常对象 的形式手动抛出

  - 如果代码块中出异常，则本块中后续语句不会继续执行，直接进入最近的符合条件的catch，中间跳过的语句不会执行

  - try..catch..处理完异常之后流程恢复，从处理异常的catch块后执行

  - java8新特性，允许catch后小括号内使用特殊写法

    - ```java
      catch(NullPointerException| EnumConstantNotPresentException|ExceptionInInitializerError e){
      }
      ```

  - 所有的异常对象都有两个方法

    - getMessage(): 获取简单的描述信息
    - printStackTrace(): 打印异常追踪的堆栈信息

  - finallly子句

    - 一套try...catch..finally..可以没有catch子句
    - finally块中必定执行，即使try或catch中执行了return语句，也会执行finally块，除非try或catch中执行了System.exit(0)等JVM退出指令

- #### Throwable

  - 有两个子类 
    - Error: 自身与子类类型的错误无法处理，只能退出JVM
    - Exception: 自身与子类类型的错误可以运行时异常处理，不必退出JVM
    - 但都可抛出

- #### UML

  - #### 是一种统一建模语言

  - 一种图标式语言

  - 软件设计人员使用的

  - 可以描述类和类之间的关系，程序执行的流程，对象的状态等...

- #### 重写之后的方法不能比重写之前的方法抛出更多（更广泛）的异常，可以更少

- #### 类如果想强制转换成接口类型，则不需要存在继承关系也不会报错

- #### 集合

  -  集合不能直接存储基本数据类型



- #### 泛型

  - jdk5.0之后推出

  - 可以自定义

    - ```java
      class MyClass<E> {
          E element;
          ...
      }
      //以上为泛型类（还可定义泛型类，泛型抽象类，泛型接口等）
      
      public <E> E doSome(E e){
          return e;
      }
      //以上为泛型方法
      
      ```

    - 

- #### 自动类型推断机制

  - jdk8之后推出钻石表达式<>
  - 形如 new 集合类<>()



- #### 增强for循环（foreach）

  - ```java
    for(元素类型 变量名: 数组或集合){//dosome}
    ```

  - foreach有一个缺点:没有下标

- #### HashSet

  - 无序不可重复
  - 若存入数据与集合内数据相同，则不会重复存入

- #### TreeSet

  - 存入的数据自动排序

- #### io流

  - 字节流
    - 万能的
    - 按字节读取
  - 字符流
    - 一次读取一个字符
    - 方便读取普通文本文件存在
  - java中的io流都已经写好了
  - 所有的流都在java.io.*下
  - io流有四大家族
    - java.io.InputStream  字节输入流
    - java.io.OutputStream  字节输出流
    - java.io.Reader  字符输入流
    - java.io.Writer  字符输出流
    - 以上全为java流的顶级抽象类
    - 都实现了java.io.Closeable接口，所以都是可关闭的，都有close方法
    - 用完一定要关闭，因为流是管道，耗费系统资源
    - Writer类簇和OutputStream类簇都是可刷新的（都实现了java.io.Flushable接口都有flush方法），输出流在最后输出之后一定要flush一下，将剩余未输出数据强行输出完，并清空管道
    - 需要掌握的流有16个
      - file相关
        - java.io.FileInputStream
        - java.io.FileOutputStream
        - java.io.FileReader
        - java.io.FileWriter
      - buffer相关(缓冲流)
        - java.io.BufferedReader
        - java.io.BufferedWriter
        - java.io.BufferedInputStream
        - java.io.BufferedOutputStream
      - data相关(数据流)
        - java.io.DataInputStream
        
        - java.io.DataOutputStream
        
        - 数据流（data stream）是一组有序，有起点和终点的字节的数据序列。包括输入流和输出流。
        
          数据流最初是通信领域使用的概念，代表传输中所使用的信息的数字编码信号序列。这个概念最初在1998年由Henzinger在文献87中提出，他将数据流定义为“只能以事先规定好的顺序被读取一次的数据的一个序列”。
        
          
      - object相关
        - java.io.ObjectInputStream
        - java.io.ObjectOutputStream
      - 流转换(字节流--->>字符流)
        - java.io.InputStreamReader
        - java.io.OutputStreamWriter
      - print相关(标准输出流)
        - java.io.PrintWriter
        - java.io.PrintStream
    - FileInputStream类的其他常用方法
      - int available(); 返回流当中剩余的没有读到的字节数量
      - long skip(long n); 跳过几个字节不读

- #### IDEA的默认当前路径

  - 工程project的根目录

- #### File

  - File类和io流四大家族没有关系，所以File类不能完成文件读写

  - 代表文件和路径名的抽象表示形式

  - 常用方法：

    - boolean exists(); 判断文件存不存在

    - boolean createNewFile(); 以文件形式新建

    - boolean mkdir(); 以目录形式新建

    - boolean mkdirs(); 以多重目录形式新建

    - String getParent(); 以字符串形式返回文件的父路径（从根目录开始）

    - File getParentFile(); 以文件形式返回文件的父目录

    - String getAbsolutePath(); 获取绝对路径

    - String getName(); 获取文件名（不包括路径）

    - boolean isDirectory(); 判断是否是一个目录

    - boolean isFile(); 判断是否是一个文件

    - boolean isHidden(); 判断是否隐藏

    - long lastModified(); 返回最后一次修改时间（从1970年到现在的总毫秒数）

      - ```java
        Date time = new Date(f.lastModified());
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss SSS");
        String strTime = sdf.format(time);
        System.out.println("最后修改时间:" + strTime);
        ```

    - long length(); 返回文件大小（字节数）

    - File[] listFiles(); 获取当前目录下的所有子目录和文件

- #### 序列化和反序列化和Serializable接口

  - 序列化：将对象放到硬盘文件上或网络上

    - Serialize
    - 把对象切成一块块的进行传输
    - 每一块拥有编号
    - 使用ObjectOutputStream
    - 对象需要支持序列化
      - 类需要实现Serializable接口才能使其实例可以被序列化
    - 如果使用集合序列化多个对象，则集合和元素类型都需要实现Serializable接口
    - transient关键字
      - 当不希望某一个类中的某一个属性参与序列化时使用
      - 表示游离的
      - 反序列化时此属性值为默认值

  - 反序列化：把对象从硬盘文件上恢复到内存中

    - DeSerialize
    - 按照编号组装对象
    - 使用ObjectInputStream

  - Serializable接口

    - 起到标识的作用

    - JVM看到这个类实现了这个接口，会为该类生成一个序列化版本号

      - 序列化版本号永久标识一个类，改动之后会重新生成序列化版本号

      - 如果序列化版本号不符，则无法反序列化

      - 可以手动将序列化版本号写出来，则不会自动生成

        - ```java
          private static final long serialVersionUID = 1;
          ```

          

- #### Properties

  - Map集合
  - 继承HashTable
  - key和value都是String类型
  - 被称为属性类对象

- #### 接口中有一种标志接口是给JVM看的

- #### java语言采用什么机制区分类

  - 首先通过类名进行比对
  - 如果类名相同，则再通过序列化版本号区分

- #### IO+Properties

  - IO流：文件的读写

  - Properties：是一个Map集合，key和value都是String类型

  - ```java
    FileReader readerr = new FileReader('file');
    Properties pro = new Properties();
    pro.load(reader);
    //自动将文件中的形如'v=k'的字符串以key,value的形式加载到pro中
    String value = pro.getProperty("v");//读取属性值
    ```

- #### 属性配置文件（.properties）

  - ##### 在属性配置文件（.properties）中，不要有空格

  - ##### key和value之间用=或者:隔开，最好用=

  - ##### 使用#注释，

  - ##### key不可以重复

  - 每行一个键值对

```tex
//P598 ----2020年7月19日22:53:41
```



- #### 并发

  - 目的：提高处理效率

  - 进程和线程

    - 进程是一个应用程序
    - 线程是一个进程中的执行场景/执行单元
    - 一个进程可以启动多个线程

  - 对于java程序来说，当在DOS命令窗口中输入：java 类名 并回车之后，会先启动JVM,JVM就是一个进程，JVM再启动一个主线程调用main方法，同时再启动一个垃圾回收线程负责看护，回收垃圾。最起码，现在的java程序中有两个线程并发，一个主线程，一个垃圾回收线程。

  - 进程之间内存独立不共享

  - 在java中，线程之间共享堆和方法区内存共享，栈内存独立且互不干扰（一个线程一个栈）

  - main方法结束，程序未必结束

  - 死锁

  - 实现多线程

    - 第一种：编写一个类继承Thread类，重写其run()方法;

      - ```java
        public class ThreadTest {
            public static void main(String[] args) {
                MyThread myThread = new MyThread();
                myThread.start();
                for (int i = 0; i < 100; i++) {
                    System.out.println("运行主线程");
                }
            }
        }
        
        class MyThread extends Thread{
            @Override
            public void run() {
                int count = 0;
                for (int i = 0;i <100;i++){
        
                    System.out.println("运行子线程" + count++);
                }
            }
        }
        ```

        

    - 第二种：编写一个类实现java.lang.Runnable接口，并作为Thread构造方法的参数创建Thread对象

      - ```java
        public class ThreadTest {
            public static void main(String[] args) {
                Thread thread = new Thread(new MyRunnable());
                thread.start();
                for (int i = 0; i < 100; i++) {
                    System.out.println("这里是主线程");
                }
            }
        }
        
        class MyRunnable implements Runnable{
            @Override
            public void run() {
                for (int i = 0; i < 100; i++) {
                    System.out.println("这里是分支线程run方法");
                }
            }
        }
        ```

    - 建议使用第二种方法，因为实现接口同时还可以继承别的类和实现别的接口

    - 第二种变相：

      - 使用匿名内部类创建Thread

      - ```java
        public class ThreadTest {
            public static void main(String[] args) {
                Thread thread = new Thread(new Runnable() {
                    @Override
                    public void run() {
                        for (int i = 0; i < 100; i++) {
                            System.out.println("使用匿名内部类创建的分支线程");
                        }
                    }
                });
                thread.start();
                for (int i = 0; i < 100; i++) {
                    System.out.println("这里是主线程");
                }
            }
        }
        ```

    - 第三种方法：

      - 实现Callable接口

      - 可以获取线程返回值

        - demo

          ```java
          FutureTask task = new FutureTask(new Callable(){
              public Object call(){
                  //重写call
              }
              //call相当于带返回值的run
          });
          Thread thread = new Thread(task);
          thread.start();
          Object obj = task.get();//获取执行结果 
          //这个方法回到导致当前线程阻塞,直到thread执行完毕返回结果
          ```

          

  - 线程方法：

    - void start(); 
      - 启动线程 //作用：在JVM中开辟一个栈空间，启动一个分支线程，只要空间开出来，start()方法就结束了！
      - 启动成功的线程会自动调用run方法，并且run方法在分支栈的底部（run方法在分支栈的栈底，main方法在主栈的栈底，run和main是平级的）
      - 如果不用start(),直接调用run()，则无法并发，因为并没有开辟新的栈空间，还是在主栈中执行

    - void setName();
      - 设置线程名字

    - String getName();
      - 获取线程名字 
    - Thread Thread.currentThread();
      - 返回当前线程的引用
      - 出现在哪个线程返回的就是哪个线程的引用
      - 当多个线程复用一个方法时便可以起到作用
    - void Thread.sleep(long millis);
      - 使当前线程进入休眠（阻塞状态）
    - void interrupt();
      - 打断进程的睡眠状态
      - 使目标线程调用的sleep方法产生异常，进入try..catch..处理后继续
    - void stop();
      - 强行终止线程
      - 缺点：容易丢失数据
      - 已过时，不建议使用
    - 和线程调度有关的方法
      - void setPriority();
        - 设置线程优先级
        - 优先级比较高的获取的时间片可能长一些
        - 1-10 默认5
      - int getPriority();
        - 获取线程优先级
      - void Thread.yield();
        - 暂停当前正在执行的线程对象，并执行其他线程
        - 不是阻塞，而是使线程马上回到就绪状态
        - 有可能马上又抢到时间片
      - void join();
        - 合并线程
        - 当前线程进入阻塞，实例线程执行，执行完毕后当前线程继续执行

  - 线程生命周期

    - 新建（new）-》新建状态
    - 启动（start()）-》就绪状态
      - 又叫可运行状态
      - 具有抢夺CPU时间片的权利（CPU时间片就是执行权）
      - 抢到时间片后进入运行状态
    - 运行状态
      - run方法的开始执行标志着进入运行状态
      - CPU时间片用完之后，会重新回到就绪状态继续抢夺CPU时间片
      - 再次抢到时间片之后从上次时间片用完时的状态继续执行
      - 运行过程中遇到阻塞事件会放弃占有的时间片，进入阻塞状态
      - run方法执行结束后进入死亡状态
    - 阻塞状态
      - 阻塞解除会回到就绪状态抢夺CPU时间片
    - 死亡状态
      - run方法执行结束的状态
    - 锁池（不属于状态）
      - 进入锁池可以理解为一种阻塞状态
      - 线程进入锁池找共享对象的共享锁，释放之前占有的时间片
      - 没找到的话在锁池等待
      - 找到的话进入就绪状态继续抢夺时间片

  - 常用的线程调度模型

    - 抢占式调度模型
      - 哪个线程的优先级比较高，抢到CPU时间片的概率就高一些/多一些
      - java采用的就是此模型
    - 均分式调度模型
      - 平均分配CPU时间片，每个线程占有的CPU时间片时间长度一样，平均分配一切平等

  - 数据安全问题

    - 发生条件

      - 多线程并发
      - 有共享数据
      - 共享数据有修改的行为

    - 如何解决

      - 线程同步机制

        - 也就是线程排队
        - 会牺牲一部分效率

      - synchronized（属于一种阻塞状态）

        - 每个java对象都有一把锁，synchronized会占用对象的锁，直到同步代码块执行完毕才会释放锁

        - 用法

          - 在开发中最好不要嵌套使用，避免发生死锁

          - 同步代码块
      
            ```java
            synchronized(data){//小括号里必须是多线程共享的数据对象（不一定是需要使用的对象，只要是被两个线程共享的对象就可以）
                //同步语句
        }
            ```

          - 修饰实例方法
      
            ```java
            public synchronized void 方法名(参数列表){
                //语句
            }
            //一定锁的是this
        //整个方法体都会同步
            ```
      
          - 修饰静态方法
            
            - 表示找类锁
        - 一个类一把锁
      
            - 保证静态变量的安全

          
      
        - 如果对象已经被其他对象占用，则只能等待
      
      - 同步代码块中语句越少，效率越高
  
  - 守护线程

    - 具有代表性的是垃圾回收器

    - 一般是一个死循环

    - 所有用户线程结束，守护线程自动结束

    - 将线程设置为守护线程：
  
      - ```java
        Thread thread = new Thread(new Runnable(){
            public void run(){
                //任务
            }
        });
        thread.setDaemon(true);//设置为守护线程    
      thread.start();
        ```

  - 定时器

    - 间隔特定的时间执行特定的程序

    - 创建定时器对象
  
      - ```java
        Timer timer = new Timer();//创建新定时器
        Timer timer = new Timer(true);//创建新定时器，且其相关的线程为守护线程
        Timer timer = new Timer("timer1");//创建新定时器，且其相关的线程名为timer1
      Timer timer = new Timer("timer1",true);//创建新定时器,且其相关的线程为守护                                          //线程,其相关的线程名为timer1
        ```

    - 其他方法参考文档

  - wait和notify方法

    - 不是线程对象的方法，而是Object中的方法

    - 建立在synchronized的基础上

    - wait();

      - 作用:让正在此对象上活动的线程（当前线程）进入无期限等待状态，并且之前释放占有的锁，直到被notify唤醒

    - notify();

      - 作用:唤醒在此对象上等待的线程，不会使当前线程释放锁

    - notifyAll();

      - 作用:唤醒在此对象上等待的所有线程，不会使当前线程释放锁

    - demo
  
      - ```java
        public class ProductorAndSpendor {
            public static void main(String[] args) {
                Catalog c = new Catalog();
        
                Thread t1 = new Thread(new ProductorRunnable(c));
                Thread t2 = new Thread(new SpendorRunnable(c));
        
                t1.start();
                t2.start();
        
            }
        }
        
        class Catalog{
            public int pro = 0;
        }
        
        class ProductorRunnable implements Runnable{
        
            public Catalog c;
        
            public ProductorRunnable(Catalog c){
                this.c = c;
            }
        
            public void run() {
                while(true){
                    synchronized (c){
                        if(c.pro == 10){
                            try {
                                System.out.println("阻塞生产者一次");
                                c.wait();
                            } catch (InterruptedException e) {
                                e.printStackTrace();
                            }
                        }
                        else{
                            c.pro++;
                            System.out.println("生产一个,目前有" + c.pro + "个");
                            c.notify();
                        }
                    }
                }
            }
        }
        
        class SpendorRunnable implements Runnable{
        
            public Catalog c;
        
            public SpendorRunnable(Catalog c){
                this.c = c;
            }
        
            public void run() {
                while(true){
                    synchronized (c){
                        if(c.pro == 0){
                            try {
                                System.out.println("阻塞消费者一次");
                                c.wait();
                            } catch (InterruptedException e) {
                                e.printStackTrace();
                            }
                        }
                        else{
                            c.pro--;
                            System.out.println("消费一个,目前有" + c.pro + "个");
                            c.notify();
                        }
                    }
                }
            }
      }
        ```

        
  
  ```tex
//P651 ----2020年7月20日21:57:54
  ```
  
- #### 反射机制

  - 可以操作字节码文件

  - 可以操作代码片段

  - 相关的类在java.lang.reflect.*;

  - 重要的类有：

    - java.lang.Class; 代表字节码文件
      - 实例引用直接指向被加载到方法区的类的字节码
    - java.lang.reflect.Method; 代表字节码中的方法字节码
    - java.lang.reflect.Constructor; 代表字节码中的构造方法字节码
    - java.lang.reflect.Field; 代表字节码中的属性字节码

  - 要操作一个类的字节码，需要要获取到这个类的字节码（即java.lang.Class实例）

  - 获取类的字节码的三种方式:

    - Class Class.forName() 
      - demo:Class c = Class.forName("包.类名");
    - Class object.getClass() //object:任意对象
      - demo:Class c = object.getClass();//object可以是任意引用类型的对象引用
    - Class c = 类型.class; //java中任意属性都有.class属性
      - demo:Class c = int.class;
      - demo:Class c = String.class;

  - 通过反射创建实例

    - Object object = c.newInstance(); //底层会调用相应类的无参构造方法，如果没有无参构造会报异常

  - 反射Field

    - Field[] fields = c.getFields(); //获取类中所有的公开属性成员 

    - Field field = c.getField("");

    - Filed[] fields = c.getDeclaredFields(); //获取类中所有属性成员

    - Field field = c.getDeclaredField("");

    - 给属性赋值

      - ```java
        Field field = c.getDeclaredField("属性名");
        Object o = field.newInstance();
        field.set(o,属性值);
        ```

    - 获取属性的值

      - ```java
        Field field = c.getDeclaredField("属性名");
        Object o = field.newInstance();
        field.get(o);
        ```

    - 打破封装

      - 如果不打破封装，则上述方法无法读写private属性

      - private修饰的字段需要打破封装才能正常读取

      - ```java
        Field field = c.getDeclaredField("属性名");
        field.setAccessible(true);
        ```

  - 反射Method

    - 获取
      - Method[] methods = c.getDeclaredMethods(); //获取类中所有方法
      - Method method = c.getDeclaredMethod("方法名",形参类型列表); //获取类中指定方法，形参列表类型为Class实例（例如String.class和int.class）
    - 常用方法
      - Class c = method.getReturnType();//获取返回值类型
      - String c = method.getModifiers(); //获取修饰符
      - String methodName = method.getName(); //获取方法名
      - Class[] cs = method.getParameterTypes(); //返回参数类型列表
    - 通过反射调用方法(******)
      - Object resturnValue = method.invoke(obj,实参数据列表);

  - 反射Constructor

    - 获取
      - Constructor[] constructors = c.getDeclaredConstructors(); //获取类中所有构造方法
      - Constructor constructor = c.getDeclaredConstructor(形参类型列表)//获取类中指定方法，形参列表类型为Class实例（例如String.class和int.class）
    - 其余参考文档

  - 使用反射创建对象

    - 无参创建
      - Object obj = c.newInstance(); //c为Class实例
    - 有参创建
      - Object obj = constructor.newInstance(实参列表); //constructor为Constructor实例

  - 通过反射获取一个类的父类

    - ```java
      Class c = Class.forName("包.类名");
      Class superclass = c.getSuperClass();
      ```

  - 获取类实现的所有接口

    - ```java
      Class c = Class.forName("包.类名");
      Class[] interfaces = c.getInterfaces();
      ```

  - 判断类上是否有注解A

    - ```java
      boolean isHas = c.isAnnotationPresent(A.class);
      ```

  - 操作类上的A类型注解对象

    - ```java
      A a = (A)c.getAnnotation(A.class);//获取类上的注解对象
      String/int/.. str = a.属性名();//获取注解中的属性值
      ```

      

- #### 如果只希望一个类的静态代码块执行，其他代码一律不执行，则可以使用Class.forName()

- #### 路径问题

  - 直接在idea中使用相对路径的话，移植性差

  - 使用通用方式

  - 通用方式前提：这个文件必须在类路径下

  - 类路径：方式在src下的都是类路径

  - src是类的根路径

  - Thread.currentThread().getContextClassLoader().getResource("").getPath();

    - Thread.currentThread() //获取当前线程对象
    - getContextClassLoader() //线程方法，可以获取到当前线程的类加载器对象
    - getResource() //当前线程的类加载器默认从类的根路径下加载资源
    - 在idea中直接在java文件簇的src下添加配置文件，再通过上述代码加载，文件会被自动添加到out中对应的目录下

  - InputStream is = Thread.currentThread().getContextClassLoader().getResourceAsStream("");

    - 直接以流的形式返回，无需再创建流

  - java.util下提供了一个资源绑定器，便于获取属性配置文件中的内容

    - 使用这种方式的时候，属性配置文件xxx.properties必须放到类路径下

    - 只能绑定properties文件

    - 写路径的时候，路径后面的扩展名不能写

    - demo

      ```java
      ResourceBundle bundle = ResourceBundle.getBundle("相对src的路径+文件名（不加后缀）");
      String className = bundle.getString("className");
      ```

    - 

- #### 关于类加载器

  - 启动类加载器（父加载器）
    - 加载C:\Program Files\Java\jdk1.8.0_221\jre\lib\rt.jar
  - 扩展类加载器（母加载器）
    - 加载C:\Program Files\Java\jdk1.8.0_221\jre\lib\ext\\*.jar
  - 应用类加载器
    - 专门加载classpath路径中的类
  - 首先通过启动类加载器专门加载rt.jar中的类，rt.jar中是最核心的类库，启动类加载器加载不到的时候，再用扩展类加载器，如果扩展类加载器没有加载到，在通过应用类加载器。

  - 双亲委派机制
    - 优先顺序 父--》母--》应用类记载器

- #### 可变长度参数

  - demo

    ```java
    public void func(int... args){
    } 
    public void func(String str,int... args){
    } 
    ```

  - 要求参数个数0-n;

  - 必须写在形参列表最后一个

  - 一个参数列表只能出现一个

  - 可以当做一个数组一样用

  - 形参也可以对应的传一个数组

- #### 注解 Annotation

  - 注解是一种引用数据类型，编译之后也是生成xxx.class文件
  - 自定义
    
    - [修饰符列表] @interface 注解类型名{}
  - 使用时的语法格式
    
    - @注解类型名
  - 可以出现在类上，属性上，方法上，变量上等......
  - 还可以出现在注解类型上
  - jdk内置了一些注解
  - 分为：
  - 标识注解: 让编译器做检查
  
- 
  
- 如果注解A用来修饰注解B,则注解A被称为元注解
  
  - 常见的元注解：
  
    - Target
      - 用来指定被标注的注解可以出现在哪些位置
        - ElementType.METHOD
        - 。。。
    - Retention
      - 用来标注被标注的注解最终保存在什么位置
        - 源文件中
          - RetentionPolicy.SOURCE
        - 字节码文件中
          - RententionPolicy.CLASS
      - c字节码文件中，并且可以被反射机制读取到
        - RententionPolicy.RUNTIME 
  
  - 自定义注解类型
  
    - ```java
      @Target(ElementType.METHOD)//元注解
      @Retention(RetentionPolicy.SOURCE)//元注解
      public @interface CusAnnotation {
          String name();
          int age();
          double length() default 180.5;
          String[] classmates();
          int[] nums();
          //带括号的不是方法，是注解类型的属性
          //如果属性没有设置默认值，则在使用时必须以键值对的形式添加
          //demo:@CusAnnotation(name="hahha",age=20)
          //属性类型只能是[String,Class,基本数据类型,枚举类型,前四种的数组类型]
          //如果属性是数组类型，则在使用时的形式为
          //demo:@CusAnnotation(...classmates={值列表}...)，如果数组中只有一个元素，		//则大括号可以省略
          
      }
      ```

```tex
//入门完结 ----2020年7月21日13:12:11
```
