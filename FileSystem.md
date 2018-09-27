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
3.  **读取文件**  
```将文件中的内容读取到buffer对象中。```  
``` javascript
/*
* fs.read(fd, buffer, offset, length, position, callback)
*   fd : 通过open方法成功打开一个文件返回的编号
*   buffer : buffer对象
*   offset : 新内容添加到buffer中的起始位置
*   length : 添加到buffer中内容的长度
*   position : 读取的文件中的起始位置
*   callback : 回调
*       - err :   报错内容
*       - len :   buffer的长度
*       - newBf : 新的buffer对象
* */
```
``` javascript
var fs = require('fs');
fs.open('1.txt','r',function (err, fd) {

    if(err){
        console.log("打开错误");
    }else{
        var buf = new Buffer("123456789");
        console.log(buf); // <Buffer 31 32 33 34 35 36 37 38 39>

        fs.read(fd, buf, 0, 4, null, function (err, len, newBf) {
            console.log( buf );  // <Buffer 61 62 63 64 35 36 37 38 39>
            console.log( len );  // 4
            console.log( newBf );// <Buffer 61 62 63 64 35 36 37 38 39>
        })

    }
});
```
