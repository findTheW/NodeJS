## FileSystem 文件系统
**每次都要先引入文件系统模块，var fs = require('fs')，才能进行后续操作**   
1.**异步打开一个文件**
/*   
* fs.open(path, flags, [mode], callback)
*   path : 要打开文件的路径
*   flags : 打开文件的方式 读/写
*   mode : 设置文件的模式 读/写/执行 4/2/1
*   callback : 回调
*       - err : 文件打开失败的错误保存在err里面，如果成功err为null
*       - fd : 被打开文件的标识， 有点类似定时器
* */
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
