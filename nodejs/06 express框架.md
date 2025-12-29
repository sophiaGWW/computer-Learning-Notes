# 介绍
express 框架是 nodejs 平台上的一个包，便于开发 web 功能（HTTP 服务）

# express 初体验
```js
//1.导入express
const express = require('express');
//2.创建应用对象
const app = express();
//3.创建路由规则
app.get('/home',(req,res) =>{
    res.end('express first test')
})
//4.监听端口，启动服务
app.listen(9000,() =>{
  console.log('9000服务已经启动...')
})
```

# express 练习
输入不同的 id 网页，获得不同的歌手
```js
const express = require('express');
const {singers} = require('./singers.json');
const app = express();

app.get('/singer/:id.html',(req,res) =>{
    const {id} = req.params;
    let result = singers.find(item => {
      if(item.id == Number(id)){
        return true;
      }
    })
    if(!result){
      res.statusCode=404;
      res.end('<h1>404 not found</h1>');
      return;
    }
    res.setHeader('content-type','text/html;charset=utf-8');  
    res.end(`<h1>${result.singer_name}</h1>
    <img src='${result.singer_pic}'>`)
})

app.listen(9000,() =>{
  console.log('9000服务已经启动...')
})
```

# 全局中间件

对所有的请求进行拦截，然后进行操作

利用全局中间件，对访问的 url 和 ip 记录到 log 里
```js
const express = require('express');
const fs = require('fs');
const path = require('path');
const app = express();
//记录访问的url，以及ip地址
function logRecord(req,res,next){
  let {url, ip} = req;
  fs.appendFileSync(path.resolve(__dirname,'./access.log'),`${url} ${ip}\r\n`);
  next();
}

app.use(logRecord);

app.get('/home',(req,res) =>{
    res.send('前台首页');
})

app.get('/admin',(req,res) =>{
  res.send('后台首页');
})

app.all('*',(req,res) =>{
  res.send('<h1>404 not found</h1>');
})

app.listen(9000,() =>{
  console.log('9000服务已经启动...')
})
```

# 路由中间件

只需要对某些路径进行封装。
格式：
```js
//声明函数
function A（）{

}

app.get('/路径',A,(request,response)=>{

})
```

# 静态资源中间件

设置静态资源中间件代码
```js
//静态资源中间件的设置，将当前文件夹下的public目录作为网站的根目录
app.use(express.static('./public')); //当然这个目录中都是一些静态资源
```

# 响应文件内容

express 中 res 响应 html 内容的方法
```js
app.get('/login', (req, res) => {
  // res.send('表单页面')
  //响应 HTML 文件内容
  res.sendFile(__dirname + '/11_form.html');
});
```

# 获取 post 请求数据体

body-parser 是 npm 中的一个包。

使用步骤：
第一步：安装
```shell
npm i body-parser
```
第二步：导入 body-parser 包
```js
const bodyParser = require('body-parser');
```
第三步：获取中间件函数
```js
//处理 querystring 格式的请求体
let urlParser = bodyParser.urlencoded({extended:false}));
//处理 JSON 格式的请求体
let jsonParser = bodyParser.json();

```
第四步：设置路由中间件，然后使用` request. body `来获取请求体数据
```js
app.post('/login', urlParser, (request,response)=>{
//获取请求体数据
//console.log(request.body);
//用户名
console.log(request.body.username);
//密码
console.log(request.body.userpass);
response.send('获取请求体数据');
});

```

获取到的请求体数据：

```shell
[Object: null prototype] { username: 'admin', userpass: '123456' }

```

# Router

是 express 的一个完整的中间件和路由系统，可以看做是一个小型的 app 对象。

为了把路由更好的分文件管理，即模块化。

## Router 的使用

创建独立的 homeRouter. js 文件
```js
//1. 导入 express
const express = require('express');
//2. 创建路由器对象
const router = express.Router();
//3. 在 router 对象身上添加路由
router.get('/', (req, res) => {
res.send('首页');
})
router.get('/cart', (req, res) => {
res.send('购物车');
});
//4. 暴露
module.exports = router;

```

主文件
```js
const express = require('express');
const app = express();
//5.引入子路由文件
const homeRouter = require('./routes/homeRouter');
//6.设置和使用中间件
app.use(homeRouter);
app.listen(3000,()=>{
console.log('3000 端口启动....');
})
```

# ejs

将 js 和 html 分离的一种模块

下载 ejs
```shell
npm i ejs 
```

html
```html
<h2>我爱你 <%= china %></h2>
```

js
```js
const ejs = require('ejs');
const fs = require('fs');

let china = '中国';

let str = fs.readFileSync('./01_html.html').toString();

let result = ejs.render(str,{china:china});

console.log(result);
```

输出
```shell
<h2>我爱你 中国</h2>
```

# express-generator

通过应用生成器工具 express-generator 可以快速创建一个应用的骨架。
中文版网页： https://www.expressjs.com.cn/starter/generator.html

对于较老的 Node 版本，请通过 npm 将 Express 应用程序生成器安装到全局环境中并使用：
```shell
npm install -g express-generator
```

1.创建 express 骨架
```shell
//添加对esj模版引擎的支持 创建15-gen..为名的文件夹
express -e 文件夹名
```

2.该文件夹右键-在集成终端中打开
3.执行 `npm i` 命令，下载所有依赖包
4.`npm start` 运行项目