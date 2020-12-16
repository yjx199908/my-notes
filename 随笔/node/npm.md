# npm

##### 1.新建项目时使用`npm init`命令初始化项目，并按照向导设置项目，完成后会自动生成package.json文件

##### 2.安装第三方包时使用`npm install(或i) 包名 --save`命令，安装完成后会自动在package.json文件中的dependencies属性中添加项

##### 3.当依赖的第三方包被删除时，可以使用`npm install`命令通过package.json文件的dependencies属性恢复项目

##### 4.npm uninstall 包名:删除包，不删除dependencies中的依赖项

##### 5.npm uninstall 包名 --save:包和依赖项都删除

##### 6.npm help :查看npm使用帮助

##### 7.npm 命令 --help 查看指定命令的使用方式



- ###### --save 简写:-S

- ###### install 简写:i

- ###### uninstall 简写:un



#### 解决npm被墙的问题

- ###### 安装淘宝的cnpm

```javascript
npm install --global cnpm
```

- ###### 接下来将npm 替换为cnpm



#### 不想安装cnpm又想使用淘宝的镜像源

```shell
npm install 包名 --registry=https://registry.npm.taobao.org
```

###### 但是每次手动这样加参数很麻烦，所以可以把这个选项加入配置文件中

```shell
npm config set registry https://registry.npm.taobao.org
```

###### 配置以后，所有的npm install将默认通过淘宝的服务器来下载



#### 查看配置信息

```shell
npm config list
```





