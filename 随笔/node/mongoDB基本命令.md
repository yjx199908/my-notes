# mongoDB基本命令

- 查看数据库列表

  - ```shell
    show dbs
    ```

- 切换指定的数据库(及新建)

  - ```shell
    use 数据库名称 //如果没有会新建
    ```

- 查看当前操作的数据库

  - ```shell
    db
    ```

- 插入数据

  - ```shell
    db.集合名.insertOne({
    	'name':'Jack'
    }) 
    //将数据插入指定的集合
    ```

- 查看数据

  - ```shell
    show collections //显示当前数据库的集合
    ```

  - ```shell
    db.集合名.find() //显示此集合中的所有数据
    ```

  -  