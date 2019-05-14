---
title:     Release“无法找到“xxx.exe”的调试信息……“解决方案
date:       2019-05-14
categories:
- 编程
tags:
- Fortran
---

#### 问题描述

VS2012环境中调试FORTRAN项目，debug模式下一切正常，调整为release模式则出现“无法找到“xxx.exe”的调试信息，或者调试信息不匹配。未使用调试信息生成二进制文件。”



#### 解决方案

首先打开菜单 项目->项目属性页 
1。选择 配置属性->链接器->调试->生成调试信息 改为 是 
2。选择 配置属性->Fortran ->常规->调试信息格式 改为 Line Numbers Only或者Full
3。选择 配置属性->Fortran->优化->优化 改为 禁用(/Od) 