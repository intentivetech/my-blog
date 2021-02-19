---
layout: post
title:  "Android Uygulama İçerisinde Animasyon Kullanımı"
date:   2021-02-16 16:45:22 +0300
categories: android 
sidebar: []
tags: android animation
---

Uygulama içerisinde daha modern bir görünüm için animasyon kullanılabilmektedir. İçerisinde hazır animasyonlar bulunan ve bunların Android içerisinde kullanılabilmesini sağlayan Lottie'den bahsedeceğim. Lottiede yer alan ücretsiz animasyonları [buradan](https://lottiefiles.com/featured) inceleyebilirsiniz. 

## Proje İçerisinde Kullanımı

Lottie kütüphanesinin kullanımı çok basittir. Proje içerisinden <code>build.gradle</code> dosyasını açarak aşağıdaki satırları ekleyin.

```
def lottieVersion = "3.6.1" 
implementation "com.airbnb.android:lottie:$lottieVersion"
```

Lottie kütüphanesinin güncel sürümünü [en son sürümü ile](https://search.maven.org/artifact/com.airbnb.android/lottie) değiştirebilirsiniz.

Animasyonu kullanacağınız layout sayfası içerisine LottieAnimationView eklemek gerekmektedir. LottieAnimationView, animasyonların görüntülenmesini sağlar.

```xml
<com.airbnb.lottie.LottieAnimationView
     android:layout_width="wrap_content"
     android:layout_height="wrap_content"
    app:lottie_rawRes="RAW_FILE"
     app:lottie_autoPlay="true" 
     app:lottie_loop="true"/>
```     

Android içerisinde animasyon görüntülemek için, görüntülemek istediğiniz animasyonun proje içerisinde yer alması gerekmektedir. Animasyon dosyalarının barındırılacağı raw klasörünü oluşturmak için **Res -> New -> Android Resource Directory** seçeneklerine tıklayarak Resource Type kısmını raw olarak değiştirmek gerekmektedir. LottieFiles web sitesinden beğendiğiniz animasyonu JSON olarak indirin ve dosya adını istediğiniz şekilde değiştirin. 

![Lottie JSON](https://i.ibb.co/VQ1htCS/santa.png)

İndirdiğiniz dosyayı Android Studio içerisinde oluşturduğunuz raw klasörüne taşıyın. 

Animasyonu kullanmak için kodu düzenleyelim:

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#f6e8d8"
    tools:context=".MainActivity">

    <com.airbnb.lottie.LottieAnimationView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:lottie_autoPlay="true"
        app:lottie_loop="true"
        app:lottie_rawRes="@raw/santa" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

<img src="https://s2.gifyu.com/images/ezgif.com-gif-maker-3051ff8aa0fd96046.gif" width="500">

Lottie animasyonlarına sahip ilk Android projeniz kullanıma hazır. İyi çalışmalar 🙌

