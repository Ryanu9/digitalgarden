---
{"created":"2024-12-01T14:01:53.509+08:00","tags":["隧道代理","端口转发"],"Type":"Note","dg-publish":true,"aliases":["chisel"],"permalink":"/26-工具使用/chisel 使用/","dgPassFrontmatter":true,"noteIcon":"2"}
---

项目地址：https://github.com/jpillora/chisel
chisel 是一个go语言开发的隧道工具
## 1. 基本用法
### 1.1. 正向连接
```shell
./chisel server -p 1234
./chisel client 192.168.1.1:1234 1111:127.0.0.1:8000
```

>  将**服务端**的 `127.0.0.1:8000` 转发到**客户端**的 `1111`

### 1.2. 反向连接
```shell
./chisel server -p 1234 --reverse
./chisel client 192.168.1.1:1234 R:1111:127.0.0.1:8000
```
> 将**服务端**的 `1111` 端口 转发到**客户端**的 `127.0.0.1:8000`


![](/img/user/26-工具使用/assets/Pasted image 20241201144704.png)