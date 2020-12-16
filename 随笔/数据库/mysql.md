- ##### mysql的数据存放在 C:\ProgramData\Mysql\Mysql Server x.x\data\下(x.x为你安装的版本) 

- ##### 引擎：INNODB和MYISAM的区别

  ![image-20201016085015701](D:\notes\随笔\note-imgs\image-20201016085015701.png)
  - ##### InnoDB在数据库表中只有一个*.frm文件，以及上级目录下的ibdata1文件

  - ##### MYISAM

    - *.frm    -表结构的定义文件
    - *.MYD  数据文件
    - *.MYI    索引文件

- ##### MD5加密函数：MD5()

- ##### 事务原则：ACID原则

  - 原子性：要么都成功，要么都失败
  - 一致性：事务前后的数据完整性保证一致
  - 隔离性：事物的隔离性是多个用户并发访问数据库时，数据库为每一个用户开启的事务，不能被其他事物的操作数据所干扰，事务之间要隔离
  - 持久性：事物一旦提交则不可逆

- ##### 事务隔离级别

  - 脏读：一个事务读取了另外一个事务未提交的数据
  - 不可重复度：在一个事务内读取表中的某一行数据，多次读取结果不同（不一定是错误，只是某些场合不对）
  - 虚读（幻读）：是指在一个事物内读取到了别的事务插入的数据，导致前后读取不一致（一般是多了一到多行）

- ##### 开启/关闭事务自动提交

  - set autocommit=0 ：关闭
  - set autocommit=1 ：开启（默认）

- ##### 事务开启：start transaction 

- ##### 事务提交：commit

- ##### 事务回滚：rollback

- ##### 保存点：savepoint

  - 设置保存点：savepoint [保存点名]
  - 回滚到保存点：rollback to savepoint [保存点名]
  - 撤销指定的保存点：release savepoint [保存点名]

- ##### 事务操作大致步骤

  ```sql
  set autocommit = 0;
  start transaction;
  ....
  commit
  set autocommit = 1;
  ```

- #### 索引

  - 官方定义：索引是帮助mysql高效获取数据的数据结构。提取句子主干，就可以得到索引的本质：索引是数据结构

  - 分类

    - 主键索引：Primary Key

      - 不可重复

    - 唯一索引：Unique Key

      - 可以有多个

    - 常规索引

      - 默认的，index,key关键字来设置

    - 全文索引：FullText

      - 在特定的数据库引擎下才有，MYISAM

      - 快速定位数据

      - 使用方式：

        ```sql
        select * from 表名 where match(列名[,列名[...]]) against(模式子串)
        ```

  - ##### 显示表中所有的索引信息：show index from 表名

- ##### 索引原则

  - 不是越多越好
  - 不要对经常变动数据加索引
  - 小数据量的表不需要加索引
  - 索引一般加在经常查询的字段上

- ##### 索引的数据结构

  - Hash类型的索引
  - Btree：InnoDB的默认数据结构

- ##### 分析sql执行的状况：explain select * from student

- ##### 字符串连接函数：concat()

- ##### 随机数生成函数：rand()

- ##### 小数向下取整函数：floor()

- ##### 简单存储函数创建语法：

  ```sql
  delimiter $$ //修改结束标识符
  create function 函数名(参数列表) //参数格式：参数名 参数类型,...
  returns 返回值类型
  begin
  	//定义变量语法：declare 变量名 变量类型 [default 默认值];
  	//while循环语法：while 循环条件 do
  	//					...
      //				end while;
  end $$ 
  demiliter ;
  ```

- ##### 权限管理

  - 创建用户：

    ```sql
    create user 用户名 identified by 密码字符串
    ```

  - 修改当前用户密码

    ```sql
    set password=password(密码字符串)
    ```

  - 修改指定用户密码

    ```sql
    set password for 用户名 = password(密码字符串)
    ```

  - 重命名

    ```sql
    rename user 原名 to 新名
    ```

  - 用户授权

    ```sql
    grant all privileges on *.* to 用户名 -- all privileges:除了给别人授权之外的所有权限
    ```

  - 查看指定用户的权限

    ```sql
    show grants for '用户名'@'主机名'
    ```

  - 撤销权限

    ```sql
    revoke all privileges on *.* from 用户名
    ```

- ##### 数据库备份

  - 导出（命令行）

    ```sql
    mysqldump -h主机名 -u用户名 -p密码 数据库名[ 表名1 表名2 表名3...] > 导出到那儿 -- 如果没有表名则直接导出数据库
    ```

  - 导入（先登录）

    ```sql
    use 数据库
    source sql文件 位置
    ```

  - 导入（不登录）

    ```sql
    mysql -u用户名 -p密码 库名<sql文件地址
    ```

- ##### 查看当前事务隔离级别：SELECT @@tx_isolation;

- ##### 设置当前事务隔离级别：

  ```sql
  set session transaction isolation level 事务隔离级别
  ```

- ##### 三大范式

  - 第一范式
    - 保证每一项不可再分
  - 第二范式
    - 满足第一范式
    - 每张表只描述一个实体
  - 第三范式
    - 满足第二范式
    - 其他列只能和主键直接相关，不能间接相关