## NodeJS的基础知识
1. 以 EcmaScript 为基础，扩展为操作os（操作系统）、file（文件系统）、net（网络）、database（数据库）等...
2. node中的顶层对象，global对象。
3. 一个文件就是一个模块，每个模块都有自己的作用域
4. 在文件中声明的变量并不是全局的，而只是模块中的局部变量
   ``` javascript
   var a = 100; //当前模块变量  
   global.a = 200; //全局变量  
   console.log(a);  
   console.log(global.a);  
   console.log(__filename); //当前模块被解析过后的绝对位置
   ```
5. 模块加载系统 ： require("模块地址")  
6. 跨模块访问变量 :  
   ① （不推荐）用global.变量名存储变量，会污染全局变量  
   ② 使用模块对象 module （里面有一个子对象exports，可以通过这个子对象对变量进行对外暴露）  
    [^1]注意是添加属性，而不是将exports对象覆盖 
   ``` javascript
   /* 1.js */
   var a = 100;
   module.exports.a = a;//将变量放在module的exports对象中向外暴露
   /* 2.js */
   var req = require("./1.js"); //返回值是1.js的exports对象
   console.log(req); // {a: 100}
   ```
7. 在模块作用域中，有一个内置模块对象exports，作用跟module.exports是一样的
   ``` javascript
   console.log(exports === module.exports); //true
   ```

## Global对象中的部分属性
1. __filename: 返回当前模块文件解析后的绝对路径，虽然在global里，但是这个属性是模块作用域下的。 （文件路径）
2. __dirname:  返回当前模块文件所在目录解析后的绝对路径，虽然在global里，但是这个属性是模块作用域下的。（文件所在目录路径）
   ```javascript
   //调用的时候不能加global，而是直接使用
   console.log(__filename);
   console.log(__dirname);
   ```
3. process：是一个全局对象，通过这个对象的属性和方法，可以使我们对当前进行的程序的进程进行访问和控制。
   ①process.argv：包含命令行参数的数组（第一参数是node，第二个是.js文件的名称，其余的是命令行传入的参数）；  
   ②execPath：开启当前进程的绝对路径；  
   ③env：返回用户环境信息；  
   ④pid：当前运行程序进程的pid；  
   ... 
   ``` javascript
   //默认情况下，输入流是关闭的，要监听处理输入流数据，首先要开启输入流
   process.stdin.resume();
   //输出流，输出到命令行
   process.stdout.write("请输入a的值：");
   var a,b;
   //用于监听用户的输入数据
   process.stdin.on("data",function (chunk) { //接受到的返回值chunk是输入的值
       if(!a){
           a = Number(chunk);
           process.stdout.write("请输入b的值：");
       }else{
           b = Number(chunk);
           process.stdout.write("a+b的和为：" + (a + b))
       }
   });
   ```
4. Buffer类：操作二进制数据流  
   (1)**以下几个都被废弃，去官网找代替方法**  
   ①~new Buffer(size)~ : 创建一个Buffer对象，并为这个对象分配大小。长度是固定的，不能进行修改。【结果为十六进制】
      ``` javascript
      var bf = new Buffer(5);
      console.log(bf); // <Buffer 00 00 00 00 00>
      bf[6] = 10; //超出Buffer对象长度，内容不会变化
      console.log(bf); // <Buffer 00 00 00 00 00>
      ```
   ②~new Buffer(array)~ :  创建一个Buffer对象，使用一个8位字节array数组。
      ``` javascript
      var bf = new Buffer([1,2,3]);
      console.log(bf);//<Buffer 01 02 03>
      ```
   ③~new Buffer(str,[encoding])~ : 创建一个Buffer对象，其中包含着给定的str字符串，encoding编码方式默认是"utf-8"。
      ``` javascript
      var bf = new Buffer('maiov','utf-8');
      console.log(bf);//<Buffer 6d 61 69 6f 76>
      for(var i=0;i<bf.length;i++){
          console.log( String.fromCharCode(bf[i]) );
      } //输出的是mai三个字母的Ascall码值，再转为正常的字母形式
      ```
   (2) bf.length ：返回的是字符的长度，而不是字符串的  
      ``` javascript
      var bf = new Buffer('mai','utf-8');
      console.log(bf.length);//3 
      var buf = new Buffer('小明','utf-8')
      console.log(buf.length); //6，一个文字等于三个字符
      ```
   (3) buf.write(要写入的字符串，从第几位写入，写入的长度，写入字符串的编码)
      ``` javascript
      var buf = new Buffer("abcd");
      console.log(buf);    //<Buffer 61 62 63 64>
      buf.write("XYZ",1,2);
      //<Buffer 61 58 59 64> 偏移量1，所以从第二位开始写入，写入长度为2，所以只写入两个
      console.log(buf);
      ```
   (4) buf.toString(编码，开始位置，结束位置)  
      buf.toJSON()
      ``` javascript
      //将编码转换为string字符串
      var buf = new Buffer("abcd");
      console.log(buf.toString()); //abcd
      console.log(buf.toString('utf-8',0,2)); //ab 
      //转为JSON格式
      console.log(buf.toJSON()); //{ type: 'Buffer', data: [ 97, 98, 99, 100 ] }
     ```
   (5)buf.slice(开始位，结束位) ： 对buffer进行截取
      ``` javascript
      var buf = new Buffer("abcd");
      console.log(buf.slice(2,3)); //c
      ```
      [^2] 注意，修改这个新建的 Buffer 切片，也会同时修改原始的 Buffer 的内存，因为这两个对象所分配的内存是重叠的。
   (6)被拷贝的buffer→buf.copy(拷贝进的buf，开始拷贝进的偏移量，开始拷贝的偏移量，结束拷贝的偏移量)
   
### Buffer的类方法（静态方法）
- Buffer.isBuffer（对象）:   
   判断一个对象是否是Buffer对象，返回true或false。
- Buffer.byteLength(对象) :  
   返回buffer对象的字节长度。
- Buffer.concat(buffer对象数组[,规定拼接后的长度]) :  
   将数组里的buffer对象拼接起来。
   ``` javascript
   var buf1 = new Buffer("abc");
   var buf2 = new Buffer("def");
   var list = [buf1,buf2];
   console.log(list);// [ <Buffer 61 62 63>, <Buffer 64 65 66> ]
   console.log(Buffer.concat(list));// <Buffer 61 62 63 64 65 66>
   ```
