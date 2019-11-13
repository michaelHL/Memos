---
title: VBS 学习笔记一
date: 2019-04-24 13:52:14
tags: VBS
categories: Programming Language
---

- 如果 VBS 文件中含有中文，文件编码一定要改为 `GBK` 或 `GB2312`
- VBS 快捷方式特殊文件夹：
  <!-- more -->

  | Special             | Folder Usage                                          |
  | :---                | :---                                                  |
  | `AllUsersDesktop`   | Desktop shortcuts for all users                       |
  | `AllUsersPrograms`  | Programs menu options for all users                   |
  | `AllUsersStartMenu` | Start menu options for all users                      |
  | `AllUsersStartup`   | Startup applications for all users                    |
  | `Desktop`           | Desktop shortcuts for the current user                |
  | `Favorites`         | Favorites menu shortcuts for the current user         |
  | `Fonts`             | Fonts folder shortcuts for the current user           |
  | `MyDocuments`       | My Documents menu shortcuts for the current user      |
  | `NetHood`           | Network Neighborhood shortcuts for the current user   |
  | `Printers`          | Printers folder shortcuts for the current user        |
  | `Programs`          | Programs menu options for the current user            |
  | `Recent`            | Recently used document shortcuts for the current user |
  | `SendTo`            | SendTo menu shortcuts for the current user            |
  | `StartMenu`         | Start menu shortcuts for the current user             |
  | `Startup`           | Startup applications for the current user             |
  | `Templates`         | Templates folder shortcuts for the current user       |
- VBS 创建快捷方式示例：
  ```VB CreateShortcut.vbs
  Set ws = WScript.CreateObject("WScript.Shell")
  Set scut = ws.CreateShortcut("xx.lnk")
  scut.TargetPath = "path\to\file"
  scut.Arguments = "arg1 arg2"
  scut.Description = "description"
  scut.HotKey = "CTRL+ALT+Y"
  scut.IconLocation = "path\to\icon\file,0"
  scut.WindowStyle = 1
  scut.WorkingDirectory = "path\to\wd"
  scut.Save()
  ```
- 类似地，Python 也可实现类似的操作，但有一点需要注意，如果对目录创建快捷方式，其 `TargetPath` 属性中的路径分割符必须是 `\`，所以为安全起见，可对所有链接目标为文件 / 文件夹的字符串的路径分隔符统一使用 `os.path.abspath` 转换。如果是 `/`，生成的快捷方式的 `Target type` 为 `File` 而非 `File folder`，导致双击快捷方式没有反应。
  ```Python CreateShortcut.py
  import win32com.client
  import os.path
  shell = win32com.client.Dispatch('WScript.Shell')
  scut = shell.CreateShortcut('xx.lnk')
  scut.TargetPath = os.path.abspath('path/to/file')
  scut.Description = 'xx'
  scut.IconLocation = 'path/to/icon,0'
  scut.WindowStyle = 1
  scut.HotKey = 'CTRL+ALT+Y'
  scut.Save()
  ```
