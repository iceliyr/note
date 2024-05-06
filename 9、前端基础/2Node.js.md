## Node.js基础

官网：https://nodejs.org/en/learn/getting-started/introduction-to-nodejs

笔记：https://www.runoob.com/nodejs

### 简介

Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行时环境，可以让你使用 JavaScript 来开发服务器端的应用程序。它使用事件驱动、非阻塞 I/O 模型，使其轻量高效，非常适合构建数据密集型的实时应用。

Node.js 主要用途包括但不限于：

- 构建 Web 服务器和 API 后端
- 实时应用程序开发，如聊天应用、实时通讯应用等
- 数据流处理，如日志处理、文件操作等
- 开发工具和构建工具，如构建工具链、前端构建工具等

## Node.js基础应用

### 安装运行



#### 安装node.js版本管理

[node.js -用nvm管理nodejs版本切换](https://segmentfault.com/a/1190000044661290)





### 变量

##### var

var 是在 ES5 中引入的关键字，用于声明变量。

使用 `var` 声明的变量是函数作用域（function-scoped），而不是块级作用域。

在同一作用域内，可以重复声明同名变量，而且变量声明会被提升（hoisting）到作用域的顶部。

```
javascriptCopy Codevar x = 10;
console.log(x); // 输出 10

{
   var y = 20;
}
console.log(y); // 输出 20
```

##### let

let 是在 ES6 中引入的关键字，也用于声明变量。

使用 let 声明的变量是块级作用域（block-scoped），只在当前代码块内有效。

不允许重复声明同名变量，而且不存在变量提升。

```
javascriptCopy Codelet a = 30;
console.log(a); // 输出 30

{
   let b = 40;
   console.log(b); // 输出 40
}
// console.log(b); // 这里会报错，因为 b 在当前作用域外不可访问
```

##### const

const 也是在 ES6 中引入的关键字，用于声明常量，其值在声明后不可变。

使用 const 声明的变量也是块级作用域的。

声明时必须进行初始化，并且不允许重新赋值。

```
javascriptCopy Codeconst PI = 3.14;
console.log(PI); // 输出 3.14

// PI = 3.14159; // 这里会报错，因为常量不能被重新赋值
```

### Buffer





## Node.js模块使用

### 简介

##### **核心模块（Core Modules）**：

**fs**：文件系统操作模块，用于读写文件、创建目录等

**http** / **https**：用于创建 HTTP 或 HTTPS 服务器和客户端

**path**：处理文件路径相关操作的模块

**os**：提供了操作系统相关的信息和方法

**events**：事件处理模块，用于处理事件的发布与订阅

**util**：常用工具函数的集合

**crypto**：加密模块，提供了加密、解密和哈希等功能

**stream**：流模块，用于处理大规模数据的流式传输

**child_process**：用于创建子进程的模块，可执行系统命令

**net**：网络通信模块，用于创建 TCP 服务器和客户端

##### **常用第三方模块**：

**Express**：Web 应用框架，简化了构建 Web 服务器的流程

**Mongoose**：MongoDB 的 ODM（对象文档映射）库，用于在 Node.js 中操作 MongoDB 数据库

**[Socket.IO](http://socket.io/)**：实时应用程序开发的库，用于 WebSocket 和 HTTP 通信

**Request** / **Axios**：用于发送 HTTP 请求的库，支持 Promise 和流式请求

**lodash**：实用的 JavaScript 实用程序库，提供了许多常用功能的封装

**Joi**：用于验证数据的库，特别适用于处理输入的验证和数据转换

**Moment.js**：用于解析、验证、操作和显示日期和时间的库

**jsonwebtoken**：用于生成和验证 JSON Web Tokens（JWT）的库

**Multer**：用于处理 multipart/form-data 格式数据（文件上传）的中间件

### 文件模块

### HTTP模块

参数方法：[Node.js http 模块 (nodejs.cn)](https://dev.nodejs.cn/learn/the-nodejs-http-module/)

基础步骤：[NodeJS 之 HTTP 模块](https://blog.csdn.net/qq_44879989/article/details/128746940)

## Node.js高级使用

### 模块化

### 包管理工具-npm

#### npm配置

#####  配置npm的全局安装路径

cmd（管理员）：npm config set prefix "G:NodeJS-npm"

##### 切换npm的镜像

淘宝镜像：cmd（管理员）：npm config set registry [https://registry.npm.taobao.org](https://registry.npm.taobao.org/)

官方镜像：cmd（管理员）：npm config set registry https://registry.npmjs.org/

#### npm使用

##### 初始化包（默认为包名）

```
npm init
```

##### 包的配置文件

package.json

package-lock.json：锁定包的版本

##### 下载安装包

```
npm i xxx
```

##### require导入npm包

const xxx=require('xxx')

##### 安装Vue-cli

npm install -g @vue/cli
