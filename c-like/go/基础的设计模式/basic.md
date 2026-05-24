

---

# 基础设计模式终结版笔记

## 一、设计模式是什么（一句话）

**设计模式是前人总结出来的“写代码的固定套路”，用来解决常见问题。**
不是语法，是经验。

---

## 二、单例模式（Singleton）

### 是什么

一个类在整个程序中 **只能有一个实例**。

### 解决什么问题

* 全局只需要一个对象
* 避免重复创建、浪费资源

### 常见场景

* 配置中心
* 数据库连接池
* 日志对象

### 核心思想

* 构造方法私有
* 对外只提供一个获取实例的方法

### 一句话记忆

**保证全局只有一个对象。**

### 面试一句话

单例模式通过限制对象创建次数，保证系统中只有一个实例，常用于全局共享资源。

---

## 三、工厂模式（Factory）

### 是什么

**把“创建对象”的代码封装起来，由工厂统一创建对象。**

### 解决什么问题

* 不想在业务代码里写大量 `new`
* 解耦“用什么”和“怎么创建”

### 常见场景

* 创建不同类型的对象
* 根据配置或参数决定创建哪种对象

### 核心思想

* 业务代码不关心具体类
* 工厂负责选择和创建

### 一句话记忆

**把 new 交给工厂。**

### 面试一句话

工厂模式通过统一的创建入口，隐藏对象创建细节，降低代码耦合度。

---

## 四、代理模式（Proxy）

### 是什么

**在不改原代码的情况下，对原对象的功能进行增强。**

### 解决什么问题

* 不方便直接改原对象
* 想在执行前后加点逻辑

### 常见场景

* 日志
* 权限校验
* 事务
* 远程调用

### 核心思想

* 代理对象和目标对象实现同一接口
* 代理负责增强，目标负责核心逻辑

### 一句话记忆

**不动原代码，外面包一层。**

### 面试一句话

代理模式通过代理对象控制对目标对象的访问，常用于功能增强和访问控制。

---

## 五、策略模式（Strategy）

### 是什么

**把不同的算法或行为封装成不同的策略，运行时自由切换。**

### 解决什么问题

* 大量 if / else 判断
* 不同规则可替换

### 常见场景

* 支付方式选择
* 不同折扣算法
* 不同排序规则

### 核心思想

* 定义统一接口
* 不同策略实现不同逻辑

### 一句话记忆

**把 if/else 变成可替换的策略。**

### 面试一句话

策略模式通过封装不同策略，实现行为的动态切换，避免复杂的条件判断。

---

## 六、模板模式（Template）

### 是什么

**把固定流程写好，把可变步骤留给子类实现。**

### 解决什么问题

* 多个流程大体相同
* 某些步骤不同

### 常见场景

* 请求处理流程
* 数据导入导出流程
* 算法骨架固定但步骤可变

### 核心思想

* 父类定义流程
* 子类只实现变化部分

### 一句话记忆

**流程固定，细节可变。**

### 面试一句话

模板模式通过抽象父类定义算法骨架，子类实现具体步骤，保证流程一致性。

---

## 七、五种模式对比速记表

| 模式 | 解决的问题            |
| -- | ---------------- |
| 单例 | 保证只有一个对象         |
| 工厂 | 统一创建对象           |
| 代理 | 不改原代码增强功能        |
| 策略 | 消除 if/else，可切换算法 |
| 模板 | 固定流程，步骤可变        |

---

## 八、面试总括一句话（高级）

在实际开发中，常通过单例管理全局资源，通过工厂解耦对象创建，通过代理实现功能增强，通过策略消除条件分支，通过模板模式复用流程结构。

---

如果你愿意，下一步我可以帮你：

* 把这五个模式用 **Java + Go 对照代码**写一遍
* 给你一套 **设计模式面试必问追问清单**
* 结合 **Spring / Gin / Go 项目**告诉你这些模式在哪用

你选一个，我继续给你整理成**能直接背的笔记版**。

好，下面是 **五种设计模式的 Java + Go 对照代码终结版**。
每个模式只给 **最小可理解实现**，你可以直接当笔记抄。

---

# 一、单例模式（Singleton）

## Java

```java
public class Singleton {
    private static final Singleton INSTANCE = new Singleton();

    private Singleton() {}

    public static Singleton getInstance() {
        return INSTANCE;
    }
}
```

使用：

```java
Singleton s = Singleton.getInstance();
```

---

## Go

```go
package main

import "sync"

type Singleton struct{}

var instance *Singleton
var once sync.Once

func GetInstance() *Singleton {
	once.Do(func() {
		instance = &Singleton{}
	})
	return instance
}
```

使用：

```go
s := GetInstance()
```

---

# 二、工厂模式（Factory）

## Java

```java
interface Animal {
    void speak();
}

class Dog implements Animal {
    public void speak() {
        System.out.println("wang");
    }
}

class Cat implements Animal {
    public void speak() {
        System.out.println("miao");
    }
}

class AnimalFactory {
    public static Animal create(String type) {
        if ("dog".equals(type)) return new Dog();
        if ("cat".equals(type)) return new Cat();
        return null;
    }
}
```

使用：

```java
Animal a = AnimalFactory.create("dog");
a.speak();
```

---

## Go

```go
package main

import "fmt"

type Animal interface {
	Speak()
}

type Dog struct{}
func (Dog) Speak() { fmt.Println("wang") }

type Cat struct{}
func (Cat) Speak() { fmt.Println("miao") }

func NewAnimal(t string) Animal {
	if t == "dog" {
		return Dog{}
	}
	if t == "cat" {
		return Cat{}
	}
	return nil
}
```

使用：

```go
a := NewAnimal("dog")
a.Speak()
```

---

# 三、代理模式（Proxy）

## Java

```java
interface Service {
    void work();
}

class RealService implements Service {
    public void work() {
        System.out.println("real work");
    }
}

class ServiceProxy implements Service {
    private Service target;

    public ServiceProxy(Service target) {
        this.target = target;
    }

    public void work() {
        System.out.println("before");
        target.work();
        System.out.println("after");
    }
}
```

使用：

```java
Service s = new ServiceProxy(new RealService());
s.work();
```

---

## Go

```go
package main

import "fmt"

type Service interface {
	Work()
}

type RealService struct{}
func (RealService) Work() {
	fmt.Println("real work")
}

type Proxy struct {
	target Service
}

func (p Proxy) Work() {
	fmt.Println("before")
	p.target.Work()
	fmt.Println("after")
}
```

使用：

```go
p := Proxy{target: RealService{}}
p.Work()
```

---

# 四、策略模式（Strategy）

## Java

```java
interface PayStrategy {
    void pay();
}

class AliPay implements PayStrategy {
    public void pay() {
        System.out.println("alipay");
    }
}

class WeChatPay implements PayStrategy {
    public void pay() {
        System.out.println("wechat pay");
    }
}

class PayContext {
    private PayStrategy strategy;

    public PayContext(PayStrategy strategy) {
        this.strategy = strategy;
    }

    public void pay() {
        strategy.pay();
    }
}
```

使用：

```java
PayContext ctx = new PayContext(new AliPay());
ctx.pay();
```

---

## Go

```go
package main

import "fmt"

type PayStrategy interface {
	Pay()
}

type AliPay struct{}
func (AliPay) Pay() { fmt.Println("alipay") }

type WeChatPay struct{}
func (WeChatPay) Pay() { fmt.Println("wechat pay") }

type Context struct {
	strategy PayStrategy
}

func (c Context) Pay() {
	c.strategy.Pay()
}
```

使用：

```go
c := Context{strategy: AliPay{}}
c.Pay()
```

---

# 五、模板模式（Template）

## Java

```java
abstract class Game {
    public final void play() {
        init();
        start();
        end();
    }

    protected abstract void init();
    protected abstract void start();
    protected abstract void end();
}

class Football extends Game {
    protected void init() { System.out.println("init football"); }
    protected void start() { System.out.println("start football"); }
    protected void end() { System.out.println("end football"); }
}
```

使用：

```java
Game g = new Football();
g.play();
```

---

## Go（用组合 + 方法模拟）

```go
package main

import "fmt"

type Game interface {
	Init()
	Start()
	End()
}

type Template struct {
	Game
}

func (t Template) Play() {
	t.Init()
	t.Start()
	t.End()
}

type Football struct{}

func (Football) Init()  { fmt.Println("init football") }
func (Football) Start() { fmt.Println("start football") }
func (Football) End()   { fmt.Println("end football") }
```

使用：

```go
t := Template{Game: Football{}}
t.Play()
```

---

# 六、终极记忆口诀（非常重要）

* 单例：全局只有一个
* 工厂：new 交给别人
* 代理：外面包一层
* 策略：if/else 变接口
* 模板：流程固定，步骤可变

---

如果你愿意，下一步我可以帮你：

* 把这五种模式 **放进 Spring / Gin 实战场景**
* 给你一套 **设计模式面试追问 + 回答模板**
* 画一张 **“看到代码就知道用的是什么模式”对照表**

你选一个，我继续。
