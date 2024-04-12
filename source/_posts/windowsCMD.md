---
title: Windows常用命令
date: 2024-04-12 20:36:14
tags:
- windows
---
1. 查看硬盘序列号：
```
wmic diskdrive get model,serialnumber
```
2. 查看设备序列号：
```
wmic bios get serialnumber（查不到可以去BIOS）
```
3. 查看IP和MAC地址：
```
ipconfig /all
```