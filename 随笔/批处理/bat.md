- #### echo

  - 打开回显挥着关闭请求回显功能，或显示消息

  - @echo off 关闭提醒信息

  - demo

    ```
    [@]echo 哈哈哈哈 
    //输出哈哈哈哈
    //有@修饰时可以不显示前面命令
    pause
    //暂停
    ```
    
  - 也可以输出变量，但要以 ‘%var_name%’的形式包裹变量

- #### del

  - del 文件名 删除文件
  - /P 删除每一个文件之前提示确认
  - /F 强制删除只读文件
  - /S 从所有子目录删除指定文件
  - /Q 安静模式，删除全局通配符时，不需要确认

- #### cls

  - 清除屏幕

- #### rem

  - 注释命令，一般用来给程序加上注释
  - 该命令后的内容不被执行，但能回显

- #### :: 

  - 也能起到rem的作用
  - 但有两个注意点
    - 不会回显

- #### set /a 算术表达式 ：计算表达式并输出结果

- #### = 赋值运算符

  - var_name = 1+2
  - 变量名不需要声明

- #### 重定向运算

  - \> 和 \< 将值覆盖到目标文件

    - demo

      ```java
      echo "hello world" > 'filepath/filename.txt'
      ```

      

  - \>\> 和 \<\< 将值追加到目标文件

    - demo

      ```java
      echo "hello world" >> "filepath/filename.txt"
      ```

      

  - type filename:查看文件内容

- #### 多命令运算

  - &和| 不会短路
  - &&和|| 会发生短路
  - 操作数为命令

- #### 管道命令

  - A|B ：A命令的输出会变成B命令的输入进而继续执行B命令

- #### find：查找文件

  - 结合管道命令demo

    ```java
    dir|find ".txt" //可以
    ```

```java
//P5 03:04 ----2020年8月25日18:15:10
```

