---
layout: post
title:  "Linux’a Giriş — 4 — Dosya ve Dizin İzinleri"
date:   2021-01-29 09:01:22 +0300
categories: linux 
sidebar: []
tags: linux izinler
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

## Dosya İzinlerini Değiştirme

Bir dosya veya dizinin izin ayarlarını değiştirmek için **chmod** komutu kullanılır. 

Basit kullanımı bu şekildedir: 

```
chmod izin dosyaadı
```

İzni tanımlamanın iki yolu vardır: 

1. Sembolik karakterleri kullanma 
2. Sayısal gösterim yöntemini kullanma


# Sembolik Karakterleri Kullanma

Sembolik karakterleri kullanarak izin ayarlarını belirtmek için, kullanıcı **(u)**, grup **(g)** ve diğer kullanıcılar **(o)** için erişilebilirliği tanımlamanız gerekir.

Dosyaya herkes için okuma, yazma, çalıştırma yetkisi vermek için:

```
chmod u=rwx,g=rwx,o=rwx dosyaadı
```

Örneğin test.txt dosyasının izinlerini belirtilen şekilde ayarlayalım.

- Dosyanın sahibi için okuma ve yazma izni
- Kullanıcı grupları için okuma izni
- Diğer kullanıcılar için okuma izni

```
chmod u=rw,g=r,o=r test.txt
```


# Sayısal Gösterim Yöntemini Kullanma

İzni belirtmenin başka bir yolu, sayısal gösterim yöntemini kullanmaktır. Bu seçenek önceki yöntem kadar basit olmasa da daha hızlıdır.

![Dosya izinlerini değiştirme](https://i.ibb.co/hKcTT74/permissions.png)

```
rwx rwx rwx = 111 111 111
rw- rw- rw- = 110 110 110
rwx --- --- = 111 000 000

rwx = 111 = 7
rw- = 110 = 6
r-x = 101 = 5
r-- = 100 = 4
```

Her kategori (kullanıcı, grup, sahip) için izin tanımlamanız gerektiğinden, komut üç sayı içerecektir. Örneğin, chmod u = rw, g = r, o = r test.txt komutu ile sembolik olarak yapılandırdığımız test.txt dosyasına bakalım. 

Aynı izin ayarları, aşağıdaki komutla sayısal gösterim yöntemi kullanılarak tanımlanabilir:

```
chmod 644 test.txt
```

![Sayısal Gösterim](https://i.ibb.co/XYh1jkY/say-salgosterim.jpg)

## Dosya ve Grup Sahipliğini Değiştirme

Dosya izinlerini değiştirmenin yanı sıra, kullanıcı dosya sahipliğini veya hatta grup sahipliğini değiştirmeyi gerektiren bir durumla karşılaşabilirsiniz. Dosya veya dizin sahipliğini değiştirebilmek için *root* kullanıcısı olmak gerekir. *root* kullanıcısı olabilmek için çalıştırdığınız komutların başına *sudo* komutunu yazmanız gerekir. 

# Dosya Sahipliğini Değiştirme 

*chown*, change owner kelimelerinin kısaltılmasıdır ve dosya sahibini değiştirmek için kullanılır. Kullanımı aşağıdaki gibidir: 

```
chown kullanıcı dosyaadı
```

Örneğin test.txt dosyasının sahibini *firstuser* kullanıcı olarak atayalım.

```
sudo chown firstuser test.txt
```

# Grup Sahipliğini Değiştirme

*chgrp* change group kelimelerinin kısaltılmasıdır ve grup sahipliğini değiştirmek için kullanılır. Kullanımı aşağıdaki gibidir: 

```
chgrp grupadı dosyaadı
```

Örneğin test.txt dosyasının grup sahipliğini *firstgroup* grubu olarak atayalım.

```
sudo chgrp firstgroup test.txt
```






