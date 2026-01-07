
# Redis 基础总结

- [Redis 基础总结](#redis-基础总结)
  - [Redis 是什么](#redis-是什么)
  - [安装 Redis](#安装-redis)
  - [ref](#ref)
    - [使用 Docker（推荐）](#使用-docker推荐)
  - [Redis 数据类型](#redis-数据类型)
    - [1️⃣ String（字符串）](#1️⃣-string字符串)
    - [2️⃣ Hash（哈希）](#2️⃣-hash哈希)
    - [3️⃣ List（列表）](#3️⃣-list列表)
    - [4️⃣ Set（集合）](#4️⃣-set集合)
    - [5️⃣ ZSet（有序集合）](#5️⃣-zset有序集合)
  - [Redis 常见使用场景](#redis-常见使用场景)
  - [命令行操作](#命令行操作)
    - [1️⃣ 进入 Redis 客户端](#1️⃣-进入-redis-客户端)
    - [2️⃣ 增（Create）](#2️⃣-增create)
      - [String](#string)
      - [Hash](#hash)
    - [3️⃣ 查（Read）](#3️⃣-查read)
      - [String](#string-1)
      - [Hash](#hash-1)
      - [查看所有 key（调试用）](#查看所有-key调试用)
    - [4️⃣ 删（Delete）](#4️⃣-删delete)
    - [5️⃣ 设置过期时间（EX）](#5️⃣-设置过期时间ex)
    - [6️⃣ 单独设置过期时间](#6️⃣-单独设置过期时间)
    - [7️⃣ 查看剩余存活时间（TTL）](#7️⃣-查看剩余存活时间ttl)
  - [使用示例](#使用示例)
  - [](#)
  - [推荐使用方式（重点）](#推荐使用方式重点)

## Redis 是什么

**概念说明：**

- 基于 **内存** 的 Key-Value 数据库
- 读写速度极快（微秒级）
- 常用于 **缓存、登录态、验证码、排行榜**

---

## 安装 Redis

## ref

- [Redis安装（Windows & macOS）](https://www.cnblogs.com/fortuneju/p/15407095.html)

### 使用 Docker（推荐）

```bash
docker run -d \
  --name redis \
  -p 6379:6379 \
  redis
```

**说明：**

- `-d`：后台运行
- `-p 6379:6379`：宿主机端口映射
- 默认无需配置即可使用

---

## Redis 数据类型

### 1️⃣ String（字符串）

**特点：**

- 最基础类型
- 一个 key 对应一个 value

**常见命令：**

```bash
SET key value
GET key
```

**使用场景：**

- token
- 验证码
- 简单状态值

---

### 2️⃣ Hash（哈希）

**特点：**

- 一个 key 下可以存多个 field
- 非常适合存 **对象**

**常见命令：**

```bash
HSET key field value
HGET key field
HGETALL key
```

**示例：**

```bash
HSET user:1 name Tom
HSET user:1 age 18
HGETALL user:1
```

---

### 3️⃣ List（列表）

**特点：**

- 有顺序
- 可重复

**常见命令：**

```bash
LPUSH key value
LRANGE key start stop
```

**使用场景：**

- 日志
- 消息队列

---

### 4️⃣ Set（集合）

**特点：**

- 无序
- 自动去重

**常见命令：**

```bash
SADD key member
SMEMBERS key
```

**使用场景：**

- 去重数据
- 点赞 / 收藏

---

### 5️⃣ ZSet（有序集合）

**特点：**

- 有序
- 每个元素有 score

**常见命令：**

```bash
ZADD key score member
ZRANGE key start stop WITHSCORES
```

**使用场景：**

- 排行榜
- 积分系统

---

## Redis 常见使用场景

- 用户信息缓存
- 登录 token
- 验证码
- 接口限流
- 排行榜

---

## 命令行操作

### 1️⃣ 进入 Redis 客户端

```bash
redis-cli
```

---

### 2️⃣ 增（Create）

#### String

```bash
SET age 18
```

#### Hash

```bash
HSET user:1 name Tom
HSET user:1 age 18
```

---

### 3️⃣ 查（Read）

#### String

```bash
GET age
```

#### Hash

```bash
HGET user:1 name
HGETALL user:1
```

#### 查看所有 key（调试用）

```bash
KEYS *
```

⚠️ **生产环境慎用**

---

### 4️⃣ 删（Delete）

```bash
DEL key
```

示例：

```bash
DEL age
DEL user:1
```

---

### 5️⃣ 设置过期时间（EX）

```bash
SET key value EX seconds
```

示例：

```bash
SET code:email:tom@qq.com 9527 EX 300
```

---

### 6️⃣ 单独设置过期时间

```bash
EXPIRE key seconds
```

示例：

```bash
EXPIRE user:1 60
```

---

### 7️⃣ 查看剩余存活时间（TTL）

```bash
TTL key
```

- `> 0`：剩余秒数
- `-1`：永久不过期
- `-2`：key 不存在

---

## 使用示例

![usage](./db-key.png)
---

## 推荐使用方式（重点）

✅ **Redis + MySQL**

- MySQL：持久化存储
- Redis：缓存层，加速查询

👉 **标准架构：**

```bash
请求 → Redis（有数据直接返回）
        ↓
      MySQL → 写回 Redis
```
