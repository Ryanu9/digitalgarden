---
{"created":"2024-12-06T15:49:23.187+08:00","tags":["python","flask","SSTI"],"Type":"Note","dg-publish":true,"aliases":["SSTIMAP"],"permalink":"/26-工具使用/SSTIMAP使用/","dgPassFrontmatter":true,"noteIcon":"2"}
---

## 1. 介绍
项目地址：https://github.com/vladko312/SSTImap
SSTImap 是一款渗透测试软件，可以检查网站是否存在代码注入和服务器端模板注入漏洞并利用它们，从而提供对操作系统本身的访问权限

## 2. 使用教程

```python
-u 检测是否存在ssti模版注入
python sstimap.py -u http://192.168.212.4/?id=1
```

![Pasted image 20241206155355](https://yurain.oss-cn-chengdu.aliyuncs.com/Obsidian/Pasted%20image%2020241206155355.png)
### 2.1. 其他参数

| 选项                        | 功能描述                                         |
| :------------------------ | -------------------------------------------- |
| --os-shell                | 提供一个交互式操作系统 shell，通常在目标系统上执行操作系统命令。          |
| --os-cmd                  | 执行一个操作系统命令，并返回结果。                            |
| --eval-shel               | 提供一个交互式 shell，基于模板引擎的底层语言（例如 Python、Ruby 等）。 |
| --eval-cmd                | 在模板引擎底层语言中执行一段代码。                            |
| --tpl-shell               | 提供一个交互式 shell，直接在模板引擎环境中。                    |
| --tpl-cmd                 | 在模板引擎中注入并执行一段代码。                             |
| --bind-shell PORT         | 在目标主机的指定端口上绑定一个 shell，等待外部连接。                |
| --reverse-shell HOST PORT | 将一个 shell 反向连接到攻击者指定的主机和端口，用于远程访问目标。         |
| --upload LOCAL REMOTE     | <br>将本地文件上传到服务器上的指定路径。                       |
| --download REMOTE LOCAL   | 从服务器下载指定的文件到本地。                              |
