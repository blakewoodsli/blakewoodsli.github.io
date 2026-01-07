

# MySQL 基础总结
- [MySQL 基础总结](#mysql-基础总结)
  - [MySQL 是什么](#mysql-是什么)
  - [安装 MySQL](#安装-mysql)
  - [ref](#ref)
    - [使用 Docker（推荐）](#使用-docker推荐)
  - [MySQL 基本概念](#mysql-基本概念)
    - [1️⃣ 数据库（Database）](#1️⃣-数据库database)
    - [2️⃣ 表（Table）](#2️⃣-表table)
    - [3️⃣ 行 \& 列](#3️⃣-行--列)
  - [MySQL 常见数据类型](#mysql-常见数据类型)
    - [1️⃣ 数值类型](#1️⃣-数值类型)
    - [2️⃣ 字符串类型](#2️⃣-字符串类型)
    - [3️⃣ 时间类型](#3️⃣-时间类型)
  - [CRUD（核心）](#crud核心)
  - [一、准备表（create）](#一准备表create)
    - [1️⃣ 建表（标准示例）](#1️⃣-建表标准示例)
    - [查看表](#查看表)
  - [二、增（create / insert）](#二增create--insert)
    - [2️⃣ 插入一条数据](#2️⃣-插入一条数据)
    - [3️⃣ 插入多条数据](#3️⃣-插入多条数据)
  - [三、查（read / select）](#三查read--select)
    - [4️⃣ 查询全部](#4️⃣-查询全部)
    - [5️⃣ 查询指定字段](#5️⃣-查询指定字段)
    - [6️⃣ 条件查询（where）](#6️⃣-条件查询where)
    - [7️⃣ and / or](#7️⃣-and--or)
    - [8️⃣ 排序（order by）](#8️⃣-排序order-by)
    - [9️⃣ 分页（limit）](#9️⃣-分页limit)
  - [四、改（update）](#四改update)
    - [🔟 更新一条数据（一定要 where）](#-更新一条数据一定要-where)
    - [更新多个字段](#更新多个字段)
  - [五、删（delete）](#五删delete)
    - [1️⃣ 删除一条](#1️⃣-删除一条)
    - [2️⃣ 删除所有（慎用）](#2️⃣-删除所有慎用)
  - [六、进阶（非常常用）](#六进阶非常常用)
    - [3️⃣ 统计数量（count）](#3️⃣-统计数量count)
    - [4️⃣ 去重（distinct）](#4️⃣-去重distinct)
    - [5️⃣ 模糊查询（like）](#5️⃣-模糊查询like)
  - [常见约束（非常重要）](#常见约束非常重要)
    - [1️⃣ 主键（PRIMARY KEY）](#1️⃣-主键primary-key)
    - [2️⃣ 非空（NOT NULL）](#2️⃣-非空not-null)
    - [3️⃣ 唯一（UNIQUE）](#3️⃣-唯一unique)
  - [索引（重点）](#索引重点)
    - [什么是索引](#什么是索引)
    - [创建索引](#创建索引)
    - [什么时候建索引](#什么时候建索引)
  - [事务（Transaction）](#事务transaction)
    - [事务四大特性（ACID）](#事务四大特性acid)
    - [事务示例](#事务示例)
  - [MySQL 常见使用场景](#mysql-常见使用场景)
  - [MySQL + Redis（重点）](#mysql--redis重点)
    - [为什么一起用？](#为什么一起用)
    - [标准架构（你后端必会）](#标准架构你后端必会)
  - [MySQL 命令行操作](#mysql-命令行操作)
    - [登录](#登录)
  - [](#)
    - [查看数据库](#查看数据库)
    - [查看表](#查看表-1)
    - [查看表结构](#查看表结构)

## MySQL 是什么

**概念说明：**

* 基于 **磁盘** 的 **关系型数据库（RDBMS）**
* 使用 **SQL（结构化查询语言）**
* 支持 **事务、索引、外键**
* 数据以 **表（Table）** 的形式存储

**常见用途：**

* 用户系统
* 订单系统
* 商品数据
* 业务核心数据（最终可信数据）

---

## 安装 MySQL

## ref

* [MySQL 官方下载](https://dev.mysql.com/downloads/)
* [Docker 安装 MySQL（推荐）](https://hub.docker.com/_/mysql)

---

### 使用 Docker（推荐）

```bash
docker run -d \
  --name mysql \
  -p 3306:3306 \
  -e MYSQL_ROOT_PASSWORD=123456 \
  mysql:8.0
```

**说明：**

* `-p 3306:3306`：端口映射
* `MYSQL_ROOT_PASSWORD`：root 密码
* 默认字符集建议后续设置为 `utf8mb4`

---

## MySQL 基本概念

### 1️⃣ 数据库（Database）

* 数据库是 **表的集合**
* 一个项目一般对应一个数据库

```sql
CREATE DATABASE test_db;
USE test_db;
```
![alt text](image.png)

---

### 2️⃣ 表（Table）

* 表 = 行（Row）+ 列（Column）
* 类似 Excel

```sql
CREATE TABLE user (
  id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(50),
  age INT
);
```

![alt text](image-2.png)

---

### 3️⃣ 行 & 列

* 一行：一条数据
* 一列：一个字段

---

## MySQL 常见数据类型

### 1️⃣ 数值类型

| 类型      | 说明     |
| ------- | ------ |
| INT     | 整数     |
| BIGINT  | 长整型    |
| DECIMAL | 金额（推荐） |

---

### 2️⃣ 字符串类型

| 类型      | 说明         |
| ------- | ---------- |
| VARCHAR | 变长字符串（最常用） |
| CHAR    | 定长字符串      |
| TEXT    | 大文本        |

---

### 3️⃣ 时间类型

| 类型        | 说明   |
| --------- | ---- |
| DATETIME  | 日期时间 |
| TIMESTAMP | 时间戳  |

---

## CRUD（核心）

## 一、准备表（create）

### 1️⃣ 建表（标准示例）

```sql
create table users (
  id int primary key auto_increment,
  name varchar(50) not null,
  age int
);
```

### 查看表

```sql
show tables;
desc users;
```

---

## 二、增（create / insert）

### 2️⃣ 插入一条数据

```sql
insert into users (name, age)
values ('tom', 18);
```
![alt text](image-3.png)

### 3️⃣ 插入多条数据

```sql
insert into users (name, age)
values
  ('jack', 20),
  ('lucy', 22),
  ('bob', 17);
```

---

## 三、查（read / select）

### 4️⃣ 查询全部

```sql
select * from users;
```

---

### 5️⃣ 查询指定字段

```sql
select name, age from users;
```

---

### 6️⃣ 条件查询（where）

```sql
select * from users where age > 18;
```

![alt text](image-4.png)

---

### 7️⃣ and / or

```sql
select * from users
where age > 18 and name = 'jack';
```

---

### 8️⃣ 排序（order by）

```sql
select * from users
order by age desc;
```

* `desc`：降序
* `asc`：升序（默认）

---

### 9️⃣ 分页（limit）

```sql
select * from users
limit 0, 2;
```

* 第一个参数：起始位置
* 第二个参数：条数

---

## 四、改（update）

### 🔟 更新一条数据（一定要 where）

```sql
update users
set age = 21
where name = 'jack';
```
![alt text](image-5.png)

⚠️ **没有 where = 全表更新（事故）**

---

### 更新多个字段

```sql
update users
set name = 'jacky', age = 22
where id = 2;
```

---

## 五、删（delete）

### 1️⃣ 删除一条

```sql
delete from users
where id = 4;
```

![alt text](image-6.png)

---

### 2️⃣ 删除所有（慎用）

```sql
delete from users;
```

---

## 六、进阶（非常常用）

### 3️⃣ 统计数量（count）

```sql
select count(*) from users;
```

---

### 4️⃣ 去重（distinct）

```sql
select distinct age from users;
```

---

### 5️⃣ 模糊查询（like）

```sql
select * from users
where name like 'j%';
```

* `j%`：以 j 开头

---

## 常见约束（非常重要）

### 1️⃣ 主键（PRIMARY KEY）

```sql
id INT PRIMARY KEY AUTO_INCREMENT
```

* 唯一
* 不为空

---

### 2️⃣ 非空（NOT NULL）

```sql
name VARCHAR(50) NOT NULL
```

---

### 3️⃣ 唯一（UNIQUE）

```sql
email VARCHAR(100) UNIQUE
```

---

## 索引（重点）

### 什么是索引

* 类似 **书的目录**
* 加快查询速度
* 用空间换时间

---

### 创建索引

```sql
CREATE INDEX idx_user_name ON user(name);
```

---

### 什么时候建索引

✅

* WHERE 条件
* JOIN 字段
* 经常查询的字段

❌

* 数据量小
* 经常更新的字段

---

## 事务（Transaction）

### 事务四大特性（ACID）

* **A** 原子性
* **C** 一致性
* **I** 隔离性
* **D** 持久性

---

### 事务示例

```sql
START TRANSACTION;

UPDATE account SET money = money - 100 WHERE id = 1;
UPDATE account SET money = money + 100 WHERE id = 2;

COMMIT;
-- ROLLBACK;
```

---

## MySQL 常见使用场景

* 用户表（user）
* 订单表（order）
* 商品表（product）
* 日志数据（持久化）

---

## MySQL + Redis（重点）

### 为什么一起用？

| MySQL | Redis |
| ----- | ----- |
| 持久化   | 高速    |
| 慢     | 快     |
| 可靠    | 缓存    |

---

### 标准架构（你后端必会）

```text
请求 → Redis（命中直接返回）
        ↓
      MySQL → 写回 Redis
```

---

## MySQL 命令行操作

### 登录

```bash
mysql -uroot -p
```
![alt text](image-1.png)
---

### 查看数据库

```sql
SHOW DATABASES;
```

---

### 查看表

```sql
SHOW TABLES;
```

---

### 查看表结构

```sql
DESC user;
```

