- #### JVM简介

  - JVM只关心字节码文件，而不关心字节码文件是由什么语言生成的
  - 二进制字节码的运行环境

- #### 前端编译器和后端编译器

  - 前端编译器是把java文件编译成字节码

```text
//P12 ----2020年7月21日14:55:45
```



- #### 命令行jps命令可以查看当前正在执行的java相关进程及其端口号

- #### 一个java程序使用一个虚拟机执行

- #### 一个JVM对应着一个运行时数据区

- #### 一个运行时数据区对应着一个RunTime对象，所以RunTime类是单例的

- #### JIT（即时编译器）

  - 把字节码编译成机器指令
  - 可以即时把热点代码编译成机器码，提高效率
  - 主要解决的问题是执行性能
  - 如果只有JIT没有解释器
    - 会先把字节码编译成机器指令
    - 会使程序暂停时间过长

- #### 解释器

  - 逐行解释字节码
  - 主要解决的问题是响应时间
  - 如果只有解释器没有JIT
    - 会使程序执行效率过低
    - 无法对热点代码进行优化处理

- #### Sun Classic VM（classic：经典的）

  - 世界上第一款商用的Java虚拟机

  - 在JDK1.4被淘汰

  - 内部只提供解释器

  - 如果使用JIT编译器，就需要使用外挂。但是一旦使用了JIT编译器，JIT就会接管虚拟机的执行系统。解释器就不再工作。解释器和编译器不能配合工作

  - 现在hotspot内置了此虚拟机

  - ##### 解释器和JIT存在一个就可以，但是两个配合可以提高效率

- #### Exact VM（exact：准确的）

  - jdk1.2时sun公司提供
  - 为了解决classic vm的问题
  - Exact Memory Management:准确式内存管理
    - 也可以叫Mon-Conservative/Accurate Memory Management
    - 虚拟机可以知道内存中某个位置的数据具体时间什么类型
  - 具备现代高性能虚拟机的雏形
    - 热点探测
    - 编译器与解释器混合工作模式
  - 只在Solaris平台短暂使用，其他平台上还是classic vm
    - 最终被hotspot虚拟机替换

- #### SUN HotSpot VM（hotspot：热点）

  - 三大商用虚拟机的一个
  - jdk1.3时，成为默认虚拟机
  - 目前占有绝对的市场地位
  - JDK6和JDK8的默认虚拟机
  - Sun/Oracle JDK 和 OpenJDK的默认虚拟机
  - 另外两款商用虚拟机都没有方法区的概念
  - 从服务器、桌面到移动端、嵌入式都有应用
  - 名称中的hotspot指的就是他的热点代码探测技术
    - 通过计数器找到最具编译价值代码，触发即时编译或栈上替换
    - 通过编译器与解释器协同工作，在最优化的程序响应时间与最佳执行性能中取得平衡

```java
//P18 ----2020年7月22日19:50:34
```

- #### BEA 的 JRockit

  - 三大商用虚拟机的一个

  - 专注于服务器应用
  - 不太关注程序启动速度，因此内部不包含解释器实现，全部代码都靠即时编译器编译后执行
  - 是世界上最快的JVM
  - 拥有全面的Java运行时解决方案组合
    - 面向延迟敏感型应用的解决方案JRockit Real Time提供以毫秒或微秒级的JVM响应时间，适合财务，军事指挥，电信网络的需要
    - MissionControl服务套件(JMC)，它是一组以极低的开销来监控、管理和分析生产环境中的应用程序的工具
  - 2008年，EBA被Oracle收购
  - Oracle整合了两大优秀的虚拟机，在JDK8时，在HotSpot的基础上，移植JRockit的优秀特性
  - 高斯林：目前就职于谷歌，研究人工智能和水下机器人

  - 专注于服务器应用
  - 不太关注程序启动速度，因此内部不包含解释器实现，全部代码都靠即时编译器编译后执行
  - 是世界上最快的JVM
  - 拥有全面的Java运行时解决方案组合
    - 面向延迟敏感型应用的解决方案JRockit Real Time提供以毫秒或微秒级的JVM响应时间，适合财务，军事指挥，电信网络的需要
    - MissionControl服务套件(JMC)，它是一组以极低的开销来监控、管理和分析生产环境中的应用程序的工具
  - 2008年，EBA被Oracle收购
  - Oracle整合了两大优秀的虚拟机，在JDK8时，在HotSpot的基础上，移植JRockit的优秀特性
  - 高斯林：目前就职于谷歌，研究人工智能和水下机器人

- #### IBM 的 J9

  - 三大商用虚拟机的一个

  - 全程：IBM Technology for Java Virtual Machine,简称IT4J,内部代号：J9
  - 市场定位与JotSpot接近，服务器端、桌面应用、嵌入式等多用途VM
  - 广泛用于IBM的各种Java产品
  - 目前，有影响力的三大商用虚拟机之一，也号称世界上最快的Java虚拟机
  - 2017年左右，IBM发布了开元J9 VM，命名为OpenJ9,交给Eclipse基金会管理，也称为 Eclipse OpenJ9

  - 全程：IBM Technology for Java Virtual Machine,简称IT4J,内部代号：J9
  - 市场定位与JotSpot接近，服务器端、桌面应用、嵌入式等多用途VM
  - 广泛用于IBM的各种Java产品
  - 目前，有影响力的三大商用虚拟机之一，也号称世界上最快的Java虚拟机
  - 2017年左右，IBM发布了开元J9 VM，命名为OpenJ9,交给Eclipse基金会管理，也称为 Eclipse OpenJ9

- #### KVM和CDC/CLDC HotSpot

  - Oracle在Java ME产品线上的两款虚拟机为：CDC/CLDC HotSpot Implemenetation VM
  - KVM（Kilobyte）是CLDC-HI早期产品
  - 目前移动领域地位尴尬，智能手机被Andriod和ios二分天下
  - KVM简单、轻量、高度可移植，面向更低端的设备上还维持自己的一片市场
    - 智能控制器、传感器
    - 老人手机、功能手机
  - 所有虚拟机的原则：一次编译，到处运行

```java
//P20 01:05 ----2020年7月23日09:01:07
```

- #### Azul VM

  - 前面三大“高性能Java虚拟机”使用在通用硬件平台上
  - 这里Azul VM和BEA Liquid VM是与特定硬件平台绑定、软硬件配合的专有虚拟机
    - 高性能Java虚拟机中的战斗机
  - Azul VM是Azul Systems公司在HotSpot基础上进行大量改进，运行于Azul System公司的专有硬件Vega系统上的Java虚拟机
  - 每个Azul VM实例都可以管理至少数十个CPU和数百GB内存的硬件资源，并提供在巨大内存范围内实现可控的GC时间的垃圾收集器、专有硬件的线程调度等优秀特性
  - 2010年，Azul Systems工地开始从硬件转向软件，发布了自己的Zing JVM,可以在通用x86平台上提供接近于Vega系统的特性

- #### Liquid VM

  - 高性能Java虚拟机中的战斗机
  - BEA公司开发的，直接运行在自家Hypervisor系统上
  - Liquid VM即是现在的JRockit VE (Virtual Edition) , Liquid VM不需要操作系统的支持，或者说它自己本身实现了一个专用操作系统的必要功能，如线程调度、文件系统、网络支持等。|
  - 随着JRockit虛拟机终止开发，Liquid VM项目也停止了

- #### Apache Harmony

  - Apache也曾经推出过与JDK 1. 5和JDK 1. 6兼容的Java运行平台 Apache Harmony.
  - 它是IBM和Intel联合开发的开源JVM，受到同样开源的openJDK的压制，Sun坚决不让Harmony获得JCP认证，最终于2011年退役，IBM转而 参与OpenJDK
  - 虽然目前并没有Apache Ha rmony被大规模商用的案例，但是它的Java类库代码吸纳进了Android sDK。

- #### Microsoft JVM

  - 微软为了在IE3浏览器中支持Java Applets， 开发了Microsoft JVM。
  - 只能在window平台下运行。但确是当时Windows下性能最好的Java VM。
  - 1997年，Sun以侵犯商标、不正当竞争罪名指控微软成功，赔了sun很多钱。微软在WindowsXP SP3中抹掉了其VM。现在windows.上安装的jdk都是HotSpot。

- #### TaoBaoJVM

  - 由A1iJVM团队发布。阿里，国内使用Java最强大的公司，覆盖云计算、金融、物流、电商等众多领域，需要解决高并发、 高可用、分布式的复合问题。有大量的开源产品。
  - 基于openJDK开发了自己的定制版本AlibabaJDK，简称AJDK。是整个阿里Java体系的基石。
  - 基于OpenJDKHotSpotVM发布的国内第-一个优化、深度定制且开源的高性能服务器版Java虚拟机。
    - ➢创新的GCIH (GC invisible heap )技术实现了off-heap，即将生命周期较长的Java对象从heap中移到heap之外，并且Gc不能管理GCIH内部的Java对象，以此达到降低GC的回收频率和提升GC的回收效率的目的。
    - GCIH 中的对象还能够在多个Java虚拟机进程中实现共享
    - 使用crc32指令实现JVM intrinsic 降低JNI的调用开销
    - PMU hardware 的Java profiling tool 和诊断协助功能
    - 针对大数据场景的ZenGC
  - taobao vm应用在阿里产品上性能高，硬件严重依赖intel的cpu,损失了兼容性，但提高了性能
    - 目前已经在淘宝、天猫上线，把oracle 官方JVM版本全部替换了。

- #### Dalvik VM

  - 谷歌开发的，应用于Android系统，并在Android2. 2中提供了JIT，发展迅猛。
  - Dalvik VM只能称作虚拟机，而不能称作“Java虚拟机”，它没有遵循Java虚拟机规范
  - 不能直接执行Java 的Class 文件
  - 基于寄存器架构，不是jvm的栈架构。
  - 执行的是编译以后的dex (Dalvik Executable) 文件。执行效率比较高。
    - 它执行的dex (Dalvik Executable) 文件可以通过Class文件转化而来
  - 使用Java语法编写应用程序，可以直接使用大部分的Java API等。
  - Android 5.0使用支持提前编译(Ahead Of Time Compilation, AOT) 的ART VM替换Dalvik VM。

- ##### 把apk文件后缀改成.zip后解压可以查看项目目录结构

- #### Graal VM

  - 2018年4月，Oracle Labs公开了Graal VM， 号称●2018年4月，Oracle Labs公开了Graal VM， 号称"Run Programs Faster Anywhere", 勃勃野心。与1995年java的”write once， run anywhere "遥相呼应。
  - Graal VM在HotSpot VM基 础.上增强而成的跨语言全栈虚拟机，可以作为“任何语言的运行平台使用。语言包括: Java、 Scala、Groovy、Kotlin; C、C++、JavaScript、Ruby、 Python、 R等
  - 支持不同语言中混用对方的接口和对象，支持这些语言使用已经编写好的本地库文件
  - 工作原理是将这些语言的源代码或源代码编译后的中间格式，通过解释器转换为能被Graal VM接受的中间表示。
  - Graal VM提供Truffle工具集快速构建面向一种新语言的解释器。在运行时还能进行即时编译优化，获得比原生编译器更优秀的执行效率。
  - 如果说HotSpot有一天真的被取代， Graal VM希望最大。但是Java的软件生态没有丝毫变化。

- #### 类加载子系统

  - Class Loader
  - 负责把class文件加载到内存当中
  - 类加载器子系统负贵从文件系统或者网络中加载Class文件，class 文件在文件开头有特定的文件标识。
  - ClassLoader只负责class文件的加载，至于它是否可以运行，则由Execution Engine决定。
  - 加载的类信息存放于一块称为方法区的内存空间。除了类的信息外，方法区中还会存放运行时常量池信息，可能还包括字符串字而量和数字常量(这部分常量信息是Class文件中常量池部分的内存映射)

```java
//P31 0:00 ----2020年7月23日20:29:05
```

- #### 类的加载器

  - 引导类加载器（启动类加载器）
    - Bootstrap（非java语言编写）
    - 嵌套在JVM内部
    - 它用来加载Java的核心库(JAVA HOME/jre/lib/rt.jar.resources . jar或sun . boot.class.path路径下的内容) , 用于提供JVM自身需要的类
    - 加载扩展类和应用程序类加载器，并指定为他们的父类加载器
    - 出于安全考虑，只加载包名为java/javax/sun等开头的类
    - 获取能够加载的类的路径
      - sun.misc.Launcher.getBootstrapClassPath().getURLs()
  - 自定义类加载器
    - 所有派生于抽象类ClassLoader的类加载器都划分为自定义类加载器
    - 层级关系
      - 扩展类加载器 Extension ClassLoader
        - 虚拟机自带
        - 加载lib/ext目录下的类
        - 如果自定义的类放在此目录下，也会被扩展类加载器加载
        - 获取加载路径
          - String extDirs = System.getProperty("java.ext.dirs");
      - 系统类加载器 System ClassLoader
        - 也叫应用程序类加载器
        - 加载classpath/系统属性java.class.path下的类
      - 自定义类加载器 Custom ClassLoader
    - 用户自定义类默认由系统类加载器加载
    - java核心API由引导类加载器加载

```java
//P33 0:00 ----2020年7月24日08:23:04
```

- #### sun.misc.Launcher是java虚拟机的入口应用

- #### 获取ClassLoader的途径

  - class.getClassLoader()
  - Thread.currentThread().getContextClassLoader()
  - ClassLoader.getSystemClassLoader()
  - DriverManager.getCallerClassLoader()

- #### 双亲委派机制

  - Java虚拟机对class文件采用的是按需加载的方式，也就是说当需要使用该类时才会将它的class文件加载到内存生成class对象。而且加载某个类的class文件时，Java 虛拟机采用的是双亲委派模式，即把请求交由父类处理，它是一种任务委派模式。 
  - 工作原理
    - 如果一个类加载器收到了类加载请求，它并不会自己先去加载，而是把这个请求委托给父类的加载器去执行;
    - 如果父类加载器还存在其父类加载器，则进一步向上委托，依次递归，请求最终将到达顶层的启动类加载器;
    - 如果父类加载器可以完成类加载任务，就成功返回，倘若父类加载器无法完成此加载任务，子加载器才会尝试自己去加载，这就是双亲委派模式。
  - 优势
    - 避免类的重复加载
    - 保护程序安全，防止核心API被随意篡改
  - 沙箱安全机制

```java
//P41 0:00 ----2020年7月24日18:42:08
```



- #### PC寄存器

  - ###### 是对物理寄存器的一个模拟，是软件层面的东西

  - 用来存储指向下一条指令的地址

  - 内存空间很小，运行速度最快

  - 每个线程有一份，是线程私有的

  - 如果是native方法，则存储undefine

```java
//P44 0:00 ----2020年7月25日16:41:34
```

- #### 栈是运行时的单位，堆是存储的单位

  - 栈解决程序的运行问题，即程序如何执行，如何处理数据
  - 堆解决的是数据存储的问题，即数据怎么放，放哪

- #### Java虚拟机栈

  - 早期也叫Java栈
  - 线程私有
  - 里面保存栈帧
  - 一个栈帧对应一个java方法
  - 栈的生命周期和线程一致
  - 访问速度仅次于程序计数器
  - 不存在垃圾回收问题
  - java虚拟机规范允许java栈的大小是动态的或者固定不变的

- #### OOM

  - out of memory
  - 内存溢出

- #### StackOverFlowError

  - java虚拟机规范允许java栈的大小是动态的或者固定不变的
  - 如果采用固定大小的java虚拟机栈，那每一个线程的java虚拟机栈容量可以在线程创建的时候独立选定，如果线程请求分配的栈容量超过java虚拟机栈允许的最大容量，java虚拟机将会抛出一个StatckOverFlow异常
  - 如果java虚拟机栈可以动态扩展，并且在尝试扩展的时候无法申请到足够内存，或者在创建新的线程时没有足够的内存区创建对应的虚拟机栈，那虚拟机将会抛出一个OutOfMemoryError异常

- #### 设置栈的大小

  - 调节虚拟机运行参数VM Options -Xss大小
  - 看文档
    - https://docs.oracle.com/en/java/javase/11/tools/java.html#GUID-3B1CE181-CD30-4178-9602-230B800D4FAE

```java
//P46 0:00 ----2020年7月26日15:27:24
```



- #### 栈中存储什么

  - 每个线程都有自己的栈，栈中的数据都是以栈帧的格式存在
  - 每一个方法对应一个栈帧
  - 栈帧是一个内存区块，是一个数据集，维系着方法执行过程的各种数据信息

- #### 栈帧中有什么

  - 局部变量表
    - Local Variables
    - 定义为一个数字数组
    - 主要用来存储方法参数和定义在方法体内的局部变量
      - 基本数据类型
      - 对象引用
      - returnAddress类型
    - 建立在线程的栈上，不存在数据安全问题
    - 所需容量大小是在编译期确定下来的
    - 并保存在方法的Code属性的maximum local variables数据项中。在方法运行期间是不会改变局部变量表的大小的  
  - 方法返回地址
    - Return Address
  - 操作数栈
    - Operand Stack
  - 动态链接
    - Dynamic Linking
  - 一些附加信息

```java
//P50 0:00 ----2020年7月27日11:56:20
```

