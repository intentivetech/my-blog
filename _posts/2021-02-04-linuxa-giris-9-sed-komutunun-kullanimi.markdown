---
layout: post
title:  "Linux’a Giriş — 9 — Sed Komutunun Kullanımı"
date:   2021-02-04 11:02:22 +0300
categories: linux 
sidebar: []
tags: linux sed
---

# $ sed Komutu 

*sed* komutu, akış düzenleyicinin kısaltmasıdır ve dosya üzerinde arama, bulma ve değiştirme, ekleme veya silme gibi birçok işlevi gerçekleştirebilir. *sed* komutunun en yaygın kullanımı, değiştirme veya bulma ve değiştirme içindir. *sed*'i kullanarak dosyaları açmadan bile düzenleyebilirsiniz; bu, dosyadaki bir şeyi bulmanın ve değiştirmenin çok daha hızlı bir yoludur.

Bir dosya oluşturalım.

```bat
nano ornek.txt
```

Dosya içerisine aşağıdaki metni yapıştırın.

```
Fatih elmasuyu
Suzan portakalsuyu
Melih kavunsuyu
Melih kavunsuyu
Rasim kirazsuyu
Tarık portakalsuyu
Lale şeftalisuyu
Suzan portakalsuyu
Melih kayısısuyu
Ayşe mangosuyu
Galip havuçsuyu
Osman karpuzsuyu
Betül narsuyu
```

```bat
sed 's/u/Z/' ornek.txt
```

Metinde bulunan ‘u’ harflerini ‘Z’ harfi yapar yalnız aynı satırda birden fazla ‘u’ varsa sadece ilk bulduğunu değiştirir.

```bat
sed 's/portakalsuyu/limonata/g' ornek.txt
```

Metinleri değiştirme ‘s’ işleci ile gerçekleşir. Yukarıdaki örnekte portakalsuyu yerine limonata yazar. ‘g’ ise tüm satırda aynı işlemi uygulamaya yarar.

```bat
sed 's/u/Z/g' ornek.txt
```

'g' işleci kullanılarak aynı işlemin tüm satırda uygulanması sağlanabilir.

```bat
sed 's/^F/f/' ornek.txt
```

<code>^</code> karakteri satır başını ifade eder. F harfi ile başlayan kelimeleri f ile değiştir.

```bat
sed 's/$/SATIRSONU/' ornek.txt
```

<code>$</code> karakteri satır sonu anlamına gelir ve bu komut satır sonlarına SATIRSONU yazar.

```bat
sed 's/^$/BOSSATIR/' ornek.txt
```

<code>^$</code> boş satırları ifade eder. Yukarıdaki örnek boş satırlara BOSSATIR kelimesini yazar.

```bat
sed '/^$/d' ornek.txt
```

*d* işleci ile silme işlemleri gerçekleştirilir. Yukarıdaki örnek komut boş satırları silmeyi sağlar.

```bat
sed -e 's/Z/u/g' -e 's/e/B/g' ornek.txt
```

*-e* parametresi birden fazla işlem gerçekleştirmek için kullanılır.

# AWK

Basitleştirilmiş bir programlama dili olarak görülebilir. Bir dosya içerisinde istediğimiz sütunlara kolayca ulaşabiliriz.Veya sütunlar arası matematiksel işlemleri kolayca yapabiliriz.

```bat
awk '{print $1}' ornek.txt
```

Az önceki dosyamız için bu komutu yazarsak 1. sütunu yani isim listesini ekrana getirir. Varolan dosyamızı şu şekilde düzenleyelim.

```
Fatih elmasuyu 5 7
Suzan portakalsuyu 3 9
Melih kavunsuyu 2 5
Melih kavunsuyu 7 2
Rasim kirazsuyu 8 4
Tarık portakalsuyu 4 5
Lale şeftalisuyu 5 2
Suzan portakalsuyu 1 8
Melih kayısısuyu 2 5
Ayşe mangosuyu 6 3
Galip havuçsuyu 7 8
Osman karpuzsuyu 3 5
Betül narsuyu 5 1
olarak değiştirelim.
```

```bat
awk '{sonuc=$3*$4; print $0 ,sonuc}' ornek.txt
```

Komutumuz 3 ve 4. satırları çarparak bir sonuc değişkenine atıyor ekrana hem tüm dosya içeriğini hemde sonucu yazıyor.