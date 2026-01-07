# 包管理工具

## Window

### 参考

- [2024年，提升Windows开发和使用体验的一些实践 - 包管理器篇
](https://www.cnblogs.com/deali/p/18022172)

### 升级 powershell

```bash
# 搜索是否存在 powershell 相关版本
winget search Microsoft.PowerShell

# 名称               ID                           版本    源
# ---------------------------------------------------------------
# PowerShell         Microsoft.PowerShell         7.5.4.0 winget
# PowerShell Preview Microsoft.PowerShell.Preview 7.6.0.6 winget

# 安装最新版本
winget install --id Microsoft.Powershell --source winget

# 已找到 PowerShell [Microsoft.PowerShell] 版本 7.5.4.0
# 此应用程序由其所有者授权给你。
# Microsoft 对第三方程序包概不负责，也不向第三方程序包授予任何许可证。
# 正在下载 https://github.com/PowerShell/PowerShell/releases/download/v7.5.4/PowerShell-7.5.4-win-x64.msi
#   ██████████████████████████████   107 MB /  107 MB
# 已成功验证安装程序哈希
# 正在启动程序包安装...
```

### scoop

安装 scoop

```bash
# 管理员模式打开 PowerShell 执行以下命令即可安装
# PowerShell terminal (版本需要大于 5.1 or later)
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
Invoke-RestMethod -Uri https://get.scoop.sh | Invoke-Expression

```

配置代理

```bash
scoop config proxy 127.0.0.1:55606
```

## MacOS 系统

MacOS 主要的包管理工具只要是 `brew`

## Ubuntu 系统（）

## Redis 系统（Linux）
