---
layout: post
title:  "Docker Compose - Common Variables"
tags: docker compose
categories: docker
---

Compose içersinde .env dosyası kullanmadan ortak değişkenler oluşturabileceğimiz bir alternatif olan common-variables örneği paylaşıyorum. Şurada köşede dursun.

``` shell
x-common-variables: &common-variables
  TZ: Europe/Istanbul
  POSTGRES_USER: pgUser
  POSTGRES_PASSWORD: pgPassword
  POSTGRES_DB: testdb
  RABBITMQ_DEFAULT_USER: rabbitUser
  RABBITMQ_DEFAULT_PASS: rabbitPassword

services:
  postgres:
    image: postgres:16
    environment:
      <<: *common-variables
    ports:
      - 5432:5432

  rabbitmq3b:
    image: rabbitmq:3-management
    environment:
      <<: *common-variables
    ports:
      - 5672:5672
      - 15672:15672
```