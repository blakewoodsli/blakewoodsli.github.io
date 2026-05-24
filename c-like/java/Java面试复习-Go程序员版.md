# Java 面试复习 · Go 程序员通俗版

> 你已经会 Go，很多概念其实**换了个名字，本质一样**。  
> 这份文档用 Go 来对照 Java / Spring / Vue，帮你快速建立直觉。

---

## 先记住一句话

```
Go 里你怎么写 HTTP 服务 + 调数据库 + 返回 JSON
Java 里就是 Spring Boot 帮你把这套东西标准化、企业化了
```

你做的 Vue 聊天项目，就像：

```
Go 后端（gin/echo）  ←→  你现在的 Node/Express 后端
Vue 前端              ←→  任何前端，发 HTTP 请求拿 JSON
```

---

## 一、Java 基础 ≈ Go 里你已经见过的东西

### 1.1 类型和变量

| Go | Java | 通俗理解 |
|----|------|----------|
| `int`, `string`, `bool` | `int`, `String`, `boolean` | 一样，基本类型 |
| `var name string` | `String name = "张三"` | 声明变量 |
| 没有 class，有 struct | 有 class | Java 更强调「类」 |

**Go 写法：**
```go
type User struct {
    ID   int
    Name string
}
```

**Java 写法：**
```java
class User {
    private int id;
    private String name;
}
```

👉 **本质一样**：都是把数据打包成一个结构。Java 叫 class，Go 叫 struct。

---

### 1.2 面向对象（OOP）—— Go 没有，但概念简单

JD 老提 OOP，其实就 4 个词：

| 词 | 用 Go 类比 | 一句话 |
|----|-----------|--------|
| **封装** | 小写字段不导出，大写导出 | 该隐藏的隐藏，该暴露的暴露 |
| **继承** | Go 用「嵌入 struct」代替 | 子类复用父类的东西 |
| **多态** | 接口 interface | 同一接口，不同实现 |
| **抽象** | Go 的 interface 只有方法签名 | 只规定「能做什么」，不管怎么做 |

**你 Go 里天天用的 interface，就是 Java 面试里说的「多态」：**

```go
// Go
type Reader interface {
    Read(p []byte) (n int, err error)
}
// 文件、网络、内存都可以实现 Reader
```

```java
// Java 类似
interface Reader {
    int read(byte[] p);
}
```

👉 **面试怎么说：**  
「Go 里我用 interface 做抽象和多态，Java 里用 interface 和 abstract class，思路一样。」

---

### 1.3 集合 ≈ Go 的 slice 和 map

| Go | Java | 干什么用 |
|----|------|----------|
| `[]int` slice | `ArrayList` | 有序列表，可重复 |
| `map[string]int` | `HashMap` | 键值对，快速查找 |

```go
// Go
users := []string{"张三", "李四"}
scores := map[string]int{"张三": 90}
```

```java
// Java
List<String> users = new ArrayList<>();
users.add("张三");

Map<String, Integer> scores = new HashMap<>();
scores.put("张三", 90);
```

👉 **ArrayList = slice，HashMap = map**，背这一句就够应付很多题。

---

### 1.4 异常：Go 的 error vs Java 的 try-catch

**Go：返回 error**
```go
result/screenshot, err := io.ReadAll(r)
if err != nil {
    return err
}
```

**Java：try-catch 捕获**
```java
try {
    // 可能出错的代码
} catch (Exception e) {
    // 出错了怎么办
}
```

👉 Go 用 `if err != nil` 一路传；Java 用 try-catch 包一层。**都是处理错误，风格不同。**

---

## 二、Spring 三层架构 ≈ Go 项目怎么分层

### 2.1 你在 Go 里可能这样写过一个 API

```go
// 类似 main.go 或 handler.go —— 接 HTTP 请求
func GetUser(w http.ResponseWriter, r *http.Request) {
    id := r.URL.Query().Get("id")
    
    // 业务逻辑
    user, err := userService.GetByID(id)
    if err != nil {
        http.Error(w, err.Error(), 500)
        return
    }
    
    json.NewEncoder(w).Encode(user)
}

// 类似 service.go —— 业务层
func (s *UserService) GetByID(id string) (*User, error) {
  return s.repo.FindByID(id)
}

// 类似 repo.go —— 访问数据库
func (r *UserRepo) FindByID(id string) (*User, error) {
    row := db.QueryRow("SELECT id, name FROM user WHERE id = ?", id)
    // ...
}
```

### 2.2 Java Spring 就是把上面三层「规定好名字」

```
Go 项目                    Java Spring 项目
─────────────────────────────────────────────
handler / controller   →   Controller 层   @RestController
service                →   Service 层      @Service
repository / repo      →   Mapper/DAO 层   @Mapper (MyBatis)
database/sql           →   MySQL + MyBatis 写 SQL
```

**画成图：**

```
浏览器 / 小程序
      ↓ HTTP
┌──────────────┐
│  Controller  │  接请求，像 gin 的 handler
└──────────────┘
      ↓
┌──────────────┐
│   Service    │  业务逻辑，像你的 service.go
└──────────────┘
      ↓
┌──────────────┐
│    Mapper    │  执行 SQL，像 repository
└──────────────┘
      ↓
    数据库
```

👉 **你已经有分层思维了，Spring 只是帮你用注解标清楚「这是哪一层」。**

---

### 2.3 Spring Boot ≈ Go 里「一个 main 就能跑」

| Go | Java Spring Boot |
|----|------------------|
| `go run main.go` | `mvn spring-boot:run` |
| 自己 import 包 | Maven 自动拉依赖（像 go mod） |
| 自己 `http.ListenAndServe` | 内嵌 Tomcat，不用单独装 Web 服务器 |
| `go.mod` | `pom.xml`（Maven 配置文件） |

**Spring Boot 示例（对照 Go handler）：**

```java
@RestController                    // 我是 HTTP 处理器
@RequestMapping("/api/user")
public class UserController {

    @Autowired                     // 自动注入，不用自己 new
    UserService userService;

    @GetMapping("/{id}")           // GET /api/user/5
    public User getUser(@PathVariable Long id) {
        return userService.getById(id);   // 交给 Service
    }
}
```

**Go 对照：**
```go
// r.GET("/api/user/:id", getUser)
func getUser(c *gin.Context) {
    id := c.Param("id")
    user := userService.GetById(id)
    c.JSON(200, user)
}
```

👉 **@GetMapping = r.GET，@PathVariable = c.Param，@Autowired = 依赖注入（不用手动 new）。**

---

### 2.4 MyBatis ≈ Go 里写 SQL + 扫描到 struct

**Go（database/sql）：**
```go
var u User
err := db.QueryRow("SELECT id, name FROM user WHERE id = ?", id).Scan(&u.ID, &u.Name)
```

**Java（MyBatis）：**
```java
// Mapper 接口
@Mapper
public interface UserMapper {
    User selectById(Long id);
}
```

```xml
<!-- 对应 SQL 文件 -->
<select id="selectById" resultType="User">
    SELECT id, name FROM user WHERE id = #{id}
</select>
```

👉 **`#{id}` = Go 里的 `?` 占位符，防 SQL 注入。**  
👉 MyBatis 就是：**SQL 写外面，结果自动映射到 Java 对象**，和 Go 的 Scan 一个意思。

---

## 三、SQL —— 和 Go 里查库一样，只是语法固定

你在 Go 里可能写过：

```go
db.Exec("INSERT INTO user (name, age) VALUES (?, ?)", "张三", 20)
db.Query("SELECT * FROM user WHERE age > ?", 18)
```

SQL 语句本身**语言无关**，Java 项目里 SQL 长这样：

```sql
-- 查
SELECT * FROM user WHERE age > 18;

-- 增
INSERT INTO user (name, age) VALUES ('张三', 20);

-- 改
UPDATE user SET age = 21 WHERE id = 1;

-- 删
DELETE FROM user WHERE id = 1;

-- 两表联查（像 Go 里 JOIN 两次查合成一次）
SELECT u.name, o.order_no
FROM user u
JOIN orders o ON u.id = o.user_id;
```

**必记 3 个概念：**

| 概念 | 类比 |
|------|------|
| 主键 id | Go struct 里的唯一 ID |
| 外键 user_id | 指向另一张表的 id，像关联 |
| 索引 | 给数据库加「目录」，查得快 |

👉 **SQL 部分和 Go/Java 无关，背 5 条语句就够应付实习面试。**

---

## 四、Vue 前端 ≈ Go 后端返回 JSON，前端负责展示

### 4.1 你已经做过的聊天项目，用 Go 思维理解

```
┌─────────────┐    POST /api/chat     ┌─────────────┐
│  Vue 前端    │  { question: "你好" }  │  后端        │
│  (浏览器)    │ ──────────────────→  │  Node/Go/Java│
│             │ ←──────────────────  │             │
│             │  { reply: "你好呀" }  │             │
└─────────────┘                       └─────────────┘
```

**Go 后端等价代码：**
```go
func chatHandler(w http.ResponseWriter, r *http.Request) {
    var req struct { Question string `json:"question"` }
    json.NewDecoder(r.Body).Decode(&req)
    
    reply := callAI(req.Question)
    
    json.NewEncoder(w).Encode(map[string]string{"reply": reply})
}
```

**Vue 前端做的事：**
```js
// 就像 Go 里 http.Post，只是跑在浏览器里
const res = await axios.post('/api/chat', { question: '你好' })
console.log(res.data.reply)  // "你好呀"
```

👉 **Vue 不负责业务逻辑，只管：显示界面 + 发 HTTP 请求 + 把结果显示出来。**  
👉 这和 Go 写 API 是**配套的**，你后端会了，前端就是「调用方」。

---

### 4.2 Vue 几个语法，用 Go/JS 思维理解

| Vue 语法 | 干什么 | 类似什么 |
|----------|--------|----------|
| `{{ name }}` | 页面上显示变量 | `fmt.Printf("%s", name)` |
| `v-for="item in list"` | 循环渲染列表 | `for _, item := range list` |
| `v-if="show"` | 条件显示 | `if show { ... }` |
| `v-model="input"` | 输入框和变量双向绑定 | 输入变化，变量自动变 |
| `@click="send"` | 点击触发函数 | `button.OnClick(send)` |
| `ref([])` | 响应式变量 | 变量变了，页面自动刷新 |

**你项目里的 messages：**

```js
// 像 Go 的 slice
const messages = ref([
  { role: 'user', content: '你好' },
  { role: 'ai', content: '你好呀' }
])

// 发消息 = append
messages.value.push({ role: 'user', content: text })
```

👉 **messages 就是 `[]Message` 结构体切片，push 就是 append，页面自动重绘。**

---

## 五、整个技术栈，用一张 Go 地图串起来

```
                    你已有的 Go 经验
                          ↓
    ┌─────────────────────────────────────────┐
    │  HTTP 服务                               │
    │  Go: gin/echo + handler                 │
    │  Java: Spring Boot + @RestController    │
    ├─────────────────────────────────────────┤
    │  业务逻辑                                │
    │  Go: service.go                         │
    │  Java: @Service                         │
    ├─────────────────────────────────────────┤
    │  数据库                                  │
    │  Go: database/sql + ? 占位符             │
    │  Java: MyBatis + #{} 占位符              │
    ├─────────────────────────────────────────┤
    │  依赖管理                                │
    │  Go: go.mod                             │
    │  Java: pom.xml (Maven)                  │
    ├─────────────────────────────────────────┤
    │  前端（Go 一般不写，但你要会对接）          │
    │  Vue 发 axios 请求 → 你的 API 返回 JSON   │
    └─────────────────────────────────────────┘
```

---

## 六、JD 里的词，Go 程序员 5 秒理解版

| JD 写的 | Go 对照 | 一句话 |
|---------|---------|--------|
| Spring Boot | gin + 自动配置 | 快速起 Web 服务 |
| Spring MVC | gin 路由 | URL 映射到函数 |
| MyBatis | database/sql | 写 SQL 查库 |
| Maven | go mod | 管依赖 |
| Git/SVN | git | 版本控制 |
| PostgreSQL/MySQL | 任何 SQL 数据库 | 存储数据 |
| Vue.js | 前端框架 | 调你的 API 显示页面 |
| uni-app | 跨端 Vue | 一套代码出小程序+App |
| Spring Security | 中间件鉴权 | 登录、权限检查 |
| Linux | 服务器系统 | 部署 Go/Java 程序的环境 |

---

## 七、面试怎么说（Go 背景版自我介绍）

```
面试官您好，我叫【姓名】。

我有 Go 语言基础，理解 HTTP 服务、分层架构和 SQL 操作。
最近做了一个 AI 聊天项目：前端 Vue 3 发 HTTP 请求，
后端提供 REST API 返回 JSON，和 Go 里写 handler 返回 json.Encode 是一样的思路。

我了解 Java 这边 Spring Boot 把 handler/service/repository 
分成 Controller、Service、Mapper 三层，MyBatis 负责 SQL，
和我 Go 项目的分层是对应的，只是语法和生态不同。

SQL、Git、Linux 基本操作我都能上手，学习能力强，希望来贵司实习。
```

---

## 八、今晚最少要弄懂的 6 件事（Go 版 checklist）

- [ ] **1. 三层架构** = Go 的 handler → service → repo，换了个名字
- [ ] **2. Spring Boot** = `go run` 一把梭起 Web 服务
- [ ] **3. MyBatis** = `database/sql` + SQL 映射到 struct
- [ ] **4. 5 条 SQL** = 和 Go 里 Exec/Query 用的 SQL 一样
- [ ] **5. Vue 项目** = 前端调 API，和你写 Go API 是配套的
- [ ] **6. 背自我介绍** = 强调「Go 有基础，Java 分层我理解了」

---

## 九、3 道自测题（用 Go 思维答）

**1. Spring 三层分别对应 Go 什么？**

<details>
<summary>答案</summary>

Controller = handler（接 HTTP）  
Service = service.go（业务逻辑）  
Mapper = repository（执行 SQL）
</details>

**2. MyBatis 的 `#{}` 对应 Go 什么？**

<details>
<summary>答案</summary>

Go 里 SQL 的 `?` 占位符，都是参数化查询，防 SQL 注入。
</details>

**3. 你的 Vue 聊天项目和 Go 后端怎么配合？**

<details>
<summary>答案</summary>

Vue 用 axios POST `{ question }` 到 `/api/chat`，
后端（Node/Go/Java 都行）收到后调 AI，返回 `{ reply }` JSON，
Vue 把 reply 显示在页面上。和 Go 写 REST API 完全一样。
</details>

---

## 十、还看不懂某一块？直接对照这张表

| 看不懂的 | 去想 Go 里的 |
|----------|-------------|
| class | struct + 方法 |
| interface | Go 的 interface |
| ArrayList | slice |
| HashMap | map |
| @RestController | gin 的路由 handler |
| @Autowired | 依赖注入，不用手动 new |
| @Service | service 包 |
| @Mapper | repository 包 |
| Maven | go mod |
| try-catch | if err != nil 的 Java 版 |
| Vue ref | 一个会自动刷新页面的变量 |
| axios | http.Client / http.Post |

---

*你会 Go，Java 后端只是换语法和框架，核心思路你已经有了。加油！*
