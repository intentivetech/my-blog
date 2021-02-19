---
layout: post
title:  "Linux’a Giriş — 8 — Crontab Kullanımı"
date:   2021-02-03 17:45:22 +0300
categories: linux 
sidebar: []
tags: linux crontab
---


Cron, görevleri belirli aralıklarla yürüten bir zamanlama arka plan programıdır. Çoğunlukla sistem bakımını veya yönetimini otomatikleştirmek için kullanılır. Örneğin, veritabanlarını veya verileri yedeklemek, sistemi en son güvenlik yamalarıyla güncellemek, disk alanı kullanımını kontrol etmek, e-posta göndermek gibi tekrar eden görevleri otomatikleştirmek için bir cron işi ayarlayabilirsiniz. Cron işleri bir dakika, saat, ayın günü, ay, haftanın günü veya bunların herhangi bir kombinasyonu ile çalışacak şekilde planlanabilir.

Crontab formatı: 

```
* * * * * komut
- - - - -
| | | | |
| | | | ----- Haftanın bir günü (0 - 7) (Sunday=0 or 7)
| | | ------- Ay (1 - 12)
| | --------- Ayın bir günü (1-31)
| ----------- Saat (0 - 23)
------------- Dakika (0 - 59)
```

*, birimleri belirtir. Örneğin; 2 * * * * her 2 dakika anlamına gelir.

Crontab işlemleri için *crontab* komutu kullanılır.

```bat
crontab -e
```

*-e* parametresi crontab dosyasını düzenlemeyi sağlar. Örneğin; 2 dakikada bir tmp dizinine dosya oluşturan komut girelim.

```bat
2  *  *  *  * touch /tmp/deneme
```

Amacımız 2 dakikada bir çalıştırmak olduğu için dakika birimine 2 yazdık, haftanın her günü, her ay, her saat çalışacağı için diğer kısımlara * koyduk. 

Haftasonları sabah 7 akşam 9 arası çalışacak crontab

```bat
0  7-21  *  *  6-7 komut
``` 

Her 10 dakikada bir çalışan crontab

```bat
*/10  *  *  *  * komut
```

Çarşamba günleri ve her ayın 6. gününde saat 15:45 de çalışan crontab

```bat
45  15  6  *  3 komut
```