---
layout: post
title:  "Linux’a Giriş — 1 — Temel Linux Komutları"
date:   "27 Ocak 2021"
categories: linux
sidebar: []
---




# Linux Nedir? 

![Linux](https://i.ibb.co/zrm5BGg/linux.png)

Windows, iOS ve Mac OS gibi Linux bir işletim sistemidir. Basitçe söylemek gerekirse, işletim sistemi, yazılımınız ile donanımınız arasındaki iletişimi yönetir. İşletim sistemi  olmadan, yazılım çalışmaz. Çoğu zaman linux kullandığımızın farkında bile olmayız. Örneğin dünyadaki en popüler platformlardan biri olan Android, gücünü Linux'tan almaktadır. Akıllı telefonlardan arabalara, süper bilgisayarlara ve ev aletlerine, ev masaüstü bilgisayarlarından kurumsal sunuculara kadar, Linux işletim sistemi her yerde.

<!--more-->

[Sanal Makineye Linux İşletim Sistemi nasıl kurulur?](http://yapbenzet.kocaeli.edu.tr/virtualbox-ubuntukurulumu/) [Windowsun yanına Centos Kurulumu](https://tr.compozi.com/how-install-centos-7-alongside-windows-10-dual-boot)

# Terminal Nedir?

Terminal, bir kullanıcının komutlar yazarak bilgisayarla iletişime geçtiği ortamdır. Dağıtımlara göre bazı komutlar farklılık gösterebilir. Bazı görevleri bir Terminal kullanarak tamamlamak, grafik uygulamalar ve menülerden çok daha hızlı olabilir. 

## Temel Linux Komutları

### $ pwd komutu

```
pwd
```
Terminali ilk açtığınızda, kullanıcınızın ana dizinindesinizdir (home). Hangi dizinde olduğunuzu öğrenmek için *pwd* komutunu kullanabilirsiniz. Bize root dizininden başlayan yolu verir. Root dizini linux dosya sisteminin temelidir. Dosya sisteminden ilerleyen yazılarımda bahsedeceğim. Ana dizin "/home/username" şeklindedir.  

### $ ls komutu

Linuxta bulunan en önemli komutlardan birisi de *ls* komutudur. *ls* komutu bulunduğumuz dizin içerisindeki dosya ve klasörleri listeler.

![ls komutu](https://i.ibb.co/6BBgS3b/ls.png)

```
ls
```
Basitçe kullanımı bu şekildedir. Ayrıntılı şekilde listeleme işlemini farklı parametreler ile gerçekleştirebiliriz.

```
ls -l
```

-l parametresi dosyaların boyut tarih bilgilerini göstererek listeler. 


**NOT:** Bir komuttan sonra - ile başlayan bir komut kullanımı parametre olarak adlandırılır. Burada ls komutunun l parametresini gördük.

```
ls -a
```

-a parametresi gizli dosyalar ve dizinler ile birlikte listeleme işlemi yapar.

# $ cd komutu

Dizinler arası geçiş yapmayı sağlayan komuttur. Örneğin şuanda ana dizinde (home) olduğunuzu varsayalım ve farklı bir dizine gitmek istiyorsunuz. (Dizinleri windowstaki klasörler olarak düşünebilirsiniz)

```
cd dizin_adi
```

komutunu kullanarak bulunduğunuz dizinden belirttiğiniz dizine gidebilirsiniz.


**Örneğin;** bir test dizininizin içerisinde dizin1 ve dizin2 adında iki tane dizininiz olsun. *cd dizin1* komutu ile dizin1 ‘e geçtiğinizde, daha sonra tekrar dizin2'ye geçmek için *cd dizin2* komutunu çalıştırdığınızda hata alacaksınız. Hatanın sebebi dizin2, test dizinin altında bulunurken, dizin1'in içerisindeyken dizin2'ye geçiş yapmanızdır.


```
cd ../dizin2
```

İç içe dizinler arası geçiş yaparken **..** bir önceki dizini belirtir. Bu şekilde alt dizindeyken dizin2'ye ulaşmış oluruz.


# $ touch komutu

Touch komutu dosya oluşturmaya yarar. Örnek kullanımı aşağıdaki şekildedir.

```
touch DOSYA_ADI
```

Dosya oluşturduktan sonra ls komutu ile bulunduğumuz dizindeki klasörleri listelersek, dosyanızın oluştuğunu görebilirsiniz.


**NOT:** Varolan bir dosyayı tekrar touch DOSYA_ADI şeklinde yazarsak dosyanın oluşturulma saatinin değiştiğini görebiliriz. (ls -l ile)

```
touch dosya1 dosya2
```

Birden fazla dosya oluşturmak için dosya isimlerini yan yana yazmamız yeterlidir.

```
touch dosyaadi{1..10}
```
Bu yöntemle birden fazla dosyayı kolayca oluşturabiliriz. dosyaadi1 , dosyaadi2 .. dosyaadi10 ‘ a kadar.


# $ mkdir komutu

mkdir komutu dizin oluşturmak için kullanılır. Dizinlerin dosyalar ile farkı ise dosyalarının .txt, .jpg, .mp3 gibi uzantılarının olmasıdır.

```
mkdir DİZİN_ADI
```

mkdir komutunun basitçe kullanımı yukarıdaki gibidir. ls komutunu kullanarak dizinin oluşturulduğunu görebilirsiniz.

```
mkdir DİZİN1 DİZİN2
```
Aynı anda birden fazla dizin oluşturmak için yan yana yazabiliriz. 


# $ rm komutu

Dosyaları ve dizinleri silmek için kullanılır.

```
rm dosya_adi
```

Sildikten sonra “silindi” şeklinde bir cevap gelmediği için yaptığımız işlemler sonrasında ls komutunu kullanarak kontrol edebilirsiniz.

```
rmdir dizin_adi
```

Dizinleri silmek için kullanılır ancak dizinin içerisi doluysa hata verir.

```
rm -r dizin_adi
```
İçerisi dolu olan bir dizini silmek için -r parametresini kullanmak gerekir. 

```
rm -rf
```

-f parametresi “silmek istediğinize emin misiniz” gibi soruları önler.

```
rm -rf dosya[12345]
```
Aynı anda isimleri dosya1,dosya2.. şeklinde giden 5 dosyayı bu şekilde tek bir komutla silebilirsiniz.

# $ mv komutu

*mv* komutu dosyaları komut satırı üzerinden farklı bir dizine taşımayı sağlamaktadır. Ayrıca dosyaların isimlerini değiştirmek için de kullanabiliriz. 

```
mv dosya1 dosya2yeni
```
Örneğin dosya1 isimli dosyanın adını dosya2yeni ile değiştirmek için *mv varolandosyaadı yenidosyaadı* komutu kullanılmaktadır.

Home dizininde (~) iki tane dizinimiz olsun (mkdir komutu ile oluşturabilirsiniz). dizin1 ve dizin2. dizin1'in içerisinde tasinacakdosya adında bir dosya olsun ve bunu dizin2'ye taşıyalım.

Bunun için cd dizin1 diyerek dizin1'e geçiş yapmak gerekmektedir. Daha sonra aşağıdaki komutu çalıştırın.

```
mv ./tasinacakdosya ../dizin2
```
Bu komutun anlamı: dizin1'in içerisindeki (. şuan bulunduğum dizin yani dizin1) tasinacakdosya adlı dosyayı al bir üst dizindeki (..)dizin2 isimli dizine taşı.

Aynı işlemler dizin1'e geçmeden de yapılabilir.

```
mv dizin1/tasinacakdosya dizin2
```
Hangisi daha kolayınıza gelirse :)


# $ cp komutu

Dosyaları komut satırı üzerinden kopyalamak için *cp* komutu kullanılmaktadır.

```
cp dosya1 kopya
```
Basitçe kullanımı bu şekildedir. 


Farklı bir dizine kopyalama işlemi:

```
cp dosya1 dizin2/kopya
```
Örnek olarak dizin2'nin içerisine kopyaladım, başka bir yere de kopyalayabilirsiniz.


### $ man komutu

Linux’un en önemli komutlarından biri de *man* komutudur.

```
man komut
```

Bir komut ve nasıl kullanılacağı hakkında daha fazla bilgi edinmek için man komutunu kullanın. Bilgi edinmek istenen komutun nasıl kullanıldığı ve komutun ne işe yaradığı gibi bilgilerin olduğunu gösterir. Komutun sahip olduğu tüm parametreleri ve anlamlarını açıklar. Bu komut sayesinde Linux hakkında hiçbir bilgisi olmayan kişiler rahatlıkla Linux kullanabilir.

*Bir sonraki yazımda Temel Linux Komutları hakkında bilgi vermeye devam edeceğim. Herkese iyi çalışmalar.*
