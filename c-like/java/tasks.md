# java 开发计划

## 核心目标

- 掌握 Tomcat 部署原理
- 熟练 MyBatis 操作 MySQL
- 用 Spring Boot 搭建 RESTful API
- 实现 Spring Cloud 基础微服务
- Vue 整合实现注册登录+商品管理

## Day 1: 环境搭建+Tomcat 核心

### 检查环境并安装

是否安装 JDK、Maven、MySQL、IDEA、VSCode，列出这些环境的版本

#### 版本

#### 配置

### 创建动态 Web 项目，手写 Servlet 处理 /hello 请求

项目名称：**web-hello**

输出：本地访问 http://localhost:8080/hello 返回 "Hello World!"

### 笔记

[第一天笔记](./day1.md)

### 参考教程：

- [JavaWeb 开发必看！Tomcat 架构及工作原理（8 分钟）\_哔哩哔哩\_bilibili](https://www.bilibili.com/video/BV1J3411k7Xc/?buvid=XYD943FD3466979469194918F4D4439774714&from_spmid=search.search-result.0.0&is_story_h5=false&mid=RmMWb8Yfn3DTANLw%2F%2BvVQg%3D%3D&plat_id=116&share_from=ugc&share_medium=android&share_plat=android&share_session_id=25918f60-faef-4351-b17f-01092144fe01&share_source=WEIXIN&share_tag=s_i&spmid=united.player-video-detail.0.0&timestamp=1751854909&unique_k=uq6hz2n&up_id=511819239&vd_source=2016b6c473c674bad90b455a087f65c0)
- [【教程】无 Bug 丝滑创建 Java Web 项目 Maven+Tomcat for servlet or jsp\_哔哩哔哩\_bilibili](https://www.bilibili.com/video/BV1ru411277z/?buvid=XYD943FD3466979469194918F4D4439774714&from_spmid=search.search-result.0.0&is_story_h5=false&mid=RmMWb8Yfn3DTANLw%2F%2BvVQg%3D%3D&plat_id=116&share_from=ugc&share_medium=android&share_plat=android&share_session_id=cdc10a92-50c7-4699-a256-fbbd61eb19de&share_source=WEIXIN&share_tag=s_i&spmid=united.player-video-detail.0.0&timestamp=1751854991&unique_k=1ADSXbX&up_id=273941776&vd_source=2016b6c473c674bad90b455a087f65c0)

## Day2: 前后端分离登录与注册

### 前端任务

1. 创建目录 web-shop
2. 注册：user-register.html
3. 登录：user-login.html

### 后端任务

使用 Spring Boot 实现 RESTful API

1. 用户管理
   - UserObject.java 存放 User 对象定义，以及注册用户存入，用 Map 保存
   - 用户信息：账号、姓名、密码、手机号、性别
2. UserLogin.java
   - RESTful URL Path: /login
   - 主要逻辑：用户登录，校验用户账号和密码
3. UserRegister.java 用户注册
   - RESTful URL Path: /register
   - 主要逻辑：
     - 用户注册，将信息注册
     - 加分：如果已经注册过，提醒重复注册

### 参考教程：
- [快速上手SpringBoot项目（登录注册保姆级教程） - MakerHu - 博客园](https://www.cnblogs.com/makerhu/p/14960440.html)
