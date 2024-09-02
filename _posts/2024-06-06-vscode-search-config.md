---
layout: post
title:  "VSCode dosya içi aramalar"
tags: vscode
categories: vscode
---

Vscode ile global bir arama yapıldığında ne kadar dosya varsa hepsininin içinden geçer. Bu da ciddi bir zaman ve CPU tüketmek demek. Özellikle Angular gibi node paketlerin bolca olduğu projelerde can sıkıcı olabilir. Bunun önüne geçmek için arama yaparken bazı kısıtlamalar getirebiliriz.

Öncelikle yoksa kök dizine .vscode klasörünü ekleyelim. Daha sonra aynı klasöre settings.json dosyasını ekleyelim.

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

Proje klasör yapınıza göre uyarlama yapabilirsiniz.