---
layout: post
title:  "VSCode - İpucu - 2"
tags: vscode
categories: vscode
---
VSCode'da HTML kod düzeltme işlemleri için kullanılabilecek örnek bir ayar. Bu ayar, özellikle uzun kod satırlarının gereksiz yere kesilmesini engellemek için faydalıdır.
``` json
{
  "html.format.wrapLineLength": 0,
    "[html]": {
    "editor.defaultFormatter": "vscode.html-language-features",
    "editor.formatOnSave": true
  },
}
```