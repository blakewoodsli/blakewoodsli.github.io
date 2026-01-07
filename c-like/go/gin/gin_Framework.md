
# Gin 学习笔记总结（
- [Gin 学习笔记总结（](#gin-学习笔记总结)
  - [初始化 Gin](#初始化-gin)
    - [创建 Gin 实例](#创建-gin-实例)
    - [启动服务](#启动服务)
- [响应（Response）](#响应response)
  - [一、Query 参数（URL ? 后面的）](#一query-参数url--后面的)
    - [1️⃣ 请求长什么样？](#1️⃣-请求长什么样)
    - [2️⃣ Gin 怎么取？](#2️⃣-gin-怎么取)
    - [3️⃣ 带默认值（非常常用）](#3️⃣-带默认值非常常用)
    - [4️⃣ 实战中你几乎一定会这样写](#4️⃣-实战中你几乎一定会这样写)
    - [📌 Query 的使用场景](#-query-的使用场景)
    - [❗常见误区](#常见误区)
  - [二、Path 参数（URL 路径里的变量）](#二path-参数url-路径里的变量)
    - [1️⃣ 路由定义](#1️⃣-路由定义)
    - [2️⃣ 请求长这样](#2️⃣-请求长这样)
    - [3️⃣ Gin 里怎么取？](#3️⃣-gin-里怎么取)
    - [📌 Path 参数的特点](#-path-参数的特点)
    - [📌 常见用途](#-常见用途)
    - [❗常见误区](#常见误区-1)
  - [三、Header（请求头）](#三header请求头)
    - [1️⃣ Header 是什么？](#1️⃣-header-是什么)
    - [2️⃣ Gin 里怎么取？](#2️⃣-gin-里怎么取)
    - [3️⃣ 最经典用途（你已经在用）](#3️⃣-最经典用途你已经在用)
    - [📌 Header 常见用途](#-header-常见用途)
    - [❗常见误区](#常见误区-2)
  - [四、Body（请求体 JSON）](#四body请求体-json)
    - [1️⃣ 请求长什么样？](#1️⃣-请求长什么样-1)
    - [2️⃣ 定义接收结构体](#2️⃣-定义接收结构体)
    - [3️⃣ 绑定 JSON](#3️⃣-绑定-json)
    - [📌 Body 适合放什么？](#-body-适合放什么)
    - [❗常见误区（非常重要）](#常见误区非常重要)
  - [🧠 四种参数对比（你要记住的表）](#-四种参数对比你要记住的表)
  - [✅ 一句话总结（你背这个）](#-一句话总结你背这个)
- [Bind 绑定器（ShouldBind）](#bind-绑定器shouldbind)
    - [一、为什么需要 Bind？（先解决“为啥要它”）](#一为什么需要-bind先解决为啥要它)
    - [你先想一个问题👇](#你先想一个问题)
  - [❌ 没有 Bind 会怎样？](#-没有-bind-会怎样)
    - [✅ Bind 的作用（一句话）](#-bind-的作用一句话)
    - [二、JSON 绑定结构体是干嘛的？（逐行拆）](#二json-绑定结构体是干嘛的逐行拆)
  - [1️⃣ `type CreateUserReq struct {}`](#1️⃣-type-createuserreq-struct-)
    - [2️⃣ `Name string`](#2️⃣-name-string)
  - [3️⃣ `json:"name"` 是干嘛的？（超级重要）](#3️⃣-jsonname-是干嘛的超级重要)
    - [如果没有这个 tag 会怎样？](#如果没有这个-tag-会怎样)
    - [4️⃣ `binding:"required"` 是干嘛的？](#4️⃣-bindingrequired-是干嘛的)
    - [举例](#举例)
    - [三、核心代码：ShouldBindJSON（重点）](#三核心代码shouldbindjson重点)
    - [1️⃣ `var req CreateUserReq`](#1️⃣-var-req-createuserreq)
    - [2️⃣ `c.ShouldBindJSON(&req)` 干了什么？](#2️⃣-cshouldbindjsonreq-干了什么)
    - [✅ 第 1 件事：读请求 Body](#-第-1-件事读请求-body)
    - [✅ 第 2 件事：JSON → 结构体（反序列化）](#-第-2-件事json--结构体反序列化)
    - [✅ 第 3 件事：字段校验（binding）](#-第-3-件事字段校验binding)
    - [3️⃣ 为什么要 `&req`？](#3️⃣-为什么要-req)
      - [四、常见 binding 规则（你一定会用）](#四常见-binding-规则你一定会用)
    - [1️⃣ 必填](#1️⃣-必填)
    - [2️⃣ 邮箱格式](#2️⃣-邮箱格式)
    - [3️⃣ 最小长度](#3️⃣-最小长度)
    - [组合使用](#组合使用)
    - [五、完整正确写法（你以后就照这个抄）](#五完整正确写法你以后就照这个抄)
    - [六、最容易踩的坑（重点记）](#六最容易踩的坑重点记)
    - [❌ 1. json tag 不一致](#-1-json-tag-不一致)
    - [❌ 2. 忘记判断 err](#-2-忘记判断-err)
    - [❌ 3. 字段小写](#-3-字段小写)
    - [七、为什么说 Bind 是“核心”？](#七为什么说-bind-是核心)
    - [八、一句话总结（背下来）](#八一句话总结背下来)
- [文件上传 \& 下载](#文件上传--下载)
    - [文件上传](#文件上传)
    - [文件下载](#文件下载)
- [路由 \& 路由组](#路由--路由组)
    - [普通路由](#普通路由)
    - [路由组（常用于权限）](#路由组常用于权限)
- [中间件（Middleware）](#中间件middleware)
    - [1️⃣ 用一句大白话说](#1️⃣-用一句大白话说)
    - [2️⃣ 请求在 Gin 里的完整流程](#2️⃣-请求在-gin-里的完整流程)
  - [二、中间件的标准结构（必须背下来）](#二中间件的标准结构必须背下来)
    - [每一行在干什么？](#每一行在干什么)
    - [举个你能“看到顺序”的例子](#举个你能看到顺序的例子)
  - [三、全局中间件（所有接口都会经过）](#三全局中间件所有接口都会经过)
    - [1️⃣ 写法](#1️⃣-写法)
    - [2️⃣ 什么时候用全局中间件？](#2️⃣-什么时候用全局中间件)
    - [3️⃣ 对应你写过的代码](#3️⃣-对应你写过的代码)
  - [四、路由分组（非常重要）](#四路由分组非常重要)
    - [1️⃣ 路由分组是干嘛的？](#1️⃣-路由分组是干嘛的)
    - [2️⃣ 路由分组基本结构](#2️⃣-路由分组基本结构)
    - [3️⃣ 给路由组加中间件](#3️⃣-给路由组加中间件)
    - [4️⃣ 完整示例（你一定要能写出来）](#4️⃣-完整示例你一定要能写出来)
    - [5️⃣ 和全局中间件的区别（很重要）](#5️⃣-和全局中间件的区别很重要)
  - [五、终止请求（Abort）](#五终止请求abort)
    - [1️⃣ Abort 是干嘛的？](#1️⃣-abort-是干嘛的)
    - [2️⃣ 标准用法（你已经写过）](#2️⃣-标准用法你已经写过)
    - [3️⃣ 如果不 Abort 会发生什么？](#3️⃣-如果不-abort-会发生什么)
    - [4️⃣ 推荐写法（一步到位）](#4️⃣-推荐写法一步到位)
  - [六、中间件里常用的 Context 操作](#六中间件里常用的-context-操作)
    - [1️⃣ 设置值（跨接口传数据）](#1️⃣-设置值跨接口传数据)
    - [2️⃣ 获取值](#2️⃣-获取值)
- [日志（Logging）详解](#日志logging详解)
    - [一、Gin 默认日志](#一gin-默认日志)
    - [二、自定义日志中间件（你能控制）](#二自定义日志中间件你能控制)
    - [三、生产环境建议（很重要）](#三生产环境建议很重要)
  - [八、一句话总结（必须记住）](#八一句话总结必须记住)
  - [🔟 Gin 项目常见分层（你已经在用）](#-gin-项目常见分层你已经在用)
  - [📌 总结一句话（非常重要）](#-总结一句话非常重要)

---

##  初始化 Gin

### 创建 Gin 实例

```go
r := gin.Default()
```

说明：

* `gin.Default()` = Logger 中间件 + Recovery 中间件
* 生产环境最常用

---

### 启动服务

```go
r.Run(":8000")
```

默认监听 `0.0.0.0:8000`

---

#  响应（Response）


## 一、Query 参数（URL ? 后面的）

### 1️⃣ 请求长什么样？

```http
GET /users?page=2&pageSize=10
```

👉 `?` 后面的，叫 **Query 参数**

---

### 2️⃣ Gin 怎么取？

```go
page := c.Query("page")
```

等价于：

```go
page := r.URL.Query().Get("page")
```

📌 **返回的是 string**

---

### 3️⃣ 带默认值（非常常用）

```go
page := c.DefaultQuery("page", "1")
```

含义是：

> 如果 URL 里没有 page
> 👉 就用 `"1"`

---

### 4️⃣ 实战中你几乎一定会这样写

```go
page, _ := strconv.Atoi(c.DefaultQuery("page", "1"))
```

---

### 📌 Query 的使用场景

* 分页
* 搜索
* 过滤条件

---

### ❗常见误区

❌ `c.Query` 取不到 JSON
❌ Query **永远是字符串**

---

## 二、Path 参数（URL 路径里的变量）

### 1️⃣ 路由定义

```go
r.GET("/users/:id", handler)
```

这里的 `:id` 是 **占位符**

---

### 2️⃣ 请求长这样

```http
GET /users/100
```

---

### 3️⃣ Gin 里怎么取？

```go
id := c.Param("id")
```

此时：

```go
id == "100"
```

---

### 📌 Path 参数的特点

* 是 **路径的一部分**
* 通常表示 **资源 ID**

---

### 📌 常见用途

* `/users/1`
* `/orders/1001`
* `/articles/23`

---

### ❗常见误区

❌ `c.Param` 取不到 `?page=1`
❌ Path 参数也 **是 string**

---

## 三、Header（请求头）

### 1️⃣ Header 是什么？

HTTP 请求的“附加信息”，比如：

```http
Authorization: Bearer xxx.yyy.zzz
Content-Type: application/json
```

---

### 2️⃣ Gin 里怎么取？

```go
token := c.GetHeader("Authorization")
```

---

### 3️⃣ 最经典用途（你已经在用）

```go
Authorization: Bearer JWT_TOKEN
```

```go
auth := c.GetHeader("Authorization")
```

---

### 📌 Header 常见用途

* JWT 鉴权
* 用户身份
* 客户端信息

---

### ❗常见误区

❌ Header 不在 Body 里
❌ Header 不会被 ShouldBindJSON 解析

---

## 四、Body（请求体 JSON）

### 1️⃣ 请求长什么样？

```http
POST /login
Content-Type: application/json

{
  "email": "test@example.com"
}
```

---

### 2️⃣ 定义接收结构体

```go
type LoginRequest struct {
	Email string `json:"email"`
}
```

---

### 3️⃣ 绑定 JSON

```go
var req LoginRequest
err := c.ShouldBindJSON(&req)
```

发生了什么？

1️⃣ 读取请求体
2️⃣ 解析 JSON
3️⃣ 按 `json` tag 填充结构体
4️⃣ 自动校验 binding 规则

---

### 📌 Body 适合放什么？

* 创建数据
* 更新数据
* 登录信息

---

### ❗常见误区（非常重要）

❌ GET 请求一般 **不带 Body**
❌ JSON key 和 struct tag 不一致 → 绑定失败
❌ 忘记判断 `err`

---

## 🧠 四种参数对比（你要记住的表）

| 来源     | 示例              | Gin 方法             | 用途      |
| ------ | --------------- | ------------------ | ------- |
| Query  | `?page=1`       | `c.Query()`        | 分页 / 搜索 |
| Path   | `/users/1`      | `c.Param()`        | 资源 ID   |
| Header | `Authorization` | `c.GetHeader()`    | 鉴权      |
| Body   | JSON            | `ShouldBindJSON()` | 创建 / 更新 |

---

## ✅ 一句话总结（你背这个）

> **Gin 请求参数只来自 4 个地方：Query、Path、Header、Body，
> Query 和 Path 是字符串，Body 用 Bind，Header 常用于鉴权。**

---

#  Bind 绑定器（ShouldBind）

### 一、为什么需要 Bind？（先解决“为啥要它”）

### 你先想一个问题👇

**前端发过来的 JSON，本质上是什么？**

```json
{
  "name": "Tom",
  "email": "tom@test.com"
}
```

👉 **它只是一个字符串 + 字节流**

Go 代码 **不能直接用这个 JSON**。

你最终想要的是：

```go
req.Name
req.Email
```

也就是说：

> **JSON → Go 结构体**

---

## ❌ 没有 Bind 会怎样？

你得自己写：

* 读 Body
* json.Unmarshal
* 校验字段是否为空
* 返回错误

**非常麻烦，而且容易错**

---

### ✅ Bind 的作用（一句话）

> **Bind = 自动帮你把“请求数据” → “Go 结构体”，并顺便做校验**

---

### 二、JSON 绑定结构体是干嘛的？（逐行拆）

```go
type CreateUserReq struct {
	Name  string `json:"name" binding:"required"`
	Email string `json:"email" binding:"required"`
}
```

这一段**非常重要**，我们一行一行看。

---

## 1️⃣ `type CreateUserReq struct {}`

这是一个 **请求 DTO（请求对象）**

👉 含义是：

> **“创建用户接口，允许前端传什么字段”**

📌 它不是数据库模型
📌 它不是 model.User
📌 它只代表 **请求参数**

---

### 2️⃣ `Name string`

Go 里的字段名

⚠️ **必须首字母大写**

不然：

* Gin
* json
* reflect

全都拿不到

---

## 3️⃣ `json:"name"` 是干嘛的？（超级重要）

```go
Name string `json:"name"`
```

意思是：

> **前端 JSON 里的 `name` → 绑定到 Go 的 `Name` 字段**

### 如果没有这个 tag 会怎样？

```json
{
  "name": "Tom"
}
```

Gin 会找：

```go
req.Name   ❌（找不到）
req.name   ❌（Go 不允许）
```

👉 **绑定失败**

---

### 4️⃣ `binding:"required"` 是干嘛的？

```go
binding:"required"
```

意思是：

> **这个字段必须存在，不能为空**

### 举例

前端发：

```json
{
  "email": "a@test.com"
}
```

❌ 没有 name
👉 **直接报错**

Gin 会返回：

```text
Key: 'CreateUserReq.Name' Error:Field validation for 'Name' failed on the 'required' tag
```

---

### 三、核心代码：ShouldBindJSON（重点）

```go
var req CreateUserReq
err := c.ShouldBindJSON(&req)
```

这一段是 **核心中的核心**。

---

### 1️⃣ `var req CreateUserReq`

你准备了一个 **空盒子**

```go
req := CreateUserReq{
	Name:  "",
	Email: "",
}
```

---

### 2️⃣ `c.ShouldBindJSON(&req)` 干了什么？

它 **一次性做了 3 件事** 👇

### ✅ 第 1 件事：读请求 Body

```http
POST /users
Content-Type: application/json

{
  "name": "Tom",
  "email": "tom@test.com"
}
```

---

### ✅ 第 2 件事：JSON → 结构体（反序列化）

等价于你手写的：

```go
json.Unmarshal(body, &req)
```

结果：

```go
req.Name  = "Tom"
req.Email = "tom@test.com"
```

---

### ✅ 第 3 件事：字段校验（binding）

* `required`
* `email`
* `min=6`

只要 **有一个不满足**：

👉 返回 `err`

---

### 3️⃣ 为什么要 `&req`？

```go
&req // 指针
```

因为：

* Gin 要 **修改 req 的值**
* Go 只能通过指针修改外部变量

❌ 写成 `req` 会直接报错

---

#### 四、常见 binding 规则（你一定会用）

### 1️⃣ 必填

```go
binding:"required"
```

---

### 2️⃣ 邮箱格式

```go
binding:"email"
```

---

### 3️⃣ 最小长度

```go
binding:"min=6"`
```

---

### 组合使用

```go
Email string `json:"email" binding:"required,email"`
```

👉 **顺序不重要，用逗号隔开**

---

### 五、完整正确写法（你以后就照这个抄）

```go
func CreateUser(c *gin.Context) {
	var req CreateUserReq

	if err := c.ShouldBindJSON(&req); err != nil {
		c.JSON(400, gin.H{
			"msg": err.Error(),
		})
		return
	}

	// 到这里说明：
	// 1. JSON 合法
	// 2. 字段齐全
	// 3. 校验通过
}
```

---

### 六、最容易踩的坑（重点记）

### ❌ 1. json tag 不一致

```go
Name string `json:"username"`
```

前端传：

```json
{ "name": "Tom" }
```

👉 绑定失败

---

### ❌ 2. 忘记判断 err

```go
c.ShouldBindJSON(&req)
// ❌ 不判断 err，数据是脏的
```

---

### ❌ 3. 字段小写

```go
name string // ❌ 永远绑定不上
```

---

### 七、为什么说 Bind 是“核心”？

因为它：

* 解耦 Controller 和 Model
* 自动校验参数
* 防止脏数据进数据库
* 让代码非常干净

---

### 八、一句话总结（背下来）

> **Bind = 把请求数据安全、自动地变成 Go 结构体
> 是 Gin 参数校验 + 反序列化的核心**

---

# 文件上传 & 下载

### 文件上传

```go
file, _ := c.FormFile("file")
c.SaveUploadedFile(file, "./upload/"+file.Filename)
```

---

### 文件下载

```go
c.File("./test.pdf")
```

---

# 路由 & 路由组

### 普通路由

```go
r.GET("/ping", handler)
r.POST("/login", handler)
```

---

### 路由组（常用于权限）

```go
auth := r.Group("/")
auth.Use(loginMiddleware())

auth.GET("/profile", handler)
```

---

# 中间件（Middleware）


### 1️⃣ 用一句大白话说

> **中间件 = 在请求到达接口之前 / 接口返回之后，插一段你自己的代码**

它不负责“业务”，
它负责的是 **流程控制**。

---

### 2️⃣ 请求在 Gin 里的完整流程

```
浏览器请求
   ↓
全局中间件
   ↓
路由组中间件
   ↓
具体接口 Handler
   ↓
路由组中间件（after）
   ↓
全局中间件（after）
   ↓
返回响应
```

⚠️ **关键点：中间件是“包裹”接口的**

---

## 二、中间件的标准结构（必须背下来）

```go
func MyMiddleware() gin.HandlerFunc {
	return func(c *gin.Context) {
		// 1️⃣ 请求前（before）
		
		c.Next() // ⭐ 放行，进入下一个中间件 / 接口
		
		// 2️⃣ 响应后（after）
	}
}
```

### 每一行在干什么？

```go
func MyMiddleware() gin.HandlerFunc {
```

👉 Gin 规定：**中间件必须返回 gin.HandlerFunc**

---

```go
return func(c *gin.Context) {
```

👉 `c` 就是这一次 HTTP 请求的 **上下文**

---

```go
c.Next()
```

👉 **这是中间件最关键的一行**

* 不写 → 请求会被卡死
* 写了 → 请求继续往下走

---

### 举个你能“看到顺序”的例子

```go
func TestMiddleware() gin.HandlerFunc {
	return func(c *gin.Context) {
		fmt.Println("A before")
		c.Next()
		fmt.Println("A after")
	}
}
```

接口里：

```go
fmt.Println("B handler")
```

实际输出顺序：

```
A before
B handler
A after
```

👉 **中间件像“洋葱模型”**

---

## 三、全局中间件（所有接口都会经过）

### 1️⃣ 写法

```go
r.Use(MyMiddleware())
```

---

### 2️⃣ 什么时候用全局中间件？

适合放：

* 日志
* 统一鉴权（如 JWT）
* 请求耗时统计
* 跨域（CORS）

---

### 3️⃣ 对应你写过的代码

你之前写的：

```go
router.Use(LoggerMiddleWare())
```

这就是一个 **标准全局中间件**

---

## 四、路由分组（非常重要）

### 1️⃣ 路由分组是干嘛的？

> **把“同一类接口”放在一起，并统一加规则**

比如：

* 都需要登录
* 都是 `/users` 开头
* 都是后台接口

---

### 2️⃣ 路由分组基本结构

```go
user := r.Group("/users")
```

这行代码做了两件事：

1. URL 自动加 `/users`
2. 可以单独加中间件

---

### 3️⃣ 给路由组加中间件

```go
user.Use(JWTAuth())
```

👉 **只有这个组里的接口才会走 JWT 校验**

---

### 4️⃣ 完整示例（你一定要能写出来）

```go
user := r.Group("/users")
user.Use(AuthMiddleware())
{
	user.GET("", GetUserList)
	user.POST("", CreateUser)
}
```

对应 URL：

```
GET  /users
POST /users
```

---

### 5️⃣ 和全局中间件的区别（很重要）

| 类型        | 作用范围 |
| --------- | ---- |
| r.Use     | 所有接口 |
| group.Use | 这个分组 |

---

## 五、终止请求（Abort）

### 1️⃣ Abort 是干嘛的？

> **立刻停止请求，不再进入后面的中间件 / 接口**

---

### 2️⃣ 标准用法（你已经写过）

```go
if token == "" {
	c.JSON(401, gin.H{"msg": "未登录"})
	c.Abort()
	return
}
```

---

### 3️⃣ 如果不 Abort 会发生什么？

❌ **即使你已经返回 JSON，接口还是会继续执行**

这是很多新手的坑。

---

### 4️⃣ 推荐写法（一步到位）

```go
c.AbortWithStatusJSON(401, gin.H{
	"msg": "token 无效",
})
```

---

## 六、中间件里常用的 Context 操作

### 1️⃣ 设置值（跨接口传数据）

```go
c.Set("userID", user.ID)
```

---

### 2️⃣ 获取值

```go
userID, _ := c.Get("userID")
```

👉 常用于：

* 登录用户信息
* 权限判断

---

# 日志（Logging）详解

---

### 一、Gin 默认日志

```go
r := gin.Default()
```

它默认帮你做了：

* 请求方法
* 路径
* 状态码
* 耗时

输出示例：

```
[GIN] 200 | 12ms | GET /users
```

---

### 二、自定义日志中间件（你能控制）

```go
func Logger() gin.HandlerFunc {
	return func(c *gin.Context) {
		start := time.Now()
		c.Next()
		fmt.Println("耗时：", time.Since(start))
	}
}
```

---

### 三、生产环境建议（很重要）

❌ 不推荐：

```go
fmt.Println()
```

✅ 推荐：

* `log`
* `zap`
* `logrus`

原因：

* 性能更好
* 支持级别
* 支持文件输出

---

## 八、一句话总结（必须记住）

> **中间件 = 控制请求流程（前 / 后）
> 路由组 = 模块化 + 统一规则
> Abort = 立即拦截请求**

---

## 🔟 Gin 项目常见分层（你已经在用）

```
controller  → 接收请求 / 返回响应
service     → 业务逻辑
model       → 数据结构
dao         → 数据库操作
```

核心原则：

> **Controller 不直接操作 DB
> Service 才是业务核心**

---

## 📌 总结一句话（非常重要）

> **Gin 本质就是：**

* 路由
* 请求解析
* 响应返回
* 中间件控制流程

---

如果你愿意，下一步我可以帮你把这份 **Gin 笔记**：

* ✅ 拆成多个 `.md` 文件
* ✅ 或整理成「面试版 Gin 知识清单」
* ✅ 或直接 **对照你现在的项目结构再总结一版**

你选一个方向，我继续陪你整理 👌
