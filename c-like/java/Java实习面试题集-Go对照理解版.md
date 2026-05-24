# Java 面试题 · Go 对照理解版

> 配合 `Java实习面试题集.md` 使用：左边是面试题，这里用 **Go 代码** 帮你理解。  
> 题号对应该题集章节，**重点 30 题** 标了 ⭐。

---

## 怎么用这份文档

1. 在 `Java实习面试题集.md` 看到不懂的题  
2. 来本文找同章节 / 同主题  
3. 先看 **Go 对照代码**，再看 **面试怎么说**

---

## 一、Java 基础

### ⭐ 题 1：Java 和 JavaScript 区别？

| | Java | JavaScript | Go |
|--|------|------------|-----|
| 跑在哪 | JVM | 浏览器 / Node | 编译成机器码直接跑 |
| 主要用途 | 企业后端 | 前端 | 后端、云原生 |
| 和 Go 关系 | 都是后端语言，**语法不同** | 前端语言，和 Go 无关 | 你已会 |

**别搞混：** Java ≠ JavaScript，就像 Go ≠ Golang 里的某个前端框架。

---

### ⭐ 题 3：== 和 equals？

**Go：** 没有 equals，struct 要自己比；`==` 比基本类型和可比较类型。

```go
a := "hi"
b := "hi"
a == b  // true，字符串可比
```

**Java：**

```java
String a = new String("hi");
String b = new String("hi");
a == b;       // false：两个不同对象（比地址）
a.equals(b);  // true：内容一样
```

👉 **记：** Java 的 `equals` ≈ Go 里自己写的 `func (u User) Equal(other User) bool`。

---

### ⭐ 题 9：接口和抽象类？

**Go 只有 interface（你一定会）：**

```go
type Storer interface {
    Save(user User) error
    FindByID(id int) (User, error)
}

// MySQL、内存、文件都可以实现 Storer
type MySQLRepo struct{}
func (r *MySQLRepo) Save(user User) error { ... }
```

**Java：**

```java
// 接口：只能规定方法，类 implements 实现
interface Storer {
    void save(User user);
    User findById(Long id);
}

// 抽象类：可以有已经写好的方法，类 extends 继承（只能继承一个）
abstract class BaseService {
    void log(String msg) { System.out.println(msg); }  // 已实现
    abstract void doWork();  // 子类必须实现
}
```

| | Go | Java |
|--|-----|------|
| 多实现 | 一个 struct 可实现多个 interface | 一个类可实现多个 interface |
| 继承 | 嵌入 struct | extends 只能继承一个类 |

**面试说：** Go 里我用 interface 做多态；Java 用 interface + abstract class，思路一样。

---

### 题 31：ArrayList 和 LinkedList？

**Go 只有 slice，一种动态数组：**

```go
users := []string{"张三", "李四"}
users = append(users, "王五")
```

**Java 两种列表：**

| | ArrayList | LinkedList |
|--|-----------|------------|
| 底层 | 数组 | 链表 |
| 查 | 快 | 慢 |
| 头尾增删 | 慢 | 快 |

👉 **实习只要记：** ArrayList ≈ slice，最常用；LinkedList 了解即可。

---

### ⭐ 题 32：HashMap？

**Go：**

```go
scores := map[string]int{
    "张三": 90,
    "李四": 85,
}
v, ok := scores["张三"]  // 90, true
```

**Java：**

```java
Map<String, Integer> scores = new HashMap<>();
scores.put("张三", 90);
scores.get("张三");  // 90
```

👉 **HashMap = map**，面试这么说就行。

---

### 题 8：异常 try-catch？

**Go：**

```go
user, err := repo.FindByID(1)
if err != nil {
    return err  // 每层处理或往上抛
}
```

**Java：**

```java
try {
    User user = userMapper.findById(1);
} catch (Exception e) {
    // 必须处理或声明抛出
    log.error(e.getMessage());
}
```

👉 **try-catch = Go 的 if err != nil**，只是 Java 用异常机制强制你处理。

---

### 题 10～11：重载 / 重写？

**Go 没有重载**（同名不同参数不行）。Java 可以：

```java
// 重载 overload：同类，方法名相同，参数不同
void save(User u) { }
void save(User u, String reason) { }

// 重写 override：子类改父类方法
class Dog extends Animal {
    @Override
    void speak() { System.out.println("汪"); }
}
```

**Go 用不同函数名代替重载：**

```go
func SaveUser(u User) { }
func SaveUserWithReason(u User, reason string) { }
```

---

### 题 22：多态？

**Go 标准写法（你肯定见过）：**

```go
type Animal interface {
    Speak() string
}

type Dog struct{}
func (d Dog) Speak() string { return "汪" }

func main() {
    var a Animal = Dog{}  // 接口变量指向具体类型
    fmt.Println(a.Speak()) // 汪
}
```

**Java：**

```java
Animal a = new Dog();
a.speak();  // 汪汪
```

👉 **一模一样**：接口类型变量，实际指向子类/实现类。

---

## 二、Spring / Spring Boot（最重要）

### ⭐ 题 42～44：Spring Boot / MVC / 三层

**用 Go + gin 一整段理解：**

```go
// ========== 相当于 @RestController ==========
func main() {
    r := gin.Default()
    r.GET("/api/user/:id", getUser)   // Spring MVC 路由
    r.POST("/api/chat", chat)
    r.Run(":8080")                    // Spring Boot 内嵌 Tomcat
}

// ========== Controller：只接 HTTP ==========
func getUser(c *gin.Context) {
    id := c.Param("id")
    if id == "" {
        c.JSON(400, gin.H{"error": "missing id"})
        return
    }
    user := userService.GetByID(id)      // 交给 Service
    c.JSON(200, user)                     // 返回 JSON
}

// ========== Service：业务逻辑 ==========
type UserService struct {
    repo *UserRepo
}

func (s *UserService) GetByID(id string) User {
    // 可以加：校验、缓存、调多个 repo
    return s.repo.FindByID(id)
}

// ========== Mapper/Repo：只操作数据 ==========
type UserRepo struct {
    db *sql.DB
}

func (r *UserRepo) FindByID(id string) User {
    row := r.db.QueryRow("SELECT id, name FROM user WHERE id = ?", id)
    var u User
    row.Scan(&u.ID, &u.Name)
    return u
}
```

**Java 拆成 3 个文件 + 注解：**

```java
@RestController
public class UserController {
    @Autowired UserService userService;
    @GetMapping("/api/user/{id}")
    public User getUser(@PathVariable String id) {
        return userService.getById(id);
    }
}

@Service
public class UserService {
    @Autowired UserMapper userMapper;
    public User getById(String id) { return userMapper.selectById(id); }
}

@Mapper
public interface UserMapper {
    User selectById(String id);  // SQL 在 XML 里
}
```

| Java | Go（你已会） |
|------|-------------|
| Spring Boot | `main` + `r.Run` |
| Spring MVC | `r.GET` / `r.POST` |
| @RestController | handler 函数 |
| @Service | service 包 |
| @Mapper | repository + database/sql |
| @Autowired | 手动传依赖，或 wire/fx 注入 |

---

### ⭐ 题 46：10 个注解 ↔ Go 写法

| Java 注解 | Go（gin）对照 |
|-----------|---------------|
| `@GetMapping("/user/{id}")` | `r.GET("/user/:id", handler)` |
| `@PostMapping` | `r.POST` |
| `@PathVariable Long id` | `c.Param("id")` |
| `@RequestParam String name` | `c.Query("name")` |
| `@RequestBody User user` | `c.ShouldBindJSON(&user)` 或 `json.NewDecoder(r.Body).Decode(&user)` |
| `@RestController` | handler 组，返回 JSON |
| `@Service` | `service` 包里的 struct |
| `@Mapper` | `repo` 包 |
| `@Autowired` | 构造时传入：`NewUserService(repo)` |

**你 Node 项目对照（更熟）：**

```js
// @PostMapping("/api/chat")
app.post('/api/chat', async (req, res) => { ... })

// @RequestBody
const { question } = req.body

// @Autowired → 不用写，require 进来就行
```

---

### ⭐ 题 47：依赖注入 DI？

**Go 手动注入（常见）：**

```go
repo := NewUserRepo(db)
svc := NewUserService(repo)   // 把 repo 传给 service
handler := NewUserHandler(svc)
```

**Java Spring 自动注入：**

```java
@Autowired
UserService userService;  // Spring 容器帮你 new 好并塞进来
```

👉 **DI = 别在类里面自己 new 依赖，从外面传进来。** Go 你手动传，Spring 自动传。

---

## 三、MyBatis 与 SQL

### ⭐ 题 56～57：MyBatis 和 #{}？

**Go 查库（你一定会）：**

```go
var u User
err := db.QueryRow(
    "SELECT id, name FROM user WHERE id = ?",
    id,
).Scan(&u.ID, &u.Name)
```

**Java MyBatis：**

```xml
<select id="selectById" resultType="User">
    SELECT id, name FROM user WHERE id = #{id}
</select>
```

| | Go | MyBatis |
|--|-----|---------|
| 占位符 | `?` | `#{}` |
| 防注入 | 预编译 | 预编译 |
| 危险写法 | 拼接字符串 | `${}` 拼接 |

```go
// 危险 ❌ 不要这样
query := fmt.Sprintf("SELECT * FROM user WHERE name = '%s'", name)
```

👉 **MyBatis = Go 的 database/sql + SQL 写在文件里 + 自动 Scan 到 struct。**

---

### ⭐ 题 71～80：SQL（Go 和 Java 用同一句 SQL）

**Go：**

```go
db.Exec("INSERT INTO user (name, age) VALUES (?, ?)", "张三", 20)
rows, _ := db.Query("SELECT * FROM user WHERE age > ?", 18)
```

**Java MyBatis：** SQL 字符串一样，只是写在 XML 里。

```sql
INSERT INTO user (name, age) VALUES ('张三', 20);
SELECT * FROM user WHERE age > 18 ORDER BY age DESC;
```

👉 **SQL 与语言无关**，会写 Go 的 SQL，Java 直接复用。

---

### 题 59：事务？

**Go：**

```go
tx, _ := db.Begin()
_, err1 := tx.Exec("UPDATE account SET balance = balance - 100 WHERE id = 1")
_, err2 := tx.Exec("UPDATE account SET balance = balance + 100 WHERE id = 2")
if err1 != nil || err2 != nil {
    tx.Rollback()
    return err
}
tx.Commit()
```

**Java（Spring 常见）：**

```java
@Transactional  // 方法里多条 SQL，出错自动回滚
public void transfer() {
    accountMapper.deduct(1, 100);
    accountMapper.add(2, 100);
}
```

👉 **事务 = 要么全成功要么全失败**，Go 手动 Begin/Commit，Spring 用注解。

---

## 四、HTTP / 网络

### ⭐ 题 81：GET 和 POST？

**Go：**

```go
r.GET("/api/user", listUsers)    // 查列表，参数常在 URL
r.POST("/api/user", createUser)  // 新增，参数在 body
```

**你 Vue 项目：**

```js
axios.post('/api/chat', { question: '你好' })  // POST + JSON body
```

| | GET | POST |
|--|-----|------|
| Go | `r.GET` | `r.POST` |
| 参数 | `c.Query("name")` | `c.ShouldBindJSON(&req)` |
| 用途 | 查询 | 提交 |

---

### ⭐ 题 82：状态码？

| 码 | 含义 | 你 server.js |
|----|------|--------------|
| 200 | 成功 | 正常返回 reply |
| 400 | 参数错了 | `!question` 时 |
| 500 | 服务器挂了 | catch 里 AI 失败 |

**Go：**

```go
c.JSON(200, user)
c.JSON(400, gin.H{"error": "bad request"})
c.JSON(500, gin.H{"error": "internal error"})
```

---

### ⭐ 题 85：CORS？

浏览器里：前端 `localhost:5173` 访问后端 `localhost:7666` = **跨域**。

**Go：**

```go
import "github.com/gin-contrib/cors"
r.Use(cors.Default())
```

**你项目：**

```js
app.use(cors())  // server.js 第 9 行
```

👉 后端允许别的端口访问，否则浏览器拦请求。

---

### 题 86：前后端分离？

```
Vue（浏览器）  --HTTP+JSON-->  Java/Go/Node 后端
```

**Go 后端只返回 JSON，不返回 HTML 页面** —— 这就是前后端分离。你的 AI 聊天项目就是。

---

## 五、Vue 相关（Go 后端视角）

### 题 91～94：Vue / ref / axios？

| Vue | Go 后端视角 |
|-----|-------------|
| `ref([])` | 前端维护的 `[]Message` 切片 |
| `v-for` | 前端 `for range` 渲染 HTML |
| `axios.post` | 前端帮你调 `http.Post` |
| `v-model` | 前端表单变量，你不用管 |

**你只要会写 Go API，前端 Vue 就是调用方：**

```go
// 后端提供
POST /api/chat  body: {"question":"你好"}  →  {"reply":"你好呀"}
```

```js
// 前端调用
const res = await axios.post('/api/chat', { question: '你好' })
console.log(res.data.reply)
```

---

## 六、项目题（结合你的 server.js）

### ⭐ 题 99～101：用 Go 思维讲项目

**发送流程 = Go handler 流程：**

```
1. 前端 POST { question }     →  c.ShouldBindJSON
2. Controller 校验 question   →  if question == "" { 400 }
3. Service 组消息调 AI        →  business logic
4. Mapper axios 调外部 API    →  http.Post 到 AI
5. 返回 { reply }             →  c.JSON(200, gin.H{"reply": reply})
```

**题 101 Key 为什么不能放前端？**

Go 里你也会：

```go
// ✅ 正确：Key 在环境变量
apiKey := os.Getenv("DEEPSEEK_API_KEY")

// ❌ 错误：写进发给前端的代码
// 前端 bundle 谁都能下载看到
```

和你 `.env` + `server.js` 第 13 行一样。

---

## 七、行为面试（不用 Go，背话术）

| 题 | 一句话 |
|----|--------|
| bug 怎么办 | 复现 → 日志 → 定位 → 改 → 测 |
| 不会怎么办 | 文档 → demo → 问人 |
| 为什么来 | Java+Vue 方向一致，想练企业项目 |
| 优缺点 | 学习快，有 Go+Vue；Java 企业经验在补 |

---

## 八、总对照表（打印这一张）

| Java 面试词 | 你已经会的 |
|-------------|-----------|
| class | struct + 方法 |
| interface | interface |
| ArrayList | slice |
| HashMap | map |
| try-catch | if err != nil |
| Spring Boot | main + gin.Run |
| Controller | handler |
| Service | service 包 |
| Mapper | repository |
| MyBatis #{} | SQL 的 ? |
| @GetMapping | r.GET |
| @PostMapping | r.POST |
| @RequestBody | json.Decode / ShouldBindJSON |
| @Autowired | 构造函数注入依赖 |
| Maven | go mod |
| 事务 | Begin / Commit / Rollback |
| REST API | 你写的 HTTP 接口 |
| Vue axios | http.Client 调用方 |

---

## 九、看不懂时查这里

| 题集里不懂的 | 看本文哪节 |
|-------------|-----------|
| 接口、抽象类、多态 | 一、题 9、22 |
| ArrayList、HashMap | 一、题 31、32 |
| Spring 三层 | 二、题 42～44 |
| 注解一堆 | 二、题 46 |
| MyBatis、#{} | 三、题 56～57 |
| SQL | 三、题 71～80 |
| GET/POST、CORS | 四 |
| 项目怎么讲 | 六 |

---

## 十、推荐学习顺序（有 Go 基础）

```
1. 二、Spring 三层（对照 gin 代码看）     ← 最重要
2. 三、MyBatis = database/sql
3. 四、HTTP（你已会在 Go 里写）
4. 一、Java 基础（当 Go 语法差异记）
5. 六、项目题（背流程）
```

---

*详细题库：`Java实习面试题集.md` · 精简背诵：`Java面试重点30题.md`*
