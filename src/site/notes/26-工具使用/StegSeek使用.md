---
{"created":"2024-11-22T10:46:10.452+08:00","tags":["文件处理","图像隐写"],"Type":"null","dg-publish":true,"aliases":["StegSeek"],"permalink":"/26-工具使用/StegSeek使用/","dgPassFrontmatter":true,"noteIcon":"2"}
---

## 1. 介绍
**StegSeek** 是一款用于 **图像隐写分析**（steganalysis）的工具，它能够自动检测并提取图像中的隐写信息
## 2. 常用命令
```bash
StegSeek a.jpg  提取图片中的隐写信息
--crack <字典>>使用一个字典破解一个stego文件
--seed 检测一个文件是否使用steghide进行编码
```

## 3. 参数
```bash
-sf, --stegofile 选择一个stego文件
-wl, --wordlist 选择一个字典文件
-xf, --extractfile 选择提取数据的文件名
-t, --threads 设置线程数量，默认为CPU核心数
-f, --force 覆盖现有文件
-v, --verbose 显示Verbose详细信息
-q, --quiet 隐藏性能计量（提升性能）
-s, --skipdefault 不添加额外的密码猜测
-n, --nocolor 禁用颜色高亮输出
-c, --continue 找到一个结果之后继续破解
-a, --accessible 提升输出输出结果的可读
```
