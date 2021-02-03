---
layout: post
title:  "Linux’a Giriş — 5 — Dosya Arşivleme ve Sıkıştırma"
date:   2021-02-03 10:12:22 +0300
categories: linux 
sidebar: []
tags: linux arşivleme sıkıştırma tarkomutu
---


 Bu yazımda, dosyaları Linux'ta nasıl arşivleyeceğimizi ve sıkıştıracağımızı öğreneceğiz. Arşivleme ve sıkıştırma kelimelerinin genellikle aynı anlama geldiği düşünülebilir. Ancak ikisi tamamen farklı süreçlerdir. Dosyaların arşivlenmesi ve sıkıştırılması linux dünyasında sistem yöneticileri tarafından çok düzenli yapılan işlemlerdir. Bu işlemleri nasıl gerçekleştireceğimizi öğrenelim.
### Dosya Arşivleme ve Sıkıştırma

Arşivleme, birden çok dosyayı tek bir dosyada birleştirme işlemidir. Sıkıştırma ise bir dosyanın boyutunu küçültme işlemidir. Arşivleme genellikle dosyaları bir sistemden diğerine taşırken kullanılır. 

# $ tar Komutu Kullanarak Dosya Arşivleme

tar komutu birden çok dosyayı tek bir dosyada birleştirmek veya depolamak için kullanılır. Tar komutunun dört ana parametresi bulunmaktadır.

- **c** – Bir dosya veya dizinden arşiv oluşturur
- **x** – Arşivden çıkarır
- **r** – Arşive dosya ekler
- **t** - Arşivin içerisindeki dosyaları listeler


```
tar -cf arsiv.tar dosya1.txt dosya2.txt
```

*-c* (create) parametresi yeni bir arşiv oluşturmaya yarar. *-f* parametresi ile arşiv içerisine eklenecek dosyalar belirtilir. Benzer şekilde bir dizinden arşiv oluşturalım.

```
tar -cf arsiv.tar dizin/
```

Arşivi mevcut dizinde çıkartmak için aşağıdaki komutu yazmanız yeterlidir:

```
tar -xf arsiv.tar
```

Arşivi farklı bir dizine çıkarmak için *-C* parametresi kullanılmaktadır.

```
tar -xf arsiv.tar -C Downloads/
```

*-C* parametresi kullanılarak dosyalar belirtilen dizine (örnekte Downloads dizini) çıkarılır.

```
tar -cvf arsiv.tar test1.txt test2.txt
```

*-v* parametresi hangi dosyaların arşivlendiğini gösterir.

![Linux](https://i.ibb.co/5jzkPdn/tarvparametre.png)

```
tar -tf arsiv.tar
```

*-t* parametresi arşivden çıkarmadan dosyanın içerisinde bulunan dosyaları gösterir.

```
tar -rf arsiv.tar test3.txt
```

*-r* parametresi arşivlenmiş bir dosyanın içerisine farklı bir dosya eklemeye yarar.

# Sıkıştırılmış Arşivler Oluşturma

Varsayılan olarak *tar* komutu .tar uzantılı arşiv dosyası oluşturur. Ayrıca, *tar* komutu ile gzip ve bzip kullanılarak sıkıştırma işlemleri gerçekleştirilebilir. *gzip* kullanılarak arşivlenmiş ve sıkıştırılmış dosyaların uzantısı .tar.gz yada .tgz, *bzip* kullanılarak oluşturulanların ise .tar.bz2 yada .tbz olur.

Öncelikle **gzip** arşivi oluşturalım: 

```
tar -czf arsiv.tar.gz dizin/
```

yada

```
tar -czf arsiv.tgz dizin/
```

*gzip* sıkıştırma yöntemini kullanmak için *-z* parametresi kullanılmaktadır.

```
tar -xzvf arsiv.tar.gz
```

Ziplenmiş ve arşivlenmiş dosyayı çıkarmak için *-x* *-z* parametreleri kullanılır.

**bzip** arşivi oluşturalım:

```
tar -cjf arsiv.tar.bz2 dizin/
```

yada

```
tar -cjf arsiv.tbz dizin/
```

*bzip* sıkıştırma yöntemini kullanmak için *-j* parametresi kullanılmaktadır.

```
tar -xjvf arsiv.tbz
```

Ziplenmiş ve arşivlenmiş dosyayı çıkarmak için *-x* *-j* parametreleri kullanılır.
