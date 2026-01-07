
# GORM 知识总结
- [GORM 知识总结](#gorm-知识总结)
	- [1️⃣ 连接数据库（Connection）](#1️⃣-连接数据库connection)
	- [2️⃣ 模型定义（Model）](#2️⃣-模型定义model)
	- [3️⃣ 单表查询（CRUD 基础）](#3️⃣-单表查询crud-基础)
		- [查询](#查询)
		- [创建](#创建)
		- [更新](#更新)
		- [删除](#删除)
	- [4️⃣ 创建钩子（Hook）](#4️⃣-创建钩子hook)
	- [5️⃣ 高级查询（一）](#5️⃣-高级查询一)
		- [条件组合](#条件组合)
		- [IN 查询](#in-查询)
	- [6️⃣ 高级查询（二）](#6️⃣-高级查询二)
		- [排序 + 分页（重点）](#排序--分页重点)
	- [7️⃣ 一对多关系（has many）](#7️⃣-一对多关系has-many)
		- [模型](#模型)
		- [查询](#查询-1)
	- [8️⃣ 一对一关系（has one）](#8️⃣-一对一关系has-one)
	- [9️⃣ 多对多关系（many2many）](#9️⃣-多对多关系many2many)
	- [🔟 自定义数据类型](#-自定义数据类型)
		- [场景：JSON 字段](#场景json-字段)
	- [1️⃣1️⃣ 事务（Transaction，重点）](#1️⃣1️⃣-事务transaction重点)
	- [✅ 总结一句话（非常重要）](#-总结一句话非常重要)


> GORM 是 Go 里最常用的 ORM，用来 **操作数据库而不是手写 SQL**

---

## 1️⃣ 连接数据库（Connection）

```go
import (
	"gorm.io/driver/mysql"
	"gorm.io/gorm"
)

dsn := "user:pwd@tcp(127.0.0.1:3306)/test?charset=utf8mb4&parseTime=True&loc=Local"
db, err := gorm.Open(mysql.Open(dsn), &gorm.Config{})
```

**作用**

* 建立 Go ↔ MySQL 的连接
* `db` 是后续所有数据库操作的入口

📌 面试一句话：

> GORM 通过 `gorm.Open` 建立数据库连接，返回全局 `*gorm.DB`

---

## 2️⃣ 模型定义（Model）

```go
type User struct {
	ID    uint   `gorm:"primaryKey"`
	Name  string
	Email string `gorm:"unique"`
}
```

**模型 = 表结构映射**

| Go 字段 | 数据库列 |
| ----- | ---- |
| ID    | id   |
| Name  | name |

📌 常用 tag：

```go
gorm:"primaryKey"
gorm:"unique"
gorm:"size:100"
```

---

## 3️⃣ 单表查询（CRUD 基础）

### 查询

```go
db.First(&user)              // 查第一条
db.First(&user, 1)           // 根据 id
db.Where("email = ?", e).First(&user)
```

### 创建

```go
db.Create(&user)
```

### 更新

```go
db.Model(&user).Update("name", "Tom")
db.Model(&user).Updates(User{Name: "Tom"})
```

### 删除

```go
db.Delete(&User{}, id)
```

📌 面试一句话：

> GORM 提供链式 API 完成 CRUD，避免手写 SQL

---

## 4️⃣ 创建钩子（Hook）

```go
func (u *User) BeforeCreate(tx *gorm.DB) error {
	u.Name = "default"
	return nil
}
```

**作用**

* 在 `Create / Update / Delete` 前后自动执行逻辑
* 常用于：密码加密、默认值、日志

📌 常见钩子：

```go
BeforeCreate
AfterCreate
BeforeUpdate
AfterDelete
```

---

## 5️⃣ 高级查询（一）

### 条件组合

```go
db.Where("age > ?", 18).
   Where("status = ?", 1).
   Find(&users)
```

### IN 查询

```go
db.Where("id IN ?", []int{1,2,3}).Find(&users)
```

---

## 6️⃣ 高级查询（二）

### 排序 + 分页（重点）

```go
db.Order("id desc").
   Offset((page-1)*size).
   Limit(size).
   Find(&users)
```

📌 这是 **分页标准写法（面试必会）**

---

## 7️⃣ 一对多关系（has many）

### 模型

```go
type User struct {
	ID     uint
	Orders []Order
}

type Order struct {
	ID     uint
	UserID uint
}
```

### 查询

```go
db.Preload("Orders").First(&user)
```

📌 含义：

> 一个用户 → 多个订单

---

## 8️⃣ 一对一关系（has one）

```go
type User struct {
	ID      uint
	Profile Profile
}

type Profile struct {
	ID     uint
	UserID uint
}
```

```go
db.Preload("Profile").First(&user)
```

📌 含义：

> 一个用户 → 一个详情表

---

## 9️⃣ 多对多关系（many2many）

```go
type User struct {
	ID    uint
	Roles []Role `gorm:"many2many:user_roles"`
}

type Role struct {
	ID   uint
	Name string
}
```

```go
db.Preload("Roles").First(&user)
```

📌 会自动生成中间表：`user_roles`

---

## 🔟 自定义数据类型

### 场景：JSON 字段

```go
type Info map[string]interface{}

func (i Info) Value() (driver.Value, error) {
	return json.Marshal(i)
}

func (i *Info) Scan(v interface{}) error {
	return json.Unmarshal(v.([]byte), i)
}
```

```go
type User struct {
	ID   uint
	Meta Info
}
```

📌 用于：

* JSON 字段
* 特殊存储格式

---

## 1️⃣1️⃣ 事务（Transaction，重点）

```go
err := db.Transaction(func(tx *gorm.DB) error {
	if err := tx.Create(&user).Error; err != nil {
		return err
	}

	if err := tx.Create(&order).Error; err != nil {
		return err
	}

	return nil
})
```

**规则**

* 返回 `error` → 回滚
* 返回 `nil` → 提交

📌 面试一句话：

> GORM 通过 `Transaction` 保证多步操作的原子性

---

## ✅ 总结一句话（非常重要）

> GORM 通过 Model 映射表结构，
> 使用链式 API 完成 CRUD，
> 支持关联关系、事务、钩子，
> 是 Go 后端最主流 ORM。

---

如果你愿意，下一步我可以帮你做三件很实用的事之一：

1️⃣ 帮你 **把这些整理成「面试问答版」**
2️⃣ 用你现在的 **go-shop 项目逐个对应这些点**
3️⃣ 单独把 **关联查询 / 事务 / Preload** 再深挖一轮

你选一个，我继续。
