---
{"created":"2024-12-03T14:37:00.272+08:00","tags":["RunasCS","windows提权","SeImpersonatePrivilege"],"Type":"Note","dg-publish":true,"aliases":["RunasCS"],"permalink":"/26-工具使用/RunasCS使用/","dgPassFrontmatter":true,"noteIcon":"2"}
---

## 1. 介绍
项目地址 https://github.com/antonioCoco/RunasCs
该工具允许我们当前用户用另外一个用户的权限执行命令，
**条件**
- 需要另一个用户的账号密码
- 当前用户需要有 `SeImpersonatePrivilege` 权限

## 2. 使用
```bash
.\runascs.exe <另一个用户> <另一个用户的密码> "cmd /c whoami /all"
```

