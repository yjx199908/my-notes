# art-template与express结合

- ### 安装:	

  ```shell
  npm install art-template --save
  ```

  ```shell
  npm install express --save
  ```

  ```shell
  npm install express-art-template --save
  ```

  

- ### 使用: 

  ```javascript
  app.engine('art',require('express-art-template'))
  //当渲染art后缀名的文件时，使用express-art-template模板引擎，有需要可以将第一个参数改成其他后缀
  //express-art-template是专门用来在express中把art-template正和岛express中
  //虽然不需要手动加载,但是也必须安装art-template,因为express-art-template依赖了art-template
  ```

  + ###### express为response对象提供了一个方法:render

  + ###### render方法默认是不可以使用，但是如果给服务器实例配置了模板引擎就可以使用了

  + ###### *response.render(‘html模板’,{模板数据})* //同时具有渲染和发送的功能

  + ###### 第一个参数不可以写路径,只能写文件名,默认会去项目中的views目录查找该模板文件

  + ###### 也就是说express有一个约定: 开发人员把所有的视图文件都放到views目录中

  + ###### 如果想改约定，即不想把视图名称放到views目录中，则可以使用app.set('views',修改后的render函数的默认路径)

