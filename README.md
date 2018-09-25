### NodeJS的基础知识
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

### Global对象中的部分属性
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
