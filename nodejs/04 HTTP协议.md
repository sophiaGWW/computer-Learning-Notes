# 使用 nodejs 创建 http 服务

```js
//1. 导入http模块
const http = require('http');
//2. 创建server服务
const server = http.createServer((request,response) => {
//解决页面乱码
  reponse.setHeader('content-type','text/html;charset=utf-8');
  reponse.end('你好');
})
//3. 监听端口，启动服务
server.listen(9000,() => {
  console.log('服务已经启动');
});
```

**如果端口被其它程序占用：**
1. 使用 `资源监视器` 通过端口找到该程序的 PID
2. 使用 `任务管理器` 关闭对应程序

# http response
**设置响应体有两个方法**
* response. write ('xx')
* response. end ('xx')

write 方法可以调用多次
end 方法
	每次处理必须执行
	只能调用一次

# reponse 解决页面乱码问题
```js
 reponse.setHeader('content-type','text/html;charset=utf-8');
```
# 获取请求 url 的路径该如何写

```js
let {pathname} = new URL(request.url,'http://127.0.0.1');
//如果http://localhost:9000/test
//返回的pathname是'/test'
if(pathname === '/'){
	进行操作
}
```

# 若请求错误，返回给页面的 code 写法

```js
//设置返回的错误码为404
 reponse.statusCode=404;
 //返回给页面
 reponse.end('<h1>404 not found</h1>');
```

# 搭建静态资源服务

```js
/**
 * 创建一个 HTTP 服务，端口为 9000，满足如下需求
 * GET  /index.html        响应  page/index.html 的文件内容
 * GET  /css/app.css       响应  page/css/app.css 的文件内容
 * GET  /images/logo.png   响应  page/images/logo.png 的文件内容
 */

//导入http模块
const http = require('http');
const fs = require('fs');

//创建服务对象
const server = http.createServer((request,response) => {
//获取请求url的路径
let {pathname} = new URL(request.url,'http://127.0.0.1');
//拼接路径名
let filename = __dirname + '/page' + pathname;
fs.readFile(filename,(err,data) =>{
  if(err){
    response.statusCode=404;
    response.end('<h1>404 not found</h1>');
    return;
  }
  response.end(data);
})
})

server.listen(9000,() => {
  console.log('开始运行');
})

```

# 网页 URL 之绝对路径
![[Pasted image 20230907110209.png]]
![[Pasted image 20230907140746.png]]
