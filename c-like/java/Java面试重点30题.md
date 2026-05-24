# Java 实习面试 · 重点 30 题（精简版）

> 只背这些就够应付大部分实习面试。每题 2～3 句，能说出来即可。

---

## 一、必背 3 句话（Spring）

| 框架 | 背这句 |
|------|--------|
| **Spring Boot** | 简化 Spring 配置，内嵌 Tomcat，写 main 就能启动 Web 项目。 |
| **Spring MVC** | 处理 HTTP 请求，把 URL 映射到 Java 方法，返回 JSON。 |
| **MyBatis** | 写 SQL 访问数据库，结果自动映射成 Java 对象；`#{}` 防 SQL 注入。 |

---

## 二、必画 1 张图（三层架构）

```
浏览器(Vue) → Controller(接请求) → Service(业务) → Mapper(查库) → 数据库
```

**你项目对照：**

| 层 | 你的 server.js |
|----|----------------|
| Controller | 第 15 行 `app.post`，第 18 行校验，第 43 行 `res.json` |
| Service | 第 22～41 行组消息、调 AI |
| Mapper | 第 23 行 `axios.post` 调 AI |
| 数据库 | AI 远程接口（Java 项目一般是 MySQL） |

**为什么分层：** 各干各的，好维护。Go 里就是 handler → service → repo。

---

## 三、必背 10 个注解

| 注解 | 干什么 |
|------|--------|
| `@RestController` | API 控制器，返回 JSON |
| `@Service` | 业务层 |
| `@Mapper` | 数据层（MyBatis） |
| `@Autowired` | 自动注入，不用 new |
| `@GetMapping` | GET 请求 |
| `@PostMapping` | POST 请求 |
| `@PathVariable` | URL 参数 `/user/{id}` |
| `@RequestBody` | 请求体 JSON |
| `@RequestParam` | `?name=xx` 查询参数 |

---

## 四、必会 5 条 SQL（能手写）

```sql
-- 1. 条件查询
SELECT * FROM user WHERE age > 18 ORDER BY age DESC;

-- 2. 插入
INSERT INTO user (name, age) VALUES ('张三', 20);

-- 3. 修改（一定要 WHERE）
UPDATE user SET age = 21 WHERE id = 1;

-- 4. 联表
SELECT u.name, o.order_no FROM user u
JOIN orders o ON u.id = o.user_id;

-- 5. 分组统计
SELECT department, COUNT(*) FROM user GROUP BY department;
```

**3 个概念：** 主键（唯一 id）、外键（关联另一张表）、索引（加快查询）。

---

## 五、Java 基础（10 题）

### 1. Java 和 JavaScript 区别？
Java 做后端，跑 JVM；JavaScript 做前端/Node。**不是同一种语言。**

### 2. == 和 equals？
`==` 比地址；`equals` 比内容（String 已重写）。

### 3. 接口和抽象类？
接口多实现、定规范；抽象类单继承、可有实现。≈ Go 的 interface。

### 4. ArrayList 和 HashMap？
ArrayList = 有序列表 ≈ slice；HashMap = 键值对 ≈ map。

### 5. 重载和重写？
重载：同类同名不同参数；重写：子类改父类方法。

### 6. try-catch 干什么？
捕获异常，防止程序崩溃。≈ Go 的 `if err != nil`。

### 7. public / private？
public 哪都能访问；private 只能本类。

### 8. static 是什么？
属于类，不属于对象；`类名.方法()` 调用。

### 9. 面向对象四特性？
封装、继承、多态、抽象。

### 10. 什么是多态？
父类引用指向子类对象，调用方法走子类实现。

---

## 六、网络 / HTTP（5 题）

### 11. GET 和 POST？
GET 查数据，参数在 URL；POST 提交数据，参数在 body。

### 12. 200、404、500？
200 成功；404 找不到；500 服务器错误。你项目：400 没传 question。

### 13. 前后端分离？
前端 Vue 展示；后端 Spring 提供 JSON API；HTTP 通信。

### 14. Cookie 和 Session？
Cookie 在浏览器；Session 在服务器；常用 Cookie 存 Session ID。

### 15. 什么是 CORS？
跨域限制；前端 5173 访问后端 7666 要后端开 `cors()`。

---

## 七、MyBatis / 数据库（5 题）

### 16. #{} 和 ${}？
`#{}` 预编译，防注入 ✅；`${}` 拼接，有风险 ❌。

### 17. 如何防 SQL 注入？
用 `#{}` 或 PreparedStatement，不拼接用户输入。

### 18. 事务是什么？
一组 SQL 要么全成功要么全失败。例：转账扣款+加款。

### 19. INNER JOIN 和 LEFT JOIN？
INNER：两表都有的；LEFT：保留左表全部。

### 20. 慢 SQL 怎么优化？
加索引、避免 SELECT *、用 EXPLAIN 分析。

---

## 八、你的项目（5 题，必背）

### 21. 介绍一下项目（30 秒）
> AI 聊天，Vue 3 前端 + Node 后端。用户输入问题，axios POST 到 `/api/chat`，后端调 AI 返回 reply，页面用 v-for 显示气泡。API Key 放后端 .env。

### 22. 发送一条消息的流程？
用户点发送 → push 用户消息 → push「正在生成」→ await 请求后端 → 替换 AI 内容 → 滚到底部。

### 23. 对应三层架构吗？
`app.post` 是 Controller；try 块调 AI 是 Service；axios 调 AI 是 Mapper；数据源是 AI 接口。

### 24. 为什么 Key 不能放前端？
前端代码用户都能看到，会泄露。放后端中转更安全。

### 25. Vue 用了哪些？
ref 响应式、v-for 列表、v-model 输入框、axios 请求、组件 ChatBox。

---

## 九、行为面试（5 题）

### 26. 遇到 bug 怎么办？
复现 → 看日志/控制台 → 定位前后端 → 查资料改 → 再测。

### 27. 不会的技术怎么办？
文档 → 教程 → 小 demo → 问同事。

### 28. 为什么来实习？
贵司 Java+Vue，和我方向一致，想在真实项目里练 Spring 和 SQL。

### 29. 优缺点？
优点：学习快，有 Go 和 Vue 项目。缺点：Java 企业经验少，正在补。

### 30. 问面试官什么？
实习生做什么？有没有导师？技术栈是 Spring Boot + MyBatis 吗？

---

## 十、1 分钟自我介绍（背这个）

```
您好，我叫【姓名】，【学校】【专业】学生，有 Go 基础。

最近做了 AI 聊天项目：Vue 3 前端，axios 调后端 API；
后端 Node 提供 REST 接口，我理解 Controller、Service、Mapper 分层，
正在学 Spring Boot 和 MyBatis。

学习能力强，希望来贵司实习参与 Java 项目维护开发。谢谢。
```

---

## 今晚只干这 5 件事（2 小时）

| 顺序 | 做什么 | 时间 |
|------|--------|------|
| 1 | 手画三层架构 + 对照 server.js | 15 分钟 |
| 2 | 背 Spring 三句话 + 10 注解 | 20 分钟 |
| 3 | 默写 5 条 SQL | 25 分钟 |
| 4 | 背项目 21～25 题 + 自我介绍 | 30 分钟 |
| 5 | 过一遍 Java/网络 1～20 题 | 30 分钟 |

---

## 明天出门前 5 分钟

1. 默画三层图  
2. 说一遍 30 秒项目介绍  
3. 说一遍自我介绍  

---

*详细版见：`Java实习面试题集.md`*
