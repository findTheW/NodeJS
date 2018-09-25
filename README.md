## NodeJS
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
