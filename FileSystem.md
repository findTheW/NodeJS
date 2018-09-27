## FileSystem 文件系统
1. **每次都要先引入文件系统模块，才能进行后续操作**
``` javascript
var fs = require('fs');
```
2.1 **异步打开一个文件**  
``` javascript
/*   
* fs.open(path, flags, [mode], callback)
*   path : 要打开文件的路径
*   flags : 打开文件的方式 读/写
*   mode : 设置文件的模式 读/写/执行 4/2/1
*   callback : 回调
*       - err : 文件打开失败的错误保存在err里面，如果成功err为null
*       - fd : 被打开文件的标识， 有点类似定时器
* */
```
``` javascript
fs.open('1.txt','r',function (err, fd) {
    if(err){
        console.log("文件打开失败");
    }else{
        console.log("文件打开成功");
        console.log( fd )
    }
});
console.log('ok');
```
```异步操作需要一定的时间，所以会先输出ok，在执行打开文件后的操作。```  
2.2 **同步打开一个文件**  
``` javascript
/*   
* fs.openSync(path, flags, [mode])
*   path : 要打开文件的路径
*   flags : 打开文件的方式 读/写
*   mode : 设置文件的模式 读/写/执行 4/2/1
* */
```
``` javascript
var fs = require('fs');
var fd = fs.openSync('1.txt','r');
console.log(fd); //返回值是fd，也就是打开文件的标识
```
```同步方式打开会阻塞进程，所以要等同步方式后再执行下一步。```  
3. **写入文件**
>     如果filename文件存在，则直接写入data数据，如果不存在，则创建新文件后写入。
3.1 异步写入文件
``` javascript
fs.writeFile(filename, data, [option], callback)
```
``` javascript
var fs = require('fs');
fs.writeFile('1.txt', "sdafasdfs", function () {
   console.log(arguments);//{ '0': null } 返回值err为null，说明没有错误，正常
});
```
3.2 同步写入文件
``` javascript
fs.writeFileSync(filename, data, [option])
```
5. **关闭文件**  
``` javascript
/*
* fs.close( fd,callback )
* */
```
