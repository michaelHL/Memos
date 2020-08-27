---
title: xlwings 不能正确处理 Excel 文件所在路径中包含中文的情况
date: 2020-05-12 21:55:16
tags:
---

测试环境：

- Windows 10 x64 英文版
- Excel 2019
- Python 3.7.5 (Conda 4.6.8)
- xlwings 0.19.2

问题复现：利用 `xlwings quickstart` 创建三个测试目录：`en`、`中文`、`cn`，并把 `cn` 目录下的文件名改为中文，目录如下：

```
    .
    ├─ cn
    │   ├─ 中文.py
    │   └─ 中文.xlsm
    ├─ en
    │   ├─ en.py
    │   └─ en.xlsm
    └─ 中文
        ├─ 中文.py
        └─ 中文.xlsm
```

配置全局 xlwings 参数，

```Conf %userprofile%\.xlwings\xlwings.conf
"USE UDF SERVER","False"
"DEBUG UDFS","False"
"INTERPRETER","python"
"CONDA PATH","E:\Miniconda"
"CONDA ENV","py37"
"UDF MODULES",""
```

分别打开三个 xlsm 文件，点击 `Import Functions` 导入相应 .py 文件中定义的函数，`cn`、`en` 下没有问题，但 `中文` 的目录下报错找不到 `中文` 模块：

```
pythoncom error: Python error invoking COM method.

Traceback (most recent call last):
  File "E:\Miniconda\envs\py37\lib\site-packages\win32com\server\policy.py", line 278, in _Invoke_
    return self._invoke_(dispid, lcid, wFlags, args)
  File "E:\Miniconda\envs\py37\lib\site-packages\win32com\server\policy.py", line 283, in _invoke_
    return S_OK, -1, self._invokeex_(dispid, lcid, wFlags, args, None, None)
  File "E:\Miniconda\envs\py37\lib\site-packages\win32com\server\policy.py", line 586, in _invokeex_
    return func(*args)
  File "E:\Miniconda\envs\py37\lib\site-packages\xlwings\server.py", line 191, in Call
    return ToVariant(getattr(obj, method)(*pargs, **kwargs))
  File "E:\Miniconda\envs\py37\lib\site-packages\xlwings\udfs.py", line 442, in import_udfs
    module = get_udf_module(module_name, xl_workbook)
  File "E:\Miniconda\envs\py37\lib\site-packages\xlwings\udfs.py", line 224, in get_udf_module
    module = import_module(module_name)
  File "E:\Miniconda\envs\py37\lib\importlib\__init__.py", line 127, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
  File "<frozen importlib._bootstrap>", line 1006, in _gcd_import
  File "<frozen importlib._bootstrap>", line 983, in _find_and_load
  File "<frozen importlib._bootstrap>", line 965, in _find_and_load_unlocked
ModuleNotFoundError: No module named '中文'
```

也就是说，如果 Excel 文件所在目录的绝对路径中含中文 xlwings 就会找不到相应的 .py 文件；如果 Excel 文件所在路径不含中文，即便工作簿本身（Excel 文件名）中包含中文，xlwings 也照常工作。

-----------

但从 xlwings 官方文档日志中判断在 Windows 下应已经修复了路径包含 Unicode 字符问题：


**v0.9.3 (Aug 22, 2016)**

- [Win] App.visible wasn’t behaving correctly (GH551).
- [Mac] Added support for the new 64bit version of Excel 2016 on Mac (GH549).  
  Unicode book names are again supported (GH546).

**v0.3.6 (July 14, 2015)**

Bug Fixes

- [Win]: When using the OPTIMIZED_CONNECTION on Windows, Excel left an orphaned process running after closing (GH193).  

Various improvements regarding unicode file path handling, including:

- [Mac]: Excel 2011 for Mac now supports unicode characters in the filename when called via VBA’s RunPython (but not in the path - this is a limitation of Excel 2011 that will be resolved in Excel 2016) (GH154).
- [Win]: Excel on Windows now handles unicode file paths correctly with untrusted documents. (GH154).
