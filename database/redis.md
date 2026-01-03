# Redis 基础总结

## 一、Redis 是什么
- 基于内存的 Key-Value 数据库
- 速度快
- 常用于缓存、登录、验证码、排行榜

---

## 二、安装
### Docker
docker run -d --name redis -p 6379:6379 redis

---

## 三、数据类型
### String
- SET / GET
- 存 token、验证码

### Hash
- HSET / HGETALL
- 存用户对象

### List
- LPUSH / LRANGE
- 日志、消息队列

### Set
- SADD / SMEMBERS
- 去重集合

### ZSet
- ZADD / ZRANGE
- 排行榜

---

## 四、常见场景
- 用户信息缓存
- 登录 token
- 验证码
- 接口限流
- 排行榜

---

## 五、命令行操作
```bash
redis-cli -h 
```

一、增（Create）
```bash
# SET —— HSET写缓存
SET key value   

SET age 18 
```
- 
-      
- 
-  
- HSET key field value(可以存多个)
-  HSET user:1 name Tom
 
- 二、查（Read）
- GET—— HGET
- GET key
- GET age 
- 
- HGET key field
- HSET user:1
-  
- KEYS *  查看所有 key（调试用）

- 三、删（Delete）
- DEL key
- DEL key field

- 四、过期时间-EX
- SET key value EX seconds
- SET code:email:tom@qq.com 9527 EX 300
- 
- SET key field value
- HSET user:1 name zhashan
- 
- EXPIRE key field seconds
- EXPIRE user:1 60
- 
- 五、TTL —— 查看还剩多久
   TTL key
![alt text](image.png)
## 六、推荐使用方式
- Redis + MySQL
- Redis 作为缓存层
