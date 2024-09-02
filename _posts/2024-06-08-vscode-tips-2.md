---
layout: post
title:  "VSCode - İpucu - 2"
tags: vscode
categories: vscode
---

Html kod düzeltme için örnek ayar. Özellikle uzun kod satırlarının saçma sapan kesilmemesi için ilk satır önemli. 

``` json
{
  "html.format.wrapLineLength": 0,
    "[html]": {
    "editor.defaultFormatter": "vscode.html-language-features",
    "editor.formatOnSave": true
  },
}
```