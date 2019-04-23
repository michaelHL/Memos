---
title: Haskell 学习笔记一
date: 2019-01-28 23:11:54
tags:
---

1. 空字符串 `""` 与空列表 `[]` 同义：
   {% codeblock lang:Haskell%}
   "" == []
   -- True
   {% endcodeblock %}
1. 常用 Ghci 命令：
   - `:m +Package` 载入包 Package
   - `:set prompt xx` 修改提示符
   - `:[un]set +t` 控制表达式返回结果时是否显示结果类型
1. `x:xs` 只能匹配长度大于等于 1 的 List
1. `Num` 不是 `Ord` 的子集，数字不一定得拘泥于排序
1. 三种乘幂的区别：[Exponentiation in Haskell][stack-overflow-6400568]
1. 将 Haskell 代码转为 point-free 风格：[Pointfree.io][pointfree-io]



[stack-overflow-6400568]: https://stackoverflow.com/questions/6400568/exponentiation-in-haskell
[pointfree-io]: http://pointfree.io
