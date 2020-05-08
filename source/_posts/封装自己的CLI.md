---
title: 封装自己的CLI
date: 2020-05-05 00:18:53
tags: ['cli']
categories: web
---

## 前置知识

1. nodejs 里`path`,`fs`模块
2. commander 包(用来自定义命令和参数)
3. inquirer 包(命令行中交互式问答收集)
4. del 包
5.

## 项目搭建

1. 使用`npm init -y`初始化一个项目,创建 bin 目录用来存放启动文件,创建 src 目录编写 cli 的主要代码
2. 添加 bin 目录和`index.js`,这是一个 nodejs 下可执行的目录,全局模块调用入口一般是这里
3. 在 package.json 文件添加`bin`字段,key 是全局执行的命令,值指向 bin 目录的执行文件

## 代码实现

### 模块划分批量引入

### 创建

### 删除

## 总结以及其他实践

[源码地址]()
