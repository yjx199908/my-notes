# -http

### 	-是node中专门提供的一个核心模块

### 	-需要用require引入 let http = require('http')

### 	-let server = http.createServer() //创建一个服务器实例

### 	-server.on('request',function(request,response){}) //注册请求事件

#### 		+request.url获取客户端请求路径（从/开始）

#### 		+响应语句的最后必须加response.end()，否则客户端会一直等待

#### 		+response.write(data) OR response.end(data) //data中必须是字符串或者二进制数据

#### +response.end([data])是一次响应结束的语句

#### 		

### 	-server.listen(3000,function(){/*服务器启动成功时被调用*/}) //绑定端口号



###### *在服务器端默认发送的数据时以utf8编码的,而浏览器默认情况下并不知道数据的编码格式,在这种情况下浏览器会按照操作系统的默认字符集为数据编码*

##### ***上述问题解决方案:设置响应头: response.setHeader('Content-Type','text/plain;<u>charset=utf-8</u>')***

