# Express

- ##### 原生的http在某些方面表现不足以应对我们的开发需求，所以需要框架来提高效率

- ##### 在Node中，有许多Web开发框架

- ##### 官网首页 expressjs.com

- ##### 安装:npm install express --save

- ##### 无需处理编码问题

- ##### 使用

  - ###### 导入：let express=require('express')

  - ###### 创建应用实例：let app=express()

  - ###### 公开文件夹: app.use('公开后用于客户端访问的目录路径',express.static('公开的服务器本地资源目录'))

    - ###### *这样处理后,在浏览器端通过'公开后用于客户端访问的目录路径'可以访问到’公开的服务器本地资源目录‘中的全部内容*

    - ###### 例如:(在本地服务器3000端口下,开放与脚本同层目录下的YY文件夹)

    - ```javascript
      服务器端:app.use(['/public/'],express.static('./YY/'))
      ```

    - ```javascript
      浏览器访问:http://localhost:3000/[public]...
      ```

    - ###### *如果use的第一个参数省略,则直接访问此公开目录下的资源*

    - ###### *可以理解为use第一个参数就是公开目录的别名*

  - ###### 编写请求处理方法:

    - ```javascript
      app.get('请求路径名',function(request,response){}) ：get请求处理
      ```

    - ```javascript
      app.post('请求路径名',function(request,response){}) ：post请求处理
      ```

    - ###### 在处理方法中:

      - ###### *url解析后的query属性可以直接通过request.query获取*

      - ###### *使用模板引擎有更好的方式*

      - ###### *响应数据可以用response.send(data)的方式发送,但只能使用一次此方法，原始的write和end也可以使用*

      - ###### *使用express后重定向:response.redirect('url')*

- ##### 监听端口 ：app.listen(端口号,function(){//成功回调方法})