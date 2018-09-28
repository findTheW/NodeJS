``` javascript
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
        fileData.forEach(function (f) {
            f.path = projectData.name + '/' + f.name;
            f.content = f.content || ""
            switch (f.type) {
                case  'dir':
                    fs.mkdir(f.path);
                    break;
                case 'file':
                    fs.writeFileSync(f.path, f.content);
                    break;
                default : break;
            }
        })
    }
}
```
