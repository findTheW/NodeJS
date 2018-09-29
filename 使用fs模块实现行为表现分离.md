**获取url，然后通过解析url地址，使其能够打开到相应的html文件**
``` javascript
var http = require('http');
var url = require('url');
var fs = require('fs');

var server = http.createServer();

//__dirname 表示当前文件夹路径
//进入当前文件夹
var htmlDir = __dirname + '/html/';

//监听客户端请求
server.on('request', function ( req, res ) {
    //将路径进行分析，分析成对象
    var urlStr = url.parse( req.url );
    switch(urlStr.pathname){
        case '/' :
            //首页
            sendData( htmlDir + 'index.html', req, res);
            break;

        case '/user' :
            //用户首页
            sendData( htmlDir + 'user.html', req, res);
            break;

        default :
            //处理其他情况
            res.writeHead(404,{
                'content-type': 'text/html;charset=utf-8'
            });
            res.end('<h1>页面不存在</h1>');
            break;
    }
});

function sendData(file, req, res){
    //读取文件页面
    fs.readFile(file, function (err, data) {
        if(err){
            res.writeHead(404, {
                'content-type' : 'text/html;charset=utf-8'
            });
            res.end('<h1>页面被吃掉了</h1>')
        }else{
            //写入响应头信息，响应内容为text/html形式
            res.writeHead(200, {
                'content-type' : 'text/html;charset=utf-8'
            });
            res.end(data)
        }
    })
}

//开启监听
server.listen(8100, 'localhost');
```
