# Linux 常用命令


## 文件 & 目录（每天都在用 ⭐⭐⭐⭐⭐）

```bash
ls        # 查看目录
ls -l     # 详细列表
ls -a     # 包含隐藏文件
cd        # 切换目录
pwd       # 当前路径
mkdir     # 创建目录
rmdir     # 删除空目录
cp        # 复制
mv        # 移动 / 重命名
rm        # 删除
```

```bash
# -r 递归删除，循环遍历目录所有文件和目录
# -f == force 强制删除，无需确认
rm -rf /data/logs   # 强制递归删除（⚠️ 危险）
```


## 文件内容查看（排错必备 ⭐⭐⭐⭐⭐）

```bash
#  error.log 需要自己创建，是文件名称

cat error.log        # 一次性看完（小文件）
less error.log       # 翻页查看（推荐）
more error.log

head -n 20 error.log     # 前 20 行
tail -n 20 error.log     # 后 20 行

# 服务异常你怎么查？通过实时查看日志文件
tail -f error.log    # 实时看日志 ⭐
```

### 面试场景

>

👉 标准回答：

```bash
tail -f error.log
```

---

# 三、搜索 & 查找（非常重要 ⭐⭐⭐⭐）

## 1️⃣ 查文件

```bash
find / -name redis.conf
find . -type f -name "*.log"
```

## 2️⃣ 查内容（面试必问）

```bash
grep "ERROR" app.log
grep -n "panic" app.log
grep -r "userId" .
```

👉 面试官爱问：

> **日志里查关键字用什么？**

标准答案：**grep**

---

# 四、权限（新手必翻车 ⭐⭐⭐⭐）

## 查看权限

```bash
ls -l
```

```
-rw-r--r-- 1 root root app.log
```

## 修改权限

```bash
chmod 755 app.sh
chmod +x run.sh
```

## 用户切换

```bash
whoami
su
sudo
```

👉 面试点：

> Linux 权限由 **读(r) / 写(w) / 执行(x)** 组成

---

# 五、进程管理（后端面试高频 ⭐⭐⭐⭐⭐）

```bash
ps aux
top
htop
```

## 查进程

```bash
ps aux | grep go-shop
```

## 杀进程

```bash
kill PID
kill -9 PID   # 强制
```

👉 面试常问：

> 服务卡死了你怎么办？

👉 回答流程：

```bash
ps aux | grep 服务名
kill PID
```

---

# 六、端口 & 网络（⭐⭐⭐⭐⭐）

```bash
netstat -tunlp
ss -lntp
```

## 常用

```bash
netstat -tunlp | grep 8080
```

👉 面试问：

> 怎么查端口被谁占用了？

标准答案：**netstat / ss**

---

# 七、磁盘 & 内存（⭐⭐⭐⭐）

```bash
df -h      # 磁盘使用
du -sh *   # 当前目录大小
free -h    # 内存
```

👉 面试点：

> 服务器磁盘满了？

```bash
df -h
du -sh /data/*
```

---

# 八、压缩 & 解压（部署必用 ⭐⭐⭐）

```bash
tar -czvf app.tar.gz app/
tar -xzvf app.tar.gz
zip -r app.zip app/
unzip app.zip
```

---

# 九、软件管理（Ubuntu & RedHat）

## Ubuntu

```bash
apt update
apt install nginx
apt remove redis
```

## RedHat

```bash
yum install redis
dnf install redis
```

👉 面试问：

> Ubuntu 用什么包管理？

答：**apt**

---

# 十、服务管理（systemd ⭐⭐⭐⭐⭐）

```bash
systemctl start nginx
systemctl stop nginx
systemctl restart nginx
systemctl status nginx
systemctl enable nginx
```

👉 面试必考：

> Linux 如何管理后台服务？

标准答案：**systemctl**

---

# 十一、环境变量（Go / Java 必会 ⭐⭐⭐⭐）

```bash
echo $PATH
export GOPATH=/data/go
source ~/.bashrc
```

---

# 十二、管道 & 重定向（加分项 ⭐⭐⭐⭐）

```bash
|    # 管道
>    # 覆盖输出
>>   # 追加输出
```

```bash
ps aux | grep redis
echo "hello" > a.txt
echo "world" >> a.txt
```

---

# 十三、面试 20 条速背清单（🔥）

你至少要**不犹豫**说出这些👇

```
ls cd pwd
cp mv rm
cat less tail -f
grep find
chmod chown
ps aux top
kill kill -9
netstat ss
df du free
tar unzip
apt yum
systemctl
```

---

# 十四、给你一个「真实面试题」示例

> ❓ 服务访问不了，你怎么排查？

👉 标准回答（按步骤）：

1. `ps aux | grep 服务`
2. `netstat -tunlp | grep 端口`
3. `tail -f 日志`
4. `df -h`
5. `free -h`

---

# 下一步我建议你这样学（非常重要）

你现在已经到这个阶段了👇
我建议你继续 **“实战型 Linux”**

我可以直接带你做：
1️⃣ **Ubuntu 上部署 Go 项目（完整流程）**
2️⃣ **Go + systemd 后台服务**
3️⃣ **Nginx + 反向代理 + 日志排错**

你选一个，我直接按「跟着敲」模式教你。
