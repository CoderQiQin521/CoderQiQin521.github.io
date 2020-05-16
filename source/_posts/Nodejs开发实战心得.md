---
title: Nodejs开发实战心得
date: 2020-05-16 18:14:23
tags: ['node', 'rpc']
categories: server
---

# 理解 nodejs

nodejs 不是框架也不是语言,它是基于`chrome`V8 引擎开发的 javascript 运行时环境,可以编写 js 获得系统能力运行于服务端.
nodejs 的特点是 无阻塞 io 和事件驱动,优点是高性能高并发,扩展性强,适合 web 开发(网络请求,数据库读写,资源加载...都属于 io,借助 v8 引擎 nodejs 也是动态语言里最快的语言)

# 模块化与原生模块

服务端代码抽象度比较高,一些通用的功能很适合提取成模块复用,极大地提高开发效率和质量,由于早期 js 没有模块化能力,所以社区推出了 cmd,amd,commonjs 等模块化规范,nodejs 采用了 [commonjs](http://www.commonjs.org/) 规范.
commonjs 的模块加载是动态引用,require,module,export 等对象

npm 是全球最大的前后端模块仓库,有大量的模块可使用.在 nodejs 推出后, 很快在服务端占据了空间,,同时也前端带来了 webpack,组件化开发...这些很好的生态, 前端发到了质变,从刀耕火种般落后进入到工程和自动化发展的黑马

系统能力
[文档](http://nodejs.cn/api/)
有些强大而常用的模块最好能掌握 fs, http,path,buffer,process,url,util

# rpc 通信

大型应用和企业里服务端经过许多年开发迭代,已经发展成枝繁叶茂的系统了, 很难轻易切换语言重构,不过 nodejs 优点也很突出,天生支持高并发 io 很适合做 web 的 bff 层, 开发和沟通成本小,具有同构能力,一些新的,小的服务就可以用 node 方案解决.
rpc 与 http 区别

http 是应用层,底层也是 tcp 封装的,建立连接需要三次握手,四次挥手,传输的数据是文本格式,性能比 rpc 慢很多
rpc 属于网络层 tcp 协议,也叫远程过程调用,传送的是二进制格式,使用 nodejs 的 net 模块很容易实现 rpc.
server 端

```
const net = require('net')

```

client 端

# 服务端渲染

# 实战

# 性能优化
