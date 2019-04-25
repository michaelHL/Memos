---
title: Windows 杂项
date: 2019-04-24 10:09:17
tags: Miscellaneous
categories: Notes
---

1. Windows 运行（Run）小结：
   - Windows 运行对话框实际为 `shell32.dll` 的资源，可通过如下方式调用：
     ```
     c:\windows\system32\rundll32.exe shell32.dll,#61
     ```
   - Windows 运行除读取 `PATH` 中的路径外，还会查询 `App Path` 注册表键：
     ```
     HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\App Paths
     ```
   - 几个比较有用的命令：
     - `\`: 打开 `C:\` 盘
     - `.`: 相当于 `%USERPROFILE%`
     - `..`: 相当于 `\Users`
     - `control`: 控制面板
     - `sysdm.cpl`: 系统属性对话框
     - `compmgmt.msc`: 计算机管理
     - `msinfo32`: 系统信息
     - `cleanmgr`: 磁盘清理
     - `eventvrw.msc`: 事件查看器
     - `resmon`: 资源监视器
     - `mstsc`: 远程桌面连接
1. cmd 终端中可运行两种命令：内部命令、外部命令，部分列举如下：
   - **Internal Commands**: `break`、`call`、`chcp`、`chdir(cd)`、`cls`、`copy`、`ctty`、`date`、`del(erase)`、`dir`、`echo`、`exit`、`for`、`goto`、`if`、`mkdir(md)`、`path`、`pause`、`prompt`、`rem`、`rename(ren)`、`rmdir(rd)`、``set``、`shift`、`time`、`type`、`ver`、`verify`、`vol`
   - **External Commands**: `append`、`assign`、`attrib`、`backup`、`chkdsk`、`command`、`comp`、`debug`、`diskcomp`、`diskcopy`、`doskey`、`dosshell`、`edit`、`edlin`、`emm386`、`exe2bin`、`expand`、`fastopen`、`fc`、`fdisk`、`format`、`graftable`、`graphics`、`help`、`join`、`keyb`、`label`、`mem`、`mirror`、`mode`、`more`、`nlsfunc`、`print`、`qbasic`、`recover`、`replace`, `restore`、`setver`、`share`、`sort`、`subst`、`sys`、`tree`、`undelete`、`unformat`、`xcopy`
