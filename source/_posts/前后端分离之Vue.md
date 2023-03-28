---
title: 前后端分离之Vue
date: 2023-03-28 13:22:46
tags:
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
