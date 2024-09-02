---
layout: post
title:  "VSCode - İpucu - 1"
tags: vscode
categories: vscode
---
VSCode ile global bir arama yapıldığında, mevcut tüm dosyaların içinden geçer. Bu işlem ciddi bir zaman ve CPU tüketimi demektir. Özellikle Angular gibi node paketlerinin bolca bulunduğu projelerde can sıkıcı olabilir. Bunun önüne geçmek için arama yaparken bazı kısıtlamalar getirebiliriz.

Öncelikle, kök dizinde yoksa `.vscode` klasörünü ekleyelim. Ardından, aynı klasöre `settings.json` dosyasını ekleyelim.
``` json
{
  "search.exclude": {
    "**/.editorconfig": true,
    "**/.git": true,
    "**/.gitignore": true,
    "**/.vscode": true,
    "**/*.csproj": true,
    "**/bazel-out": true,
    "**/bin": true,
    "**/Directory.Packages.props": true,
    "**/dist": true,
    "**/node_modules/**": true,
    "**/obj": true,
    "**/package-lock.json": true,
    "**/package.json": true,
    "**/.angular/**": true
  }
}
```

Bu ayarları, proje klasör yapınıza göre uyarlayabilirsiniz.