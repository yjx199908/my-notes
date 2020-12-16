# 在Node中操作mongoDB数据

- ##### 安装第三方模块:mongoose(基于mongodb包再一次做了封装)

  + ```shell
    npm install mongoose --save
    ```

- ##### 引入

  - ```javascript
    let mongoose = require('mongoose')
    ```

- ##### 连接数据库

  - ```javascript
    mongoose.connect('DBMS://主机名/数据库')
    ```

- ...

  - ```javascript
    mongoose.Promise = global.Promise
    ```

- ##### 创建集合架构

  - ```javascript
    //例如:
    let Schema = mongoose.Schema 
    
    let CatSchema = new Schema({
        name:{
            type:String,
            required:true
        },
        age:{
            type:Number,
            required:true
        },
        birth:{
            type:Date,
            required:false
        }
      ...
    })
    ```
    
  - 

- ##### 创建模型(设计数据库)

  - ```javascript
    例如:let Cat = mongoose.model('Cat',CatSchema) 
    //mode方法的第一个参数为大写的名词单数，mongoose会自动生成以对应小写复数为名字的集合
    //例如这里会生成cats集合
    //mode会返回一个模型构造函数
    ```

- ##### 实例化

  - ```javascript
    例如:let Kitty = new Cat('Cat',{name:'zildjian',age:18})
    ```

- ##### 持久化保存实例

  - ```javascript
    Kitty.save(function(err){
        if(err){
            //保存失败处理
            return
        }
        else{
            //存储成功执行
        }
    })
    ```

- ##### 查询数据(查不到返回null)

  - ```javascript
    //查找是所有符合条件的数据，返回数组
    Cat.find([{
    	name:'zildjian' //按照一种或多种属性查找	
    },]function(err,data){
    	if(err){
            //查询失败处理
        }
        else{
            //查询成功处理
        }
    })
    ```

  + ```javascript
    //查找所有符合条件的第一条数据
    Cat.findOne([{
       name:'aaaaa' 
    },]function(err,data){
        if(err){
        	//查询错误处理
        }else{
            //查询成功处理
        }
    })
    ```

- ##### 删除数据

  - ```javascript
    Cat.remove([{
        name:'aaaa'
    },]function(err,data){
        if(err){
            //删除失败处理
        }
        else{
            //删除成功处理
        }
    })
    ```

- ##### 更新数据

  - ```javascript
    Cat.findByIdAndUpdate('数据Id',{
        name:'bbbbb'
    },function(err,data){
        if(err){
            //数据更新失败处理
        }
        else{
            //数据更新成功处理
        }
    })
    ```

  - 