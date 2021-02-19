---
layout: post
title:  "Android Emulatörün Ekran Görüntüsünü Alma"
date:   2021-02-04 12:19:22 +0300
categories: android 
sidebar: []
tags: android emulatör ekrangörüntüsü
---

Merhabalar, bir yazımda paylaşmak için cihazın ekran görüntüsünü alıp gife çevirmem gerekiyordu öğrendiklerimi sizinle de paylaşmak istedim.

Terminalde adb komutu kullanmamız için öncelikle Path içerisine platform-tools’u eklememiz gerekiyor. Bunun için Bilgisayarım’a sağ tıklayarak **Özellikler -> Gelişmiş Sistem Ayarları -> Ortam Değişkenleri -> Path -> Düzenle -> Yeni** adımlarını takip ederek platform-tools’un bulunduğu dizini belirtiyoruz. Benim bilgisayarımda **C:\Users\USERNAME\AppData\Local\Android\sdk\platform-tools** dizinindeydi. Artık terminalde adb komutunu çalıştırabiliriz, emulatörümüzün veya cihazımızın ekran görüntüsünü alabiliriz.

Ekran resmi almak için **adb shell screencap** komutunu kullanıyoruz.

```bat
adb shell screencap /sdcard/screen.png
```

Oluşturduğumuz dosyaları Android Studio içerisinde bulunan Device Explorer panelinden sdcard klasörünün altında görebilirsiniz.
Aldığımız ekran görüntüsünü bilgisayarımıza indirmek için **adb pull** komutunu kullanıyoruz. Aramaya dosyanın adını yazarak hangi klasörde olduğunu bulabilirsiniz.

```bat
adb pull /sdcard/screen.png
```

Ekranı kaydetmek için **adb shell screenrecord** komutunu kullanıyoruz.

```bat
adb shell screenrecord /sdcard/foo.mp4
```

MP4 uzantılı dosyayı GIF’e çevirmek için **FFmpeg** aracını kullanacağız. FFmpeg, bir çok platformda kullanılabilen ses ve görüntü kaydı, dönüştürme, yayın işlemlerini gerçekleştirmesi sağlayan açık kaynak geliştirilen bir multimedya uygulamasıdır. FFmpeg indirmek ve kurulum adımları için [buradan](https://github.com/adaptlearning/adapt_authoring/wiki/Installing-FFmpeg) faydalanabilirsiniz.

ffmpeg -i komutunu kullanarak önce input dosyamızı daha sonra output dosyamızı ve formatını belirtiyoruz.

```bat
ffmpeg -i foo.mp4 foo.gif
```

**NOT:** No such file or directory hatasının oluşmaması için dosyamızın bulunduğu dizinde komutu çalıştırın yada dosyanın tam yolunu belirtiniz.

İyi Çalışmalar :)