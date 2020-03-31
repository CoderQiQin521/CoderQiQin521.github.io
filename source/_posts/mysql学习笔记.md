---
title: mysql学习笔记
date: 2020-03-28 08:59:44
tags: ['数据库', 'mysql', '关系型']
categories: database
---

## mysql介绍

## 安装

## 语句类型 
- DML:  数据操纵语句   用于添加、删除、更新和查询数据库记录，并检查数据完整性，常用的语句关键字主要包括 insert、delete、udpate 和select 等。(增添改查）
-DDL: 数据定义语句   数据段、数据库、表、列、索引等数据库对象的定义。常用的语句关键字主要包括 create、drop、alter等。
-DCL（Data Control Language）语句:  数据控制语句，用于控制不同数据段直接的许可和访问级别的语句。这些语句定义了数据库、表、字段、用户的访问权限和安全级别。主要的语句关键字包括 grant、revoke 等。
-TCL: 事务控制语言

## 常用命令
```
mysql --version/-V 查看版本
mysql -h localhost -P 3306 -u root -p 链接本地数据库3306端口 roo用户
show databases; 查看所有数据库
use xxx; 使用某个库
show tables; 查看当前库所有的数据表
show tables from xxx;   查看某个库下的所有数据表;
create table xxx(id int, title varchar(20));  创建一张数据表
insert into xxx (id,title) values(1,xx); 插入一行数据;
select * from xxx; 查询当前表的所有行数据;
select database(); 查看当前所在的库;
select version();  查看当前mysql版本
```