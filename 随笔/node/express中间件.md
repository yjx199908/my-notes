# express中间件(middleware)

- ##### 可以把一个复杂的问题分解成一个接一个的处理流程

- ##### 任意中间件只要在回调函数中调用了带参数的next方法，则流程会直接跳到下一个错误处理中间件,忽略中间所有步骤

- ##### 中间件是用来处理请求的，本质就是个函数

- ```javascript
  配置:app.use(['请求路径',]function(request,response,next){
      //request:
      	处理request
      //response:
      	处理response
      //next:
      next() //流程走向下一个请求路径符合的中间件
  })
  ```

  

- ##### 分类:

  - ###### 不关心请求路径和请求方法的中间件，任何请求都会进入此函数

  - ###### 第一个参数为请求路径的中间件,则此中间件只处理对以此路径开头的所有路径的请求

  - ###### 严格匹配路径的中间件

    - ```javascript
      app.get('/请求路径',function(request,response,next){
          
      })
      ```

      

    - ```javascript
      app.post('/请求路径',function(request,response,next){
          
      })
      ```

  - ###### 回调函数拥有四个参数的中间件:专门用来处理错误

    - ```javascript
      app.use(function(err,request,response,next){
          
      })
      ```

- ##### 小应用:

  - ###### 在所有中间件的最后加上不关心请求路径和方法的中间件,可以处理资源不存在的请求路径(即404)