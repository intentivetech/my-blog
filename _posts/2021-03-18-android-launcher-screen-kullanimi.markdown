---
layout: post
title:  "Android Launcher Screen Kullanımı - Splash Screen Kullanmanın Doğru Yolu"
date:   2021-03-18 17:04:22 +0300
categories: android 
sidebar: []
tags: android launcher
---

Herkese merhabalar, bu yazımda sizlere nasıl doğru şekilde splash screen oluşturabileceğimizi göstereceğim.

Splash Screen, genellikle uygulama açıldığında görünen ilk başlangıç ​​ekranıdır. Başka bir deyişle, şirket logosunu, adını, reklam içeriğini vb. görüntülemek için kullanılan basit bir ekrandır. Güzel tasarlanmış bir splash screen uygulamanızı daha profesyonel gösterir. Kullanıcı uygulamayı başlattığında activitynin onCreate metodunun çağırılması arasında biraz gecikme olabilir. Bu esnada pencere yöneticisi <code>windowBackground</code> gibi uygulama temasından ögeler kullanarak arayüz çizmeye çalışır. windowBackground, default olarak beyazdır, bu yüzden karşımıza beyaz ekran çıkar.


