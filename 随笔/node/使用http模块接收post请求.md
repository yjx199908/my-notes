- #### 普通POST-----------------------------------------

- ##### 需要用到的依赖：

  - http
  - querystring

- ##### 基本框架不变

  ```js
  let http = require("http")
  
  let server = http.createServer()
  
  server.on("request",function(request,response){
     //同样是在这获取数据
  })
  
  server.listen(3000,function(){
      console.log("服务器启动成功")
  })
  ```

  

- ##### 需要额外监听request的data和end事件（监听同样是使用on，且注意data和end事件是属于request的，而不是server的）

  ```js
  function(request,response){
      let post = ""
      
      //data事件：前端post过来的数据可能需要分好几块接受，每接收一块会触发一次request的data事件
      request.on("data",function(chunk){
          console.log(chunk.toString())
          post += chunk
      })
      
      //end事件：前端post过来的数据全部都接收完后会触发request的end事件
      request.on("end",function(){
          
          //querystring的parse方法会把 v1=k1&v2=k2&v3=k3... 形式的字符串转换成{v1:k1,v2:k2,v3:k3}的对象形式
          post = querystring.parse(post)  
          console.log(post) //传到后台的所有数据已经保存在post对象中，可以通过 post.属性名 的方式获取
          // response.setHeader("Content-type","text/html;charset=utf-8")
          // response.end("后台观察结果")
      })
  }
  ```

- #### 带文件的POST-----------------------------------------

- ##### 需要的依赖

  - http
  - formidable

- ##### 基本框架同上

- ##### 需要多做的步骤是将使用formidable依赖中写好的方法将request转化

  ```js
  function(req, res){
      const form = new formidable.IncomingForm();
      form.uploadDir = 文件保存路径;//上传文件的保存路径
      form.keepExtensions = true;//保存扩展名
      form.maxFieldsSize = 20 * 1024 * 1024;//上传文件的最大大小
      form.parse(req, function(err, fields, files){
          if (err) {
              throw err;
          }
          //fields对象中保存着除了表单文件域之外的数据
          //files对象中保存
      })
  };
  ```

  

