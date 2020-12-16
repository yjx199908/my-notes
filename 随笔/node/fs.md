## fs

## -引入fs

### 	-let fs=require('fs')

## -使用

### 	-fs.readFile('路径',['utf8',]function(err,data){//data:二进制对象})

### 	-fs.writeFile('路径','写入内容',function(err){//err:执行成功时为null})

### -fs.unlink('路径',err=>{}) //文件删除

### -fs.mkdir('path'[,mode],callback) //创建目录

### -fs.readdir(path[,options],callback) //读取目录