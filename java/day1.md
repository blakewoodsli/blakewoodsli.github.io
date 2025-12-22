# day 1 笔记

java

## 包（Package）和导包（Imports）

**package org.lee.springbootlogindemo.controller;**
作用：Java 要求把类放在包（Package）里，好比文件系统里的文件夹。

为什么：便于管理、避免不同开发者写的类名冲突。

命名规范：一般用公司域名反写（org.lee），再加项目名和功能模块名 -->
i**mport org.springframework.web.bind.annotation.CrossOrigin;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestParam;**
**import org.springframework.web.bind.annotation.RestController;**

`@RestController`：告诉 Spring “这个类是个控制器，会返回数据给前端（不返回视图页）”

`@PostMapping`：把方法和 HTTP 的 POST 请求绑定，比如用户发送表单登录时用 POST

`@RequestParam`：从前端请求里抽取单个参数（表单或 URL 参数）

`@CrossOrigin`：允许“跨域”访问（前端网页和后端接口不在同一地址时，会被浏览器拦截，需要标明允许）

**public class LoginController**

---

---

---

---

---

## 类定义和成员变量

**@RestController**
**public class LoginController {**
@RestController：等价于 @Controller + @ResponseBody。

@Controller：这个类里有方法能响应 HTTP 请求

@ResponseBody：方法返回值直接写入 HTTP 响应体，不去找模板
定义一个公开的 Java 类，名字叫 LoginController。

**private List `<User>` userList;**
成员变量：它属于类 LoginController，不是某个方法的局部变量。

用途：用来存放所有注册过的用户信息。

为什么用 List 而不是数组：List 可以随时添加、删除，数组长度一旦定死就不能变了。

---

## 构造函数：初始化用户列表

 **public LoginController(){
        super();
        userList = new ArrayList `<User>`();
    }**
    构造函数：当 Spring 创建 LoginController 实例对象时，会调用这里。

**super()**：调用父类（Object）的构造方法，所有类默认都是 Object 的子类，这一句其实可以省略。

**new ArrayList `<User>`()**：真正“开辟”一块内存，准备一个空列表给 userList。

---

## 日志记录器

**public static Log logger = LogFactory.getLog(LoginController.class);**
静态成员：static 表示这个 logger 所有对象共享，只加载一次。

LogFactory.getLog(...)：告诉日志系统“我要给 LoginController 里的日志打个标签”，以后打印出来的日志会注明是哪个类打的。

---

## 登录接口（login 方法

 **@CrossOrigin(origins = "*")
    @PostMapping(value = "/login")
    public String login(
        @RequestParam("phonenum") String phoneNum,
        @RequestParam("passwd") String passWd) {@CrossOrigin(origins = "")：**

**@CrossOrigin(origins = "")**：允许任何地址的网页来调用这个方法，解除浏览器的同源策略限制。

**@PostMapping("/login")**：当 HTTP POST 请求的路径是 /login 时，就会进到这个方法。

**public String login(...)**，返回值是 String，也就是说响应体里会直接写一个字符串（比如“登录失败”或“张三欢迎登陆”）。

**@RequestParam("phonenum")**：把前端表单里 phonenum=xxx 的值，绑定到方法参数 phoneNum 上。

 ***for (User user : userList) {
            if (user.getPassWd().equals(passWd)
             && user.getPhoneNum().equals(phoneNum)) {
                logger.info("loginpost 方法访问");
                return user.getUserName() + "欢迎登陆";
            }
        }
        return "登录失败";
    }**
**for (User user : userList)**：这是一个增强型的for循环，用于遍历userList中的每一个User对象。userList是之前定义的模拟数据库，保存着注册过的用户信息。在每次循环中，user代表userList中的一个具体用户。
**if(user.getPassWd().equals(passWd)&&user.getPhoneNum().equals(phoneNum))：**这是一个条件判断语句。它检查当前遍历到的user对象的密码（通过getPassWd方法获取）是否与前端传来的passWd参数相等，并且手机号（通过getPhoneNum方法获取）是否与前端传来的phoneNum参数相等。如果这两个条件都满足，说明找到了匹配的用户。
****logger.info("loginpost 方法访问")：****当找到匹配用户时，使用之前定义的日志对象logger记录一条日志信息，表明loginpost方法被访问。这有助于调试和监控，了解登录请求的处理情况。

---

## 注册接口（register 方法）

 ***@PostMapping(value = "/register")
    @CrossOrigin(origins = "*")
    public String register(
        @RequestParam("phonenum") String phoneNum,
        @RequestParam("passwd") String passWd,
        @RequestParam("username") String userName) {**

 @PostMapping("/register")：映射注册请求，路径是 /register。

三个参数：手机号、密码、用户名，分别从前端表单里取。

**User user = new User();**

新建一个用户对象（User 类需要你自己在项目里定义，包含 phoneNum、passWd、userName 这三个属性和它们的 getter/setter）。

 ***user.setPassWd(passWd);
user.setUserName(userName);
user.setPhoneNum(phoneNum);**
 把表单数据写入用户对象。

 **userList.add(user);**
 把用户对象加入列表，相当于“存入数据库”，但这里只是内存里。

---

## Getter 和 Setter 方法

  **public String getUserName() {
        return userName;
    }**
**public：**任何地方都能调用它。

返回类型 String：调用后会得到一个字符串（即 userName）。

方法名 getUserName：符合 JavaBean 规范 —— get + 属性名（首字母大写）。

**return userName;**：把私有字段 userName 的当前值返回给调用者。

 **public void setUserName(String userName) {
        this.userName = userName;
    }**
    返回类型 void：不返回任何值，只“设置”字段。

参数列表 (String userName)：外部传进来一个新用户名。

**this.userName = userName;：**

左侧的 this.userName 指的是“当前对象”里的 userName 字段。

右侧的 userName 是方法参数。

把参数的值赋给字段。

**为什么用 this？**
当方法参数名和成员变量名相同时，必须用 this 明确指向“字段”而不是参数。

---

## 拓展问题

**✅ 1. HTTP 协议是什么？**
HTTP 全称： HyperText Transfer Protocol（超文本传输协议）

作用：
HTTP 是浏览器和服务器之间通信的规则。你打开网页、提交表单、加载图片……其实背后都在用 HTTP 来传数据。

打个比方：
就像打电话要有一套对话规则，HTTP 就是浏览器和服务器“说话”的语言和规范。

特点：

无状态（每次请求之间不记得上次你是谁）

基于文本（所有请求和响应都是文本格式）

客户端-服务器架构（浏览器是客户端，后端是服务器）

**✅ 2. 什么是 HTTP 的表单格式？为什么要使用 JSON 格式？它们的区别和使用场景**？
📌 表单格式（application/x-www-form-urlencoded）：
传统的表单提交格式

类似这样：username=abc&password=123

后端用 @RequestParam 接收

常见于普通网页 `<form>` 表单提交

📌 JSON 格式（application/json）：
是一种数据格式，长这样：

json
复制
编辑
{
  "username": "abc",
  "password": "123"
}
后端用 @RequestBody 接收

通常用于前后端分离，配合 JavaScript（比如 fetch、axios）

✅ 区别：
对比项	表单格式（x-www-form-urlencoded）	JSON 格式
结构	键值对字符串	结构化的 JSON 对象
提交方式	传统 HTML 表单	JS 异步提交 (AJAX)
接收方式	@RequestParam	@RequestBody
使用场景	早期网页表单	现代前后端分离应用

**✅ 3. HTTP 请求的 GET 和 POST 的区别与使用场景**？
对比项	GET 请求	POST 请求
传参方式	参数写在 URL 里，如：?id=1	参数放在请求体里，用户看不到
数据大小	小（受 URL 长度限制）	大（可以传 JSON、大文件等）
安全性	差（参数暴露）	相对更安全
幂等性	是（多次调用结果相同）	否（多次调用可能产生副作用）
用途	查询数据	新增、修改、删除数据

记忆口诀：

查是 GET，改是 POST。

**✅ 4. HTTPS 和 HTTP 的区别？**
对比项	HTTP	HTTPS
是否加密	否（明文传输）	是（使用 SSL/TLS 加密）
安全性	不安全	安全（防止被窃听或篡改）
端口	80	443
使用场景	测试、本地	真实项目、传敏感数据的场合

HTTPS 适合什么？

登录页面

支付功能

任何需要保护用户信息的接口

**✅ 5. 什么是 RESTful 定义**？
REST 全称： Representational State Transfer（表现层状态转化）

简单理解： 一种写接口的规范，用来设计“清晰、简洁、有语义”的 Web 接口。

举个例子：
假设你要操作“用户”资源：

操作	请求方法	URL
获取所有用户	GET	/users
获取一个用户	GET	/users/1
添加用户	POST	/users
修改用户	PUT	/users/1
删除用户	DELETE	/users/1

这样风格的接口叫做 RESTful 接口。

**✅ 6. Java 方法的入参和出参如何定义**？
📌 入参（参数）：
方法定义中圆括号 () 里的内容，就是参数。

java
复制
编辑
public void sayHello(String name) {
    System.out.println("Hello " + name);
}
这里的 String name 是入参，类型是 String，名字是 name。

📌 出参（返回值）：
方法名前的类型就是返回值类型。比如：

java
复制
编辑
public int add(int a, int b) {
    return a + b;
}
返回值类型是 int。方法执行完，会返回一个整数。

**✅ 7. Java 如何定义对象变量？**
在 Java 中，对象是类的实例。你可以用 new 来创建。

java
复制
编辑
User user = new User(); // 创建一个对象
user.setUsername("张三"); // 设置对象的属性
这里：

User 是一个类（你自己定义的，比如有 username、password 属性）

user 是变量名

new User() 是在堆内存中分配一个 User 对象

✅ 8. Java 的 get 和 set 有什么用？
get： 获取对象里的某个属性值
set： 设置对象里的某个属性值

比如类：

java
复制
编辑
public class User {
    private String username;

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }
}
你可以这样用：

java
复制
编辑
User u = new User();
u.setUsername("张三");      // 设置
System.out.println(u.getUsername()); // 获取
好处：

保证类的封装性

可以在 get/set 方法中加一些逻辑（比如验证、转换）

---

---
