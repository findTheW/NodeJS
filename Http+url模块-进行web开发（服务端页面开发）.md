### 通过加载http模块，创建服务器
``` javascript
/*
* 1. 用户通过浏览器发送一个http请求到指定的服务器主机；
* 2. 服务器接收到请求，对请求进行分析和处理；
* 3. 服务器处理完成后，返回对应的数据到用户机器；
* 4. 浏览器接受到服务器返回的数据，并根据接收到的数据进行分析和处理。
*
* 客户端       服务器端
*  由客户端发送一个http请求到指定服务端 -> 服务端接受并处理请求 -> 返回数据到客户端
* */
```
``` javascript
//加载http模块
var http = require('http')

//通过模块里的createServer创建并返回一个web服务器对象
var server = http.createServer();

//开启HTTP服务器监听连接
server.listen(8080, 'localhost');//不带参数则系统随机分配端口

//表示服务器是否正在监听连接。
server.on('listening', function () {
    console.log("listening...")
});

//当有客户端发送请求到该主机和端口的时候触发
server.on('request', function ( req, res) {
    console.log("有客户端请求");
    //req:（http.IncomingMessage的一个实例）通过他可以获取这次请求的一些信息，比如头信息，数据等

    //#请求# 接受到客户端的请求信息
    //console.log(req);包含了好多信息，看官网的 http.IncomingMessage 部分

    //发送一个响应头给请求。 状态码是一个三位数的 HTTP 状态码，如 404。 最后一个参数 headers 是响应头。 第二个参数 statusMessage 是可选的状态描述。
    res.writeHead(200, "fuck", {
        //以text/html处理写入文本
        'content-type' : 'text/plain'
    });

    //#响应# 发送一个数据块到响应正文 http.ServerResponse
    res.write('<h1>hello</h1>');
    //当所有征文和头信息发送完时调用这个方法，告诉服务器数据已经发送完了，必须在每次发送完后调用
    res.end();
});

console.log(server.address()); // { address: '::', family: 'IPv6', port: 8080 }
```
### url模块
``` javascript
var http = require('http');
var url = require('url');

var server = http.createServer();

server.on('request', function (req, res) {
    //req.url : 访问路径
        // ?后面的部分 query string 查询字符串
    //console.log(req.url); // 返回的是 /b 访问路径的后面部分

    //会将url解析成一个对象，分成好几部分
    //var urlStr = url.parse( "http://baidu.com/a/d" );

    var urlStr = url.parse( req.url );
    console.log(urlStr)
});
server.listen(8080, 'localhost');
```
