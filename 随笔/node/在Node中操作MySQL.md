# 在Node中操作MySQL

- 安装

  - ```shell
    npm install mysql --save
    ```

- 引入

  - ```javascript
    let mysql = require('mysql')
    ```

- 创建连接

  - ```javascript
    let connection = mysql.createConnection({
        host:'localhost',
        user:'root',
        password:'****',
        database:'my_db'
    });
    ```

- 连接

  - ```javascript
    connection.connect()
    ```

- 执行数据操作

  - ```javascript
    connection.query("任意sql语句",function(err,data,fields){
        if(err){
            //错误处理语句
        }
        else{
            
        }
    })
    ```

  - 

- 关闭连接

  - ```javascript
    connection.end();
    ```

    