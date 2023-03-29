---
title: MyBatis学习记录
date: 2023-03-29 13:42:24
tags:
    - [MyBatis]
---

<!-- more -->

# MyBatis

## 什么是MyBatis？

* MyBatis是一款优秀的持久层框架，用于简化JDBC开发
* MyBatis本是Apache的一个开源项目iBatis，2010年这个项目由apache software foundation迁移到了google code，并改名为MyBatis。2013年11月迁移至Github
* [官网](https://mybatis.org/mybatis-3/zh/index.html)

## 持久层

* 负责将数据保存到数据库的那一层代码
* JavaEE三层架构：表现层、业务层、持久层

## 框架

* 框架就是一个半成品软件，是一套可重用的、通用的、软件基础代码模型
* 在框架的基础上构建软件编写更加高效、规范、通用、可扩展

## JDBC缺点

```java
// 1. 注册驱动
Class.forName("com.mysql.jdbc.Driver");
// 2. 获取Connection连接
String url = "jdbc:mysql:///db1?useSSL = false";
String uname = "root";
String pwd = "1234";
Connection conn = DriverManager.getConnection(url, uname, pwd);
// 接收输入的查询条件
String gender = "男";
// 定义sql
String sql = "select * from tb_user where gender = ?";
// 获取pstmt对象
PreparedStatement pstmt = conn.prepareStatement(sql);
// 设置 ？的值
pstmt.setString(1, gender);
// 执行sql
ResultSet rs = pstmt.executeQuery();
// 遍历Result，获取数据
User user = null;
ArrayList<User> users = new ArrayList<>();
while (rs.next()){
    // 获取数据
    int id = rs.getlnt("id");
    String username = rs.getString("username");
    String password = rs.getString("password");
    // 创建对象，设置属性值
    user = new User();
    user.setId(id);
    user.setUsername(username);
    user.setPassword(password);
    user.setGender(gender);
    // 装入集合
        users.add(user);
}
```

### 硬编码（将未来可能变化的字符串信息写到代码里面）

* 注册驱动，获取链接
* SQL语句

### 操作繁琐

* 手动设置参数
* 手动封装结果集

## MyBatis简化

1. 硬编码写到配置文件中
2. 繁琐的操作自动完成

MyBatis免除了几乎所有的JDBC代码以及设置参数和获取结果集的工作

# 目录

* MyBatis快速入门
* Mapper代理开发
* MyBatis核心配置文件
* 配置文件完成增删改查
* 注解完成增删改查
* 动态SQL

# MyBatis快速入门

## 查询user表中所有数据

1. 创建user表，添加数据
2. 创建模块，导入坐标
3. 编写MyBatis核心配置文件 --> 替换连接信息 解决硬编码问题
4. 编写SQL映射文件 --> 统一管理sql语句，解决硬编码问题
5. 编码

### 创建user表，添加数据

打开NaviCat，创建一个如下所示的表

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230329154716.png)

并填入测试信息

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230329154836.png)

在idea中新建Maven模块mybatis-demo