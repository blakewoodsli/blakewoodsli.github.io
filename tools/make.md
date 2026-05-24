
# 一、Make 是什么？

> **Make 是一个“自动执行命令的工具”，根据规则决定“哪些命令该不该执行”。**

 核心关键词：
**自动化 / 依赖关系 / 少打命令**

---

##  Make 是干嘛用的？

你不用 Make 时：

```bash
go build
go test
go run main.go
```

你用 Make 后：

```bash
make build
make test
make run
```

 **把一堆命令，变成一个命令**

---

## 2️⃣ Make 的核心文件：Makefile

Make 只认一个文件：**Makefile**

最小示例：

```makefile
build:
	go build -o app

run:
	./app
```

执行：

```bash
make build
make run
```

---

##  Make 的核心概念（面试重点）

### ① target（目标）

```makefile
build:
```

* 你要执行的“事情”
* 对应：`make build`

---

### ② command（命令）

```makefile
	go build -o app
```

 **必须是 Tab，不是空格（新手必踩坑）**

---

### ③ dependency（依赖）

```makefile
run: build
	./app
```

含义：

> run 之前，必须先执行 build

执行：

```bash
make run
```

实际发生：

```
go build
./app
```
**这就是 Make 的灵魂：依赖关系**

---

##  Make 在真实项目中干嘛？（非常重要）

### Go 项目

```makefile
build:
	go build -o server

test:
	go test ./...

run:
	go run main.go
```

### C / C++ 项目（经典）

* 自动判断哪些 `.c` 变了
* 只重新编译必要的文件

 **Make 最早就是为 C 诞生的**

---

## 总结

> Make 是一个构建工具，通过 Makefile 定义目标和依赖关系，实现编译、测试、运行等流程的自动化。

---

