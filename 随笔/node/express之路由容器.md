# express之路由容器

- ### express提供了一种更好的方式来包装路由

  - ###### 包装:

    ```javascript
    let express = require('express')
    let router = express.Router()
    
    router.get('路径1',function(request,response){
        //路径1请求处理
    })
    router.get('路径2',function(request,response){
        //路径2请求处理
    })
    ......
    
    module.exports = router
    ```

  - ###### 使用(挂载):

    ```javascript
    在入口文件中:
    let router = require('包装文件路径')
    app.use(router)
    ```

    

