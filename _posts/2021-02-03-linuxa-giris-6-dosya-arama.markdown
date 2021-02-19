---
layout: post
title:  "Linux’a Giriş — 6 — Find Komutu"
date:   2021-02-03 10:13:22 +0300
categories: linux 
sidebar: []
tags: linux dosyaarama find
---

Kullanıcıların bir Linux makinesiyle ilk uğraşırken karşılaştıkları bir sorun, aradıkları dosyaların nasıl bulunacağıdır. *find* komutu sistem yöneticileri için güçlü araçlardan biridir. Dosya ve dizinleri izinlerine, türlerine, tarihlerine, sahipliklerine, boyutlarına ve daha fazlasına göre aramak için find komutunu kullanabilirsiniz.


Genel kullanımı aşağıdaki gibidir: 

```bat
find [yol] [aranacak-dosya-yada-dizin] [yapılacak-eylem]
```

- **yol:** dizini belirtir
- **aranacak-dosya-yada-dizin:** Aranacak dosyanın veya dizinin adı
- **yapılacak-eylem:** kopyalama, silme, taşıma vb.

### Dosyaları veya Dizinleri Arama

# Ada veya Uzantıya Göre Dosyaları Arama

```bat
find -name deneme
```

Bir dosyayı ada göre bulmak için *-name* kullanılır.

```bat
find -name 'deneme*'
```

Adı deneme ile başlayan dosyaları bulur.

```bat
find -name '*deneme*'
```

Adında deneme kelimesi geçen dosyaları bulur.

![Linux](https://i.ibb.co/J7wyLnm/finddeneme.png)

```bat
find -iname deneme
```

*-iname* dosyayı ada göre bulur fakat büyük küçük harf kullanımını göz ardı eder. (ignore case)

![Linux](https://i.ibb.co/J7wyLnm/finddeneme.png)

# Dosya Türüne Göre Arama

*-type* parametresiyle bulmak istenen dosyanın türü belirtilir. Dosya türünü belirtmek için kullanılan en yaygın tanımlayıcılardan bazıları şunlardır: 

- **f:** normal dosya 
- **d:** dizin 
- **l:** sembolik link 
- **c:** karakter cihazlar
- **b:** blok cihazlar

```bat
find / -type f -name '*.conf'
```

.conf uzantısı ile biten tüm dosyaları bu şekilde bulabilirsiniz.

```bat
find /etc -type f -name 'a*'
```

etc dizininde türü dosya olan ve a ile başlayan dosyaları görüntüler.

```bat
find /etc -type d -iname '*a'
```

etc dizininde türü dizin olan ve a ile biten dosyaları görüntüler.

```bat
find /etc -type f -iname '*.conf' | xargs tar -czf conf.tar.gz
```

etc dizinindeki .conf uzantılı dosyaları hem arşivler hemde sıkıştırır.

```bat
find /etc -type f -iname '*.conf' >> /tmp/conf
```

etc dizinindeki .conf uzantılı dosyaları tmp dizininde conf adında dosyaya yazar.

# Boyuta Göre Dosyaları ve Dizinleri Arama

Belirli bir boyuta eşit veya daha büyük, belirli bir aralıkta veya boş olan tüm dosyaları veya dizinleri bulabilirsiniz. Aradığınız dosya veya dizin türüne bağlı olarak uygun boyut biçimini kullanın.

- **c:** byte
- **k:** kilobyte
- **m:** megabyte
- **g:** gigabyte

```bat
find / -size 30M
```

Boyutu 30M olan tüm dosyaları bulur.

```bat
find -size +1M
```

Boyutu 1MB'den büyük tüm dosyaları bulur.

```bat
find . -type f -size -10M
```

Geçerli dizinde boyutu 10M'dan az olan dosyaları bulur.

```bat
find / -size +100M -size -200M
```

Boyutu 100M ve 200M arasındaki dosyaları bulur.

# Boş Dosyaları Arama

```bat
find ./ -type f -size 0
```

yada 

```bat
find -empty
```

Boş dosyaları bulur.

# İzinlere Göre Arama

Genel kullanımı: 

```bat
find -perm mod
```

- **mod:** Sayısal gösterim yada sembolik karakter gösterimleri kullanılmaktadır. (644, 655, 700, 777, u=x, a=r+x gibi.)

```bat
find -perm 777
```

Tüm kullanıcıların tam yetki ile işlem yapabileceği dosyaları listeler.

```bat
find -perm -766
```

Belirtilen iznin önündeki *-* girilen iznin alt sınır olduğunu belirtir. 

