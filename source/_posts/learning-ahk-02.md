---
title: AHK 基础（二）
date: 2019-07-05 11:05:59
tags: AHK
categories: Programming Language
---

## 字符函数

- 字符函数 `Asc()` 仅返回 `0 ~ 65536` 间的数字，而 `Ord()` 返回其 Unicode 码位，范围为 `0x0 ~ 0x10FFFF`


## 控件

### Button

#### 创建控件

```
Gui, Add, Button, Default w80, OK
```

示例中属性 `Default` 表示默认情况下（除了以下两种情况外：焦点在于其它按钮上、多行编辑控件具有 `WantReturn` 属性）回车会触发该按钮的点击事件，如需调整某按钮默认属性：

```
GuiControl, +Default, Cancel
GuiControl, -Default, OK
```

