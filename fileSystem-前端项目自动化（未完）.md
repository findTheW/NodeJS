``` javascript
//构造一个对象，对象name为主文件夹名字，然后在里面创建子文件夹/文件的数组
var projectData = {
    'name' : 'maiov',
    'fileData' : [
        {
            'name' : 'css',
            'type' : 'dir'
        },
        {
            'name' : 'js',
            'type' : 'dir'
        },
        {
            'name' : 'images',
            'type' : 'dir'
        },
        {
            'name' : 'index.html',
            'type' : 'file',
            'content' : '<html>\n\t<head>\n\t\t<title>title</title>\n\t</head>\n\t\t<body>\n asdasd</body>\n\t</html>'
        }
    ]
};

var fs = require('fs');
if( projectData.name ){
    //创建文件夹
    fs.mkdirSync(projectData.name);
    var fileData = projectData.fileData;
    //判断数组是否存在
    if( fileData && fileData.forEach ){
        //遍历数组
        fileData.forEach(function (f) {
            //保留存储路径
            f.path = projectData.name + '/' + f.name;
            //如果子文件有内容，保存，没有则为空字符串
            f.content = f.content || "";
            //判断文件类型
            switch (f.type) {
                case 'dir':
                //创建文件夹
                    fs.mkdir(f.path);
                    break;
                case 'file':
                //创建文件
                    fs.writeFileSync(f.path, f.content);
                    break;
                default : break;
            }
        })
    }
}
```
