---
layout: post
title:  "Angular için fake backend"
tags: angular ng json
categories: angular
---
Angular ile kodlama yaparken yada test ederken sıkıkla bir api uç noktasına ihtiyacım oluyor. Bunun için pratik bir yöntem olan json-server kullanıyorum.

# Kurulum

``` shell
npm i -g json-server
```

# Yapılandırma

``` shell
mkdir data && cd data
nano db.json
```

data adlı bir klasör oluşturup içine de db.json dosyası ekleyeceğim. Nano dan çıkarken kayıt edeceğimden dolayı db.json dosyası oluşmuş olacak.

Dosya içeriği ise;

``` json
{
  "product": [
    {
      "id": 1,
      "name": "Kalem",
      "price": 200,
      "stock": 412,
      "categoryId": 1
    },
    {
      "id": 2,
      "name": "Silgimre",
      "price": 100,
      "stock": 670,
      "categoryId": 1
    }
  ],
  "category": [
    {
      "id": 1,
      "name": "Kırtasiye"
    }
  ]
}
```

# Kullanma

json dosyasının bulunduğu klasörde aşağıdaki komutunu çalıştırıyorum.

``` shell
json-server --watch db.json
```

Sonuç;

``` shell
Press CTRL-C to stop
Watching db.json...

(˶ᵔ ᵕ ᵔ˶)

Index:
http://localhost:3000/

Static files:
Serving ./public directory if it exists

Endpoints:
http://localhost:3000/product
http://localhost:3000/category
```

Test için bir get isteği.

``` shell
curl -X GET -H "Content-Type: application/json" -d '{
}' http://localhost:3000/product
```

Son olarak bir de post isteği.

``` shell
curl -X POST -H "Content-Type: application/json" -d '{
  "id": "3",
  "name": "Defter",
  "price":"45",
  "stock":"900",
  "categoryId":"1"
}' http://localhost:3000/product
```

Post isteği sonucu tüm veri listelenecektir.