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
**byte** veri tipine 0-255 arası pozitif tam sayı değerleri atanabilir. Yukarıdaki kodda her bir değişkenin değeri aralıkta olsa bile taşma olasılığı vardır. Başka bir deyişle, sayi1 değişkeninin değerini 255 yaptığımızda sonuç aralık dışına çıkar. Derleyici, bunun kaçınılmaz olduğunu bildiği için iki byte değeri aritmetik işlemlere girdiğinde sonucu Int32 veya daha üst bir veri tipi olarak bekler.
```csharp
byte sayi1 = 10;
byte sayi2 = 10;
int toplam = say1 + sayi2;//kod derlenir
```
> negatif değerler için **sbyte** tercih edilmeli