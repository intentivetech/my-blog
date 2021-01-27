---
layout: post
title:  "Linux’a Giriş — 1 — Temel Linux Komutları"
date:   2021-01-27 14:57:26 +0300
categories: linux
---


Merhaba, bu yazımda sizlere Linux ile ilgili bilmemiz gereken temel kavramlardan ve terminalde kullandığımız temel komutlardan bahsedeceğim.

![Linux](https://i.ibb.co/zrm5BGg/linux.png)

*Linux, Unix’e fikirsel ve teknik anlamda atıfta bulunarak geliştirilmiş; açık kaynak kodlu, özgür ve ücretsiz bir işletim sistemi çekirdeğidir. İster Sanal Makine üzerine isterseniz Ana Makinenize kurabilirsiniz.*


[Virtual Box’a nasıl kurulur?](http://yapbenzet.kocaeli.edu.tr/virtualbox-ubuntukurulumu/) [Windowsun yanına Centos Kurulumu](https://tr.compozi.com/how-install-centos-7-alongside-windows-10-dual-boot)


### Terminal Nedir?

Terminal, bir kullanıcının komutlar yazarak bilgisayarla iletişime geçtiği ortamdır. Dağıtımlara göre bazı komutlar farklılık gösterebilir.

## Temel Linux Komutları

# $ ls komutu

ls komutu bulunduğumuz dizin içerisindeki dosya ve klasörleri listeler.

![ls komutu](https://i.ibb.co/6BBgS3b/ls.png)

```
ls
```
Basitçe kullanımı yukarıdaki şekildedir. Ayrıntılı şekilde listeleme işlemini farklı parametreler ile gerçekleştirebiliriz.

```
ls -l
```

-l parametresi dosyaların boyut tarih bilgilerini göstererek listeler. 


**NOT:** Bir komuttan sonra - ile başlayan bir komut kullanımı parametre olarak adlandırılır. Burada ls komutunun l parametresini gördük.

```
ls -a
```

-a parametresi gizli dosyalar ve dizinler ile birlikte listeleme işlemi yapar.


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

mkdir komutu dizin oluşturmak için kullanılır. Dizini windowstaki klasörler olarak düşünebilirsiniz. Dosya ile farkı ise dosyalarının .txt, .jpg, .mp3 gibi uzantılarının olmasıdır.

```
mkdir DİZİN_ADI
```

mkdir komutunun basitçe kullanımı yukarıdaki gibidir. ls komutunu kullanarak dizinin oluşturulduğunu görebilirsiniz.

```
mkdir DİZİN1 DİZİN2
```
Aynı anda birden fazla dizin oluşturmak için yan yana yazabiliriz. 


# $ rm komutu

Dosya veya dizin silmemizi sağlayan komuttur. Örnek kullanımı aşağıdaki gibidir.

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


# $ cd komutu

Dizinler arası geçiş yapmayı sağlayan komuttur.

```
cd dizin_adi
```

Basitçe kullanımı yukarıdaki gibidir.


**Örneğin;** bir test dizininizin içerisinde dizin1 ve dizin2 adında iki tane dizininiz olsun. *cd dizin1* komutu ile dizin1 ‘e geçtiğinizde, daha sonra tekrar dizin2'ye geçmek için *cd dizin2* komutunu çalıştırdığınızda hata alacaksınız. Bunun nedeni dizin1’in içerisindeyken dizin2'ye geçmeye çalışmanızdır.


Sadece cd komutunu çalıştırmak sizi home dizinine yönlendirecektir. Daha sonra home dizininden daha detaylıca bahsedeceğim. Şuanlık kodlarımızı yazarken burda olduğumuzu bilelim. ~ işaretinden anlayabilirsiniz.

```
cd ../dizin2
```
Buradaki .. bir üst dizini belirtir. Bu şekilde dizin2'ye ulaşmış oluruz.


# $ mv komutu

mv komutu dosya ismini değiştirmeyi ve dosyayı farklı bir dizine taşımayı sağlamaktadır.

```
mv dosya1 yenidosyaadi
```
Bu şekilde kullandığımızda varolan dosya1 isimli dosyayının adını yenidosyaadi yapar.

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

Bir dosyayı kopyalamaya yarar. 

```
cp dosya1 kopya
```
Basitçe kullanımı yukarıda görülmektedir. 


Farklı bir dizine kopyalama işlemi:

```
cp dosya1 dizin2/kopya
```
Örnek olarak dizin2'nin içerisine kopyaladım, başka bir yere de kopyalayabilirsiniz.


### Diğer Linux Komutları 

Linux’un en önemli komutu man komutudur.

```
man komut
```

Bilgi edinmek istenen komutun nasıl kullanıldığı ve komutun ne işe yaradığı gibi bilgilerin olduğunu gösterir. Komutun sahip olduğu tüm parametreleri ve anlamlarını açıklar. Bu komut sayesinde Linux hakkında hiçbir bilgisi olmayan kişiler rahatlıkla Linux kullanabilir.

```
uptime
```
Sisteme ne kadar süredir bağlı olduğumuzu gösterir.

```
reboot
```
Reset atar. Ctrl+R kısayoludur.

```
clear
```
Terminal ekranımızı temizler. Kısayolu Ctrl+l ‘dir.

```
echo
```
Ekrana yazı yazdırmaya yarar. Örneğin “echo test” yazdığımızda ekrana “test” yazar.

```
echo $SHELL
```
Hangi terminalde olduğumuzu gösterir.

```
pwd
```
Bulunduğumuz dizin bilgisini verir.