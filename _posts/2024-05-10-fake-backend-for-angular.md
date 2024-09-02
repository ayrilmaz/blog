---
layout: post
title:  "Angular için fake backend"
tags: angular ng json
categories: angular
---
Angular ile kodlama yaparken ya da test ederken sık sık bir API uç noktasına ihtiyaç duyabiliyorum. Bunun için pratik bir yöntem olan `json-server` kullanıyorum.
# Kurulum
``` shell
npm i -g json-server
```
# Yapılandırma
``` shell
mkdir data && cd data
nano db.json
```
data adlı bir klasör oluşturup içine de db.json dosyasını ekleyeceğim. Nano'dan çıkarken dosyayı kaydettiğimde, db.json dosyası oluşmuş olacak. Dosya içeriği ise şu şekilde olacak:
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
JSON dosyasının bulunduğu klasörde aşağıdaki komutu çalıştırıyorum:
``` shell
json-server --watch db.json
```
Sonuç olarak, aşağıdaki gibi bir çıktı alacaksınız:
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
Test için bir GET isteği örneği:
``` shell
curl -X GET -H "Content-Type: application/json" -d '{
}' http://localhost:3000/product
```
Son olarak bir de POST isteği örneği:
``` shell
curl -X POST -H "Content-Type: application/json" -d '{
  "id": "3",
  "name": "Defter",
  "price":"45",
  "stock":"900",
  "categoryId":"1"
}' http://localhost:3000/product
```
POST isteği sonucu, tüm veriler listelenecektir.