---
layout: post
title:  "Linux’a Giriş — 2 — Temel Linux Komutları"
date:   2021-01-28 11:04:24 +0300
categories: linux
sidebar: []
---


Bu yazımda birinci yazımın devamı olarak linuxta kullanılan temel komutlardan bahsedeceğim.

### $ du komutu

Sisteminizdeki bir dosyanın disk kullanımını öğrenmek için *du* komutu kullanılır. 

```
du Documents
```

Örneğin belgeler klasörü tarafından kullanılan disk alanını öğrenmek istiyorsanız bu şekilde öğrenebilirsiniz.

### $ echo komutu 

*echo* komutu, bazı verileri, genellikle metni bir dosyaya taşımamıza yardımcı olur.

```
echo hello, this is test >> new.txt”
```

Örneğin, yeni bir dosyaya veya varolan bir dosyaya hello,this is test satırını eklemek için bu şekilde kullanabilirsiniz.


### $ cat komutu

Bir dosyanın içeriğini görüntülemek için cat komutunu kullanılır.

```
cat dosyaadı
```

şeklinde basit bir kullanımı vardır.

### $ nano komutu 

*nano* komut satırında yüklü metin düzenleyicidir. Nano, anahtar kelimeleri renkli olarak belirten ve çoğu dili tanıyan iyi bir metin düzenleyicidir.

```
nano new.txt
```

Örneğin "new.txt" adında yeni bir dosya oluşturarak, dosyanın içerisine yazı yazabilirsiniz. Düzenlemelerinizi gerçekleştirdikten sonra sırasıyla Ctrl + X, ardından Ctrl + Y(Yes) tuşlarına basarak dosyada yapılan değişiklikleri kaydedebilirsiniz.

### $ history komutu 

Terminalde yazdığımız komut geçmişini gösterir.

```
history -c
```

-c parametresi ile komut geçmişini temizleyebilirsiniz.

```
history > dizin/28.01.2021
```

 Örneğin hergün yazdığımız komutları kaydetmek istediğimizi düşünelim. Bunun için şöyle bir komut yazabiliriz. Bir dizin oluşturup içerisinde oluşturduğumuz dosyaya historynin çıktılarını atabiliriz. 

 Geçmişte yazdığımız bir komutu arıyorsak history yazdıktan sonra CTRL+R ye basarak aramak istediğimiz komutun belirli bir kısmını yazarsak bulabiliriz.

 history komutunu yazdıktan sonra komutların başında belirli numaralar olduğunu farketmişsinizdir. Bu numaralar ile de bir komutu çalıştırabiliriz. Örneğin 65 numaralı komutumuz ls olsun ve bunu çalıştıralım. !65

 ### $ head komutu 

 *head* komutu, kendisine verilen dosyaların ilk bölümünün standart giriş yoluyla çıktısını almak için kullanılmaktadır.

```
head dosyaadı
```

şeklinde kullanılmaktadır. 

Komutun çıktasında görüldüğü üzere dosya içerisindeki ilk 10 satırının görüntülenmesini sağlamaktadır. 

```
head -n 5 dosyaadı
```

-n parametresiyle görüntülenmek istenen satır sayısında değişiklik yapılabilmektedir. Örneğin 5 yazıldığında ilk 5 satırı listeler.

```
head -n -5 dosyaadı
```

Negatif bir sayı girildiğinde ise listemeyi sondan başlayarak gerçekleştirir.

### $ tail komutu 

*tail* komutu, varsayılan olarak her dosyanın son 10 satırını standart çıktıya yazdırır.

```
tail dosyaadı
```

Belirtilen dosya içerisindeki son 10 satırı görüntüler.

```
tail -n 5 dosyaadı
```

-n parametresiyle görüntülemek istediğimiz satır sayısında değişiklik yapabiliriz. Bu şekilde son 5 satırı görüntüleyebiliriz.

 # > , >> , | KULLANIMI

 ## > 

 *>* işareti bir dosyanın içerisine yazı yazmamızı sağlar. Sistemde varolan bir dosyayı kullanırsak içeriği silinerek yeni yazdıklarımız kaydolur. Komutu yazdıktan sonra bir alt satırda dosya içerisine yazmak istediklerimizi girebiliriz, CTRL+D ile yazı yazma işlemini bitirebiliriz. cat dosyaadi ile yaptıklarımızı gözden geçirebilirsiniz.

```
cat > dosyaadı
```

**NOT:** *cat* komutu yerine başka komutlarda deneyebilirsiniz. Örneğin *ls* > dosyaadı komutunu yazarsanız *ls* çıktısını bir dosyaya aktardığını görebilirsiniz.

## > 

*>* kullanırken içerisi dolu olan bir dosyaya yazı yazdığımızda içerisinin silindiğinden bahsetmiştim. Bunu engellemek için *>>* kullanılmaktadır. Yazdıklarınızı dosyanın en alt kısmına ekler.

## | (pipe)

Pipe yazdığımız bir komutun çıktısını başka bir komutun girdisi olarak atar.

```
ls | wc
```

Örneğin *ls* komutunun çıktısındaki satır,kelime ve byte değerlerini(wc) verir.

Bir sonraki yazımda dosya sistemi yapısı, dosya yetkileri ve türlerinden bahsedeceğim. Herkese iyi çalışmalar.