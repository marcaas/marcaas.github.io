---
title: 从0开始搭建一套SpringBoot+Vue后台管理系统
date: 2023-03-02 11:15:28
tags:
    - [SpringBoot]
    - [Vue]
    - [后端]
description: 初次尝试使用Springboot + VUE框架搭建后台管理网站
---
![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/wallhaven-j3m8y5.png)

<!-- more -->

# 环境配置
* jdk 1.8
* mysql 5.7+
* node 16.19.0
* navicat 
* IDEA2021.3
# 使用Vue进行前端框架搭建
## 创建Vue项目
```
vue creat (vuename)
```
具体选项如下

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230303142235.png)

然后可以将服务运行起来看一下是否成功，cd到(vuename)文件夹，使用命令
```
npm run serve
```
![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230303143942.png)

给创建好的页面添加一些元素

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230306084740.png)

浏览器界面长这样

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230303144110.png)

## 安装Element插件
这里推荐使用npm安装
```
npm i element-ui -S
```
此时可以在package.json文件里看到有element-ui的依赖

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230306084215.png)

在Home.vue文件的div块中添加一个button来测试element-ui是否可用
```html
<el-button>{{ msg }}</el-button>
```
如图，标签出现

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230306085001.png)

接下来参考[element官网](https://element.eleme.cn/#/zh-CN/component/container#container-bu-ju-rong-qi)的内容对页面布局进行自定义的配置，在对页面细节进行修改后得到一个页面的雏形，如下图所示

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230306095241.png)

以下是我的相关配置文件
global.css

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230306095834.png)

App.vue

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230306095926.png)

Home.vue（主要修改处已用红色框标出）

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230306100214.png)

具体网页美化请参照[Element组件官网](https://element.eleme.cn/#/zh-CN/component/installation)并结合自己审美进行修改。

# 使用Springboot进行后端搭建
## 项目依赖
* Lombok
* Spring Web
* MySQL Driver
* MyBatis Framework

此处默认已安装MySQL和Navicat

## 项目结构

* src
* pom.xml

使用idea的springboot框架自动创建项目后，得到如下项目结构

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230328103910.png)

其中src是主程序文件夹，pom.xml是依赖的配置文件，包含了依赖的版本等信息。

## 配置数据库

数据库配置文件在/src/main/resources目录下的application.yml文件，需要先对它进行配置

我的测试用数据库配置如下，其中数据库连接地址中test字段为用户数据库的名字，需根据实际情况修改，serverTimezone也应根据所在地自行更改时区（GMT%2b8为东八区）

```yml
## 应用名称
spring:
#  application:
#    name: Springboot
# 数据库驱动：
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
# 数据源名称
    name: defaultDataSource
# 数据库连接地址
    url: jdbc:mysql://localhost:3306/test?serverTimezone=GMT%2b8
# 数据库用户名&密码：
    username: ****
    password: ******
# 应用服务 WEB 访问端口
server:
  port: 9090
mybatis:
  mapper-locations: classpath:mapper/*.xml #扫描所有mybatis文件
```

首先在springboot文件目录下创建entity层（也叫model层、domain层）用于存放我们的实体类，与数据库中的属性基本保持一致，实现set和get的方法（此处使用的@Data注解包含了set和get方法，故没有在下面体现）

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230309102013.png)

在entity包下创建实体类与之前创建的数据库的结构对应，如下图所示

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230309102359.png)

```java
package com.marcaas.springboot.entity;

import lombok.Data;

@Data
public class User {
    private Integer id;
    private  String username;
    private  String password;
    private  String nickname;
    private  String email;
    private  String phone;
    private  String address;

}
```

在springboot文件夹下创建mapper层（也叫dao层），对数据库进行数据持久化操作，方法语句是直接针对数据库操作的，主要实现一些增删改查操作，在mybatis中方法主要与xxx.xml内相互一一映射

在mapper层创建Interface 接口类UserMapper

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230309114057.png)

并对其进行编辑

```java
package com.marcaas.springboot.mapper;

import com.marcaas.springboot.entity.User;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Select;

import java.util.List;

@Mapper
public interface UserMapper {
    @Select("SELECT * from sys_user")
    List<User> findAll();

    @Select("INSERT into sys_user(username,password,nickname,email,phone,address) " +
            "VALUES(#{username},#{password},#{nickname},#{email},#{phone},#{address})")
    Integer insert(User user);
}

```

创建controller层（也叫web层），负责具体模块的业务流程控制，需要调用service逻辑设计层的接口来控制业务流程。因为service中的方法是我们使用到的，controller通过接受前端H5或App传来的参数进行业务操作，再将处理结果返回到前端。

在controller层下创建UserController.java文件，并编辑新增接口和查询接口

```java
package com.marcaas.springboot.controller;

import com.marcaas.springboot.entity.User;
import com.marcaas.springboot.mapper.UserMapper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/user")
public class UserController {

    @Autowired
    private UserMapper userMapper;

    // 新增接口
    @PostMapping
    public Integer save(@RequestBody User user) {
       return userMapper.insert(user);
    }

    // 查询接口
    @GetMapping
    public List<User> index() {
        return userMapper.findAll();
    }

}

```
