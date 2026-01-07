# docker

## 参考资料

## docker 常用命令

## 常见问题

### 为什么「不同架构」需要「不同镜像」？

CPU 指令集不一样，编译出来的二进制程序 (可执行文件) 不能通用

```bash
# docker hub 版本列表看到的 OS/ARCH
OS/ARCH
linux/amd64
linux/arm64/v8
linux/386
```

| 层级 | 含义                               |
|------|------------------------------------|
| OS   | 操作系统（linux / windows）        |
| ARCH | CPU 架构（amd64 / arm64 / 386 等） |

例子：

- linux/amd64
👉 给 Intel / AMD 64 位 CPU 用的
- linux/arm64/v8
👉 给 ARM 架构 CPU（Apple M 芯片、云服务器、树莓派新款）
- linux/386
👉 老的 32 位 x86

💥 同一个 Nginx，在不同 CPU 上

- 编译出来的可执行文件 完全不同
- 指令不兼容，不能直接运行

### Docker pull 时发生了什么？

Docker 会：

- 检测你的运行环境
- 自动匹配最合适的 OS/ARCH
- 拉对应的 digest（你看到的那一行）

👉 你不用手动选，Docker 自动帮你选
