## NodeJS
1. 以 EcmaScript 为基础，扩展为操作os（操作系统）、file（文件系统）、net（网络）、database（数据库）等...
2. node中的顶层对象，global对象。
3. 一个文件就是一个模块，每个模块都有自己的作用域
4. 在文件中声明的变量并不是全局的，而只是模块中的局部变量
> var a = 100; //局部变量
> global.a = 200; //全局变量
> console.log(a);
> console.log(global.a);
