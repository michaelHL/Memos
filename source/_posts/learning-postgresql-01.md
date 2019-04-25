---
title: PostgreSQL 学习笔记一
date: 2019-01-23 10:17:21
tags:
   - SQL
   - PostgreSQL
categories: Database
---

## 安装 PostgreSQL

这里介绍非安装版 PostgreSQL 的配置。

1. 从 [PostgreSQL 发行版压缩包下载页面][postgresql-official] 下载
1. 解压至目录 `POSTGRESQL_ROOT` （自定），初始化数据库：
   {% codeblock lang:Dos%}
   initdb -U postgres -A password -E utf8 --locale=en-US -W -D POSTGRESQL_ROOT\data
   {% endcodeblock %}
   其中各参数含义如下：
   - `-U postgres` 设定超级用户名为 `postgres`
   - `-A password` 开启验证密码
   - `-E utf8` 数据库默认编码为 `UTF-8`
   - `-W` 手动输入超级用户密码
   - `-D POSTGRESQL_ROOT` 指定数据目录为 `POSTGRESQL_ROOT`
   - `--locale=locale / --no-locale` 指定区域 / 不指定区域
1. 创建 Windows 服务（管理员身份运行）
   {% codeblock %}
   POSTGRESQL_ROOT\bin\pg_ctl.exe register -N "postgresql" -U "NT AUTHORITY\NetworkService" -D "POSTGRESQL_ROOT\data" -w
   {% endcodeblock %}

[postgresql-official]: https://www.enterprisedb.com/download-postgresql-binaries

