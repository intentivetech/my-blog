---
layout: post
title:  "Linux’a Giriş — 7 — Grep Komutu"
date:   2021-02-03 15:13:22 +0300
categories: linux 
sidebar: []
tags: linux dosyaiçindearama grep
---

Bu yazımda sizlere dosyanın içerisinde nasıl arama yapacağımızdan bahsedeceğim. Bir Linux sisteminde, belirli bir metin dizesi için bir veya daha fazla dosyada arama yapma ihtiyacı oldukça sık ortaya çıkabilir. *grep* komutu dosyalar içerisinde kolay bir şekilde arama yapmayı sağlar. 

Basit kullanımı:

```bat
grep <parametre> <aranan-ifade> <yol>
```

Bir metin dizesi ararken ihtiyaç duyabileceğiniz kullanışlı grep parametreleri şunlardır: 

- **-c** - Bir kelimenin kaç kez kullanıldığının sayısını gösterir
- **-i** - Büyük küçük harf kullanımını göz ardı eder
- **-n** - Aranan metnin bulunduğu satır sayısını gösterir 

*grep* komutu ile dosyanın içerisindeki herhangi bir kelimeyi arayabiliriz. Örnek olarak <code>bilgisayar.txt</code> adında bir dosya oluşturup içerisine şu cümleyi yazalım: “Bilgisayar, kendisine verdiğimiz bilgileri istediğimizde saklayabilen, istediğimizde geri verebilen cihaza denir. İlk elektrikli bilgisayar ENIAC’tır.”

```bat
grep "elektrik" bilgisayar.txt
```

bilgisayar.txt isimli dosyanın içerisinde elektrik kelimesinin geçtiği satırı bulur. 

![Grep kullanımı](https://i.ibb.co/sp4LpjC/grep.png)

```bat
grep "ELEKTRIK" bilgisayar.txt
```

Büyük harflerle "ELEKTRIK" kelimesi yazıldığında hiçbir sonuç listelenmeyecektir çünkü dosya içerisinde büyük harfle yazılmış "ELEKTRIK" kelimesi yoktur.

```bat
grep -i "ELEKTRIK" bilgisayar.txt
```

*-i* parametresi ile büyük küçük harf kullanımını göz ardı ederek istenen kelimeyi arar.

```bat
grep -r "bilgisayar"
```

*-r* parametresi tüm alt dizinlerdeki dosyaları özyinelemeli olarak arar. 

![Grep -r parametresi](https://i.ibb.co/sCty8DK/grep-r.png)

```bat
grep -c "bilgi" bilgisayar.txt
```

*-c* parametresi içerisinde bilgi kelimesi geçen kelimelerden kaç tane olduğunu gösterir.

```bat
grep -n "bilgi" bilgisayar.txt
```

“bilgi” kelimesinin kaçıncı satırlarda geçtiğini görebiliriz.

```bat
grep -E -i "elektrik|bilgisayar" bilgisayar.txt
```

Bir dosyada birden çok dizeyi aramak için *-E* parametresi kullanılır. Farklı arama terimleri <code>|</code> (veya) karakteri ile ayrılır. 