- #### 支持的构建

  - 清理
    - 把之前项目编译的东西删除掉，为新的编译代码做准备
  - 编译
    - 把程序源代码编译为执行代码
    - 批量操作
  - 测试
    - 可以执行测试程序代码，验证你的功能是否正确
    - 批量操作
  - 报告
    - 生成测试结果的文件
  - 打包
    - 把你的项目中所有的class文件，配置文件等所有资源放到一个压缩文件里
    - 这个压缩文件通常就是项目的结果文件，通常是jar包
    - 对于web应用，压缩文件是扩展名是.war
  - 安装
    - 把上一步生成的文件jar,war安装到本地仓库

  - 部署
    - 把程序安装好可以执行

- #### 核心概念

  - POM
    - 一个文件
    - 项目对象模型
  - 约定的目录结构
    - 有规定
  - 坐标
    - 唯一的字符串
    - 用来表示资源的
  - 依赖管理
    - 管理你的项目可以使用的jar文件
  - 仓库管理
    - 你的资源存放的位置
  - 生命周期
    - 构建项目的过程
  - 插件和目标
    - 执行maven构建的时候用的工具是插件
  - 继承

  - 聚合

- #### 安装和配置

  - 子目录bin,主要是mvn.cmd
  - 子目录conf,主要是settings.xml
  - 配置环境变量
  - 验证：在cmd中输入 mvn -v命令

- #### 约定的目录结构

  - Whole

    - src
      - main ：放主程序java代码和配置文件
        - java ：放程序包和包中的java文件
        - resources ：放置配置文件
      - test ：放测试程序，可以没有
        - java ：测试程序中要使用的程序文件
        - resources ：测试程序中要使用的配置文件

    - pom.xml ：maven的核心文件

```java
//P7 0:00 ----2020年8月25日16:22:58
```

- ##### 解决资源文件没能成功打包的问题

  - 在pom.xml文件中的project标签中添加标签配置

    ```xml
    <build>
        <resources>
        	<resource>
            	<directory>src/main/resources</directory>
                <includes>
                    <include>...</include>
                	<include>**/*.properties</include>
                    <include>**/*.xml</include>
                    <include>...</include>
                </includes>
                <filtering>true</filtering>
            </resource>
            <resource>
            	<directory>src/main/java</directory>
                <includes>
                    <include>...</include>
                	<include>**/*.properties</include>
                    <include>**/*.xml</include>
                    <include>...</include>
                </includes>
                <filtering>true</filtering>
            </resource>
        </resources>
    </build>
    ```

    