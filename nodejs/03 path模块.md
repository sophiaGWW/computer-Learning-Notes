# path. resolve

拼接规范的绝对路径

在写入文件时，path 变量有时需要用到绝对路径，这个时候拼接的路径长这样

```js
const fs = require('fs');

//写入文件
fs.writeFileSync(__dirname+'/index.html','love');

console.log(__dirname+'/index.html');

C:\Users\sophia\Desktop\计算机\前端\nodejs\尚硅谷\code\06_express/index.html

console.log(path.resolve(__dirname,'index.html'));

```

path.resolve 之后正常