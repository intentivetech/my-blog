---
layout: post
title:  "Linux’a Giriş — 4 — Dosya ve Dizin İzinleri"
date:   2021-01-29 09:01:22 +0300
categories: linux
sidebar: []
---

Linux gibi Unix benzeri işletim sistemleri, yalnızca çoklu görev değil aynı zamanda çok kullanıcılı olmaları bakımından diğer bilgi işlem sistemlerinden farklıdır. Bu tam olarak ne anlama geliyor? Bu, bilgisayarı aynı anda birden fazla kullanıcının çalıştırabileceği anlamına gelir. Örneğin, bilgisayar bir ağa veya internete bağlıysa, uzak kullanıcılar ssh aracılığıyla oturum açabilir ve bilgisayarı çalıştırabilir.

Bir kullanıcının eylemlerinin bilgisayarı çökertmesini istemeyiz ve bir kullanıcının başka bir kullanıcıya ait dosyalara müdahale etmesine izin vermek istemeyiz. Bunun için her dosya ve dizine dosyanın sahibi veya ilgili bir kullanıcı grubunun üyeleri için erişim izinleri atanmalıdır. 

Bir dizinin içerisinde ls -l komutunu kullandığımızda en solda **-rwxrwxrwx** şeklinde harfler görürüz.Bunları inceleyelim.

![Linux Dosya İzinleri](https://i.ibb.co/yQbRYDw/file-permissions.png)

 En solda bulunan - dosyamızın türünü belirtir. - normal dosya (txt,jpg,png gibi) anlamına gelir. Linux'ta bir çok dosya türü bulunmaktadır. Bunlar aşağıdaki gibidir: 

- **b:** block (/dev dizininde görülür)
- **c:** character (/dev dizininde görülür)
- **l:** link
- **d:** directory 

Peki rwx nedir bunu inceleyelim. Bir Linux sisteminde, her dosya ve dizine dosyanın sahibi, ilgili bir kullanıcı grubunun üyeleri ve diğer herkes için erişim izinleri atanır. Bir dosyayı okumak, bir dosya yazmak ve bir dosyayı yürütmek (yani, dosyayı bir program olarak çalıştırmak) için haklar atanabilir.

- **r:** read (dosyayı okuma izni)
- **w:** write (dosyaya yazma izni)
- **x:** execute (dosyayı program olarak çalıştırma izni)

Örnek olarak etc/passwd dosyasını inceleyelim. *ls -al* komutunu kullanarak dosyaların izinlerini ayrıntılı olarak inceleyelim.

![Dosya hakkında bilgi](https://i.ibb.co/7YLtccK/terminal.png)

Dosyanın türünden sonraki ilk üç tanesi dosyanın sahibinin (user) izinlerini belirtir. Dosya sahibinden sonraki üçlü kullanıcı grubunun izinlerini belirtir. Daha sonraki üçlü de diğer kullanıcıların (others) izinleridir. 

Dosyanın izinlerini incelediğimizde çıkarılacak bilgiler:

- etc/passwd dosyası *root* kullanısına aittir. 
- Dosyanın sahibi *root* kullanıcı grubuna aittir.
- Dosyanın sahibinin sadece dosyayı okuma ve yazma izni vardır. 
- *root* grubunun üyeleri bu dosyayı sadece okuyabilir. 
- Bu dosyayı başkaları sadece okuyabilir.

Dosyanın sahibinin izinlerini inceleyelim. Görüldüğü üzere dosyanın sahibinin yetkileri **rw-**'tir. Buradaki - dosyanın sahibinin bu dosyayı çalıştırmaya izni olmadığı anlamına gelir. Yada diğer kullanıcıların izinlerini incelediğinizde (**r- -**) sadece okuma izni olduğunu görebilirsiniz. Elbette bu izinleri değiştirmek mümkün. Şimdi gelin dosya izinlerini nasıl değiştirebileceğimizi öğrenelim.

# Dosya İzinlerini Değiştirme

![Dosya izinlerini değiştirme](https://i.ibb.co/hKcTT74/permissions.png)








