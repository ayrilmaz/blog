---
layout: post
title:  "C# - İpucu - 1"
tags: csharp
categories: csharp
---
# Byte

```csharp
byte sayi1 = 10;
byte sayi2 = 10;
byte toplam = say1 + sayi2;//kod derlenmez
```

**byte** veri tipine 0-255 arası pozitif tam sayı değerleri atanabilir. Yukarıdaki kodda her bir değişken değeri aralıkta bile olsa taşma olasılığı var. Diğer bir değişle; sayi1 değişkenin değerini 255 yaptığımzda sonuç aralık dışına çıkmış olur. Derleyici bunun kaçınılmaz olduğunu bildiği için iki byte değeri aritmetik işlemlere girdiğinde sonucu Int32 veya üstü bir veri tipi olarak bekler.

```csharp
byte sayi1 = 10;
byte sayi2 = 10;
int toplam = say1 + sayi2;//kod derlenir
```

> negatif değerler için **sbyte** tercih edilmeli