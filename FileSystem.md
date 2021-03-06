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
```**如果filename文件存在，则直接写入data数据，如果不存在，则创建新文件后写入**```  
3.1 **异步写入文件**  
``` javascript
fs.writeFile(filename, data, [option], callback)
```
``` javascript
var fs = require('fs');
fs.writeFile('1.txt', "sdafasdfs", function () {
   console.log(arguments);//{ '0': null } 返回值err为null，说明没有错误，正常
});
```
3.2 **同步写入文件**  
``` javascript
fs.writeFileSync(filename, data,  [option])
```
3.3 **将数据添加到文件尾部**  
``` javascript
fs.appendFile(filename, data, [option], callback)
```
4. **检测指定路径的文件是否存在**  
4.1 **同步方式**
``` javascript
fs.existsSync(path)
```
``` javascript
if (fs.existsSync('1.txt')){
    console.log('存在')
}
```
4.2 **异步方式**  
``` javascript
fs.exists(path, callback)
```
``` javascript
var fs = require('fs');
fs.exists('1.txt', function (isExists) {
    if(isExists){
        console.log("存在")
    }else{
        console.log("不存在")
    }
});
```
5. **读取文件**
``` javascript
fs.readFile(filenmae, [options], callback)
```
``` javascript
var fs = require('fs');
fs.readFile('1.txt',function (err, data) {
    if(err){
        console.log("文件读取失败");
    }else{
        console.log(data.toString());//读取到的为buffer对象形式，所以要转化为字符串的形式
    }
});
```
6. **关闭文件**  
``` javascript
/*
* fs.close( fd,callback )
* */
```
7. **删除文件**
``` javascript
/*
* fs.unlink( path,callback )
* */
```
8. **文件重命名**
``` javascript
/*
* fs.unlink( 旧文件名, 新文件名, callback )
* */
```
**关于文件夹的操作看官方文档**  
[Node.js v10.8.0 文档](http://nodejs.cn/api/)

9. **文件夹的读取**
``` javascript
var fs = require('fs');
fs.readdir('../FileSystem',function (err, fileList) {
    //遍历当前文件夹下的文件
    fileList.forEach(function (f) {
    //stat可返回文件的信息
        fs.stat(f, function (err, info) {
            switch (info.mode) {
                //通过文件信息的mode，来判断出他是文件还是文件夹
                case 16822:
                    console.log('[文件夹]' + f);
                    break;
                case 33206:
                    console.log('[文件]' + f);
                    break;
            }
        });
    })
});
```
