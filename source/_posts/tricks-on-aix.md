---
title: AIX 系统技巧
date: 2020-10-26 21:35:02
tags:
    - AIX
categories: Wiki
---

AIX 系统相关技巧。

<!-- more -->

1. 针对终端中文字符乱码，可将字符集设置为 UTF-8（并不推荐改成狭隘的 `GB` 系列编码），并将系统 `locale` 设为中文：
   ```Bash
   export LANG=ZH_CN
   ```
   其中，中文相关的编码有：

   🚀 **Unicode encoded languages and locales in AIX**

   | Language       | Territory or region       | Code set | Default locale name | Aliases     | Introduced in AIX release |
   | :--:           | :--:                      | :--:     | :--:                | :--:        | :--:                      |
   | Chinese (Hans) | China                     | UTF-8    | ZH_CN               | ZH_CN.UTF-8 | 5.2 or earlier            |
   | Chinese (Hans) | China                     | UTF-8    | zh_Hans_CN.UTF-8    |             | 7.1.2.0                   |
   | Chinese (Hans) | Hong Kong S.A.R. of China | UTF-8    | ZH_HK               | ZH_HK.UTF-8 | 5.2 or earlier            |
   | Chinese (Hans) | Singapore                 | UTF-8    | ZH_SG               | ZH_SG.UTF-8 | 5.2 or earlier            |
   | Chinese (Hans) | Singapore                 | UTF-8    | zh_Hans_SG.UTF-8    |             | 7.1.2.0                   |
   | Chinese (Hant) | Hong Kong S.A.R. of China | UTF-8    | zh_Hant_HK.UTF-8    |             | 7.1.2.0                   |
   | Chinese (Hant) | Macao                     | UTF-8    | zh_Hant_MO.UTF-8    |             | 7.2.1.0                   |
   | Chinese (Hant) | Taiwan                    | UTF-8    | ZH_TW               | ZH_TW.UTF-8 | 5.2 or earlier            |
   | Chinese (Hant) | Taiwan                    | UTF-8    | zh_Hant_TW.UTF-8    |             | 7.1.2.0                   |

   🚀 **Non-Unicode encoded languages and locales in AIX**

   | Language       | Territory or region       | Code set   | Default locale name | Aliases          | Introduced in AIX release |
   | :--:           | :--:                      | :--:       | :--:                | :--:             | :--:                      |
   | Chinese (Hans) | China                     | IBM-eucCN  | zh_CN               | zh_CN.IBM-eucCN  | 5.2 or earlier            |
   | Chinese (Hans) | China                     | GB18030    | zh_CN               | Zh_CN.GB18030    | 5.2 or earlier            |
   | Chinese (Hant) | Hong Kong S.A.R. of China | BIG5-HKSCS | Zh_HK               | Zh_HK.BIG5-HKSCS | 5.2 or earlier            |
   | Chinese (Hant) | Taiwan                    | IBM-eucTW  | zh_TW               | zh_TW.IBM-eucTW  | 5.2 or earlier            |
   | Chinese (Hant) | Taiwan                    | big-5      | Zh_TW               | Zh_TW.big-5      | 5.2 or earlier            |

