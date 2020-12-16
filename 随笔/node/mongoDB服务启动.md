# mongoDB

- #####  安装

  - ```shell
    https://www.mongodb.com/download-center/community
    ```

- ##### 配置环境变量

  - ###### 将安装后的bin目录路径放到环境变量的Path里面

- ##### 第一次启动服务之前创建存储目录

  - ###### 在运行mongod命令的磁盘根目录下创建data目录

  - ###### 在data目录下创建db目录

- ##### 启动服务

  - ###### 管理员身份打开命令行

  - ###### cd到具有/data/db目录的磁盘

  - ###### 执行mongod命令

  - ###### 服务已启动

  + ###### *此方式打开的服务再关掉cmd窗口或者ctrl C后自动停止*

- ##### 更改默认的数据存储路径

  - ```shell
    mongod --dbpath=数据存储目录路径
    ```

    