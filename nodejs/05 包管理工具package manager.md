前端常用的包管理工具
* npm
* yarn
* cnpm
# npm

全称 node package manager，node 的包管理工具。
npm 是 node. js **官方内置**的包管理工具，是必须要掌握住的工具。

在安装 nodejs 时会自动安装 npm，通过 `npm -v` 查看 npm 版本

## npm 的基本使用

### 初始化

新建一个空文件夹，在命令行中打开，`npm init`
该命令的作用是创建一个 package 的配置文件 `package.json`
package. json 内容示例：
```json
{
"name": "1-npm", #包的名字
"version": "1.0.0", #包的版本
"description": "", #包的描述
"main": "index.js", #包的入口文件
"scripts": { #脚本配置
"test": "echo \"Error: no test specified\" && exit 1"
},
"author": "", #作者
"license": "ISC" #开源证书
}

```
注意事项：
1. 包名默认是文件夹的名字，包名不能大写和中文
2. package. json 可以手动创建与修改
3. 使用 `npm init -y(es)` 可以快速创建 package. json

### 搜索自己需要的包
网站搜索网址是 https://www.npmjs.com/

### 下载安装包

`npm i <包名>`
`npm insatall <包名>`

**npm i 的作用：**
当你运行 npm i 命令时，npm 会根据项目根目录中的 package. json 文件中的依赖项列表，安装所需的所有依赖包。这些依赖包通常是项目的各种 JavaScript 模块、库和工具，它们被存储在项目的 node_modules 目录中。
![[Pasted image 20230922164139.png|300]]

**运行之后文件夹下会增加两个资源**
node_modules 文件夹存放下载的包
package-lock. json 包的锁文件，用来锁定包的版本

### 导入包

使用 require 导入包


# 命令使用

`npm start `是项目中常用的一个命令，一般用来启动项目
