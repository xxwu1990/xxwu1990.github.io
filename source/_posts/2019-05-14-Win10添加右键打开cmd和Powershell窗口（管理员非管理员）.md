---
title: Win10添加右键打开cmd和Powershell窗口（管理员/非管理员）
date: 2019-05-14
categories:
- WINDOWS
tags:
- 命令行
---

### 1.背景

Win10打开命令行窗口的方法有很多，常见的有

① win+R输入cmd；

② win+X选择命令提示符；

③ 右键开始菜单选择命令提示符。

其中②③均有管理员/非管理员，并且可以在 设置 → 个性化 → 任务栏 中改成Powershell。

但有时候需要在指定的文件夹打开命令行窗口或者Powershell，还需要再进行cd操作，比较麻烦，所有现在添加右键命令：在此处打开命令行（或Powershell）窗口，管理员和非管理员方式。

### 2.添加方法

#### 2.1 方法一：直接操作注册表手动添加

通过注册表进行添加，可以直接win+R，输入regedit打开注册表，定位到以下路径（可以直接复制粘贴到注册表编辑器上面的地址栏）：

````
HKEY_CLASSES_ROOT\Directory\Background\shell\
````

**！！！**

**注意：在进行进一步操作前请务必备份注册表，以免出现问题，可以进行还原。选择 文件-导出 ，全部备份文件较大，可以选择仅备份上面路径的分支。**

**！！！**

具体可以参考：

[Win10设置右键以管理员方式打开cmd](https://blog.csdn.net/hiudawn/article/details/80701935)

[如何设置在右键菜单中打开](https://blog.csdn.net/tp7309/article/details/53449792)

 

#### 2.2方法二：通过编写.reg文件进行添加（推荐）

和上面其实本质上一样，只是通过代码进行，更方便。

参考：

[Win10 Shift 右键打开命令行窗口（管理员/非管理员身份）](https://blog.csdn.net/huanghenghua/article/details/80199673)

[编写注册表reg文件及批处理操作注册表](https://blog.csdn.net/tp7309/article/details/53449792)

具体方法如下：

##### 2.2.1 实现效果

- 右键：

>在此处打开命令行窗口
在此处打开命令行窗口(管理员)

- shift + 右键：

>在此处打开 Powershell 窗口
在此处打开 Powershell 窗口(管理员)

其中shift + 右键 实现“在此处打开 Powershell 窗口”为系统自带，不需要添加

下面代码里的3和4均是通过隐藏的PowerShell窗口来调起powershell(cmd)的，因此会闪过一次powershell窗口。打开的时候会有用户账户控制弹窗，以确认管理员身份，

##### 2.2.2 具体代码

代码如下，Windows直接新建txt，粘贴进去保存，然后选择另存为，保存类型选所有文件、编码选ANSI、文件名为CmdAndPowershellAll.reg(名字无所谓，后缀为.reg就可以)。双击打开，会进行两次确认，然后会提示“已成功添加到注册表中”，这样就成功了！现在可以右键、shift+右键尝试一下了！

```shell
Windows Registry Editor Version 5.00
 
; 原文链接：
; https://blog.csdn.net/cxrsdn/article/details/84538767
 
; 若原先有，先删除原来的
[-HKEY_CLASSES_ROOT\Directory\Background\shell\OpenCmdHere]
[-HKEY_CLASSES_ROOT\Directory\Background\shell\runas]
[-HKEY_CLASSES_ROOT\Directory\Background\shell\PowershellAdmin]
 
; 1.右键：命令行
[HKEY_CLASSES_ROOT\Directory\Background\shell\OpenCmdHere]
@="在此处打开命令行窗口"
 
[HKEY_CLASSES_ROOT\Directory\Background\shell\OpenCmdHere\command]
@="cmd.exe -noexit -command Set-Location -literalPath \"%V\"" 
 
; 2.右键：命令行（管理员）
[HKEY_CLASSES_ROOT\Directory\Background\shell\runas]
@="在此处打开命令行窗口(管理员)"
"ShowBasedOnVelocityId"=dword:00639bc8
 
[HKEY_CLASSES_ROOT\Directory\Background\shell\runas\command]
@="cmd.exe /s /k pushd \"%V\""
 
; 3.shift+右键：Powershell(管理员)
[HKEY_CLASSES_ROOT\Directory\Background\shell\PowershellAdmin]
@="在此处打开 Powershell 窗口(管理员)"
"Extended"=""
 
[HKEY_CLASSES_ROOT\Directory\Background\shell\PowershellAdmin\command]
@="\"C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe\" -windowstyle hidden -Command $stpath = pwd; Start-Process PowerShell -ArgumentList \\\"-NoExit\\\", \\\"-Command Set-Location -literalPath '%V'\\\" -verb RunAs"
 
; 4.设置右键 管理员打开cmd的另一种方法（可用来替换上面的2）
; 通过Powershell调起，会闪过一次Powershell的窗口，去掉下面几行的[; ]可以取消注释
; [-HKEY_CLASSES_ROOT\Directory\Background\shell\OpenCmdHereAdmin]
; 
; [HKEY_CLASSES_ROOT\Directory\Background\shell\OpenCmdHereAdmin]
; @="在此处打开命令行窗口(管理员)"
; 
; [HKEY_CLASSES_ROOT\Directory\Background\shell\OpenCmdHereAdmin\command]
; @="PowerShell -windowstyle hidden -Command \"Start-Process cmd.exe -ArgumentList '/s,/k, pushd,%V' -Verb RunAs\""
```

说明：

1.前面有分号;的是注释；
2.带有"Extended"=""的是shift+右键的，可以自行调整四个命令是否加这个；
3.可以参考这个链接：Win10 Shift 右键打开命令行窗口（管理员/非管理员身份）进行理解
4.cmd管理员有两种方法，一个是2的runas，一个是4的powershell调起，4（已注释掉）会闪过powershell窗口，所以没有采用。

---------------------
作者：cxrsdn 
来源：CSDN 
原文：https://blog.csdn.net/cxrsdn/article/details/84538767 
版权声明：本文为博主原创文章，转载请附上博文链接