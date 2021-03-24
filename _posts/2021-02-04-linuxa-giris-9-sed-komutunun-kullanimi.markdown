---
layout: post
title:  "Linux’a Giriş — 9 — Sed Komutunun Kullanımı"
date:   2021-02-04 11:02:22 +0300
categories: linux 
sidebar: []
tags: linux sed
---

# $ sed Komutu 

*sed* komutu, akış düzenleyicinin kısaltmasıdır ve dosya üzerinde arama, bulma ve değiştirme, ekleme veya silme gibi birçok işlevi gerçekleştirebilir. *sed* komutunun en yaygın kullanımı, değiştirme veya bulma ve değiştirme içindir. *sed*'i kullanarak dosyaları açmadan bile düzenleyebilirsiniz; bu, dosyadaki bir şeyi bulmanın ve değiştirmenin çok daha hızlı bir yoludur.

Bir dosya oluşturalım.

```bat
nano ornek.txt
```

Dosya içerisine aşağıdaki metni yapıştırın.

```
Fatih elmasuyu
Suzan portakalsuyu
Melih kavunsuyu
Melih kavunsuyu
Rasim kirazsuyu
Tarık portakalsuyu
Lale şeftalisuyu
Suzan portakalsuyu
Melih kayısısuyu
Ayşe mangosuyu
Galip havuçsuyu
Osman karpuzsuyu
Betül narsuyu
```

```bat
sed 's/u/Z/' ornek.txt
```

Metinde bulunan ‘u’ harflerini ‘Z’ harfi yapar yalnız aynı satırda birden fazla ‘u’ varsa sadece ilk bulduğunu değiştirir.

```bat
sed 's/portakalsuyu/limonata/g' ornek.txt
```

Metinleri değiştirme ‘s’ işleci ile gerçekleşir. Yukarıdaki örnekte portakalsuyu yerine limonata yazar. ‘g’ ise tüm satırda aynı işlemi uygulamaya yarar.

```bat
sed 's/u/Z/g' ornek.txt
```

'g' işleci kullanılarak aynı işlemin tüm satırda uygulanması sağlanabilir.

```bat
sed 's/^F/f/' ornek.txt
```

<code>^</code> karakteri satır başını ifade eder. F harfi ile başlayan kelimeleri f ile değiştir.

```bat
sed 's/$/SATIRSONU/' ornek.txt
```

<code>$</code> karakteri satır sonu anlamına gelir ve bu komut satır sonlarına SATIRSONU yazar.

```bat
sed 's/^$/BOSSATIR/' ornek.txt
```

<code>^$</code> boş satırları ifade eder. Yukarıdaki örnek boş satırlara BOSSATIR kelimesini yazar.

```bat
sed '/^$/d' ornek.txt
```

*d* işleci ile silme işlemleri gerçekleştirilir. Yukarıdaki örnek komut boş satırları silmeyi sağlar.

```bat
sed -e 's/Z/u/g' -e 's/e/B/g' ornek.txt
```

*-e* parametresi birden fazla işlem gerçekleştirmek için kullanılır.

```bat
sed -i 's/^/"/' ornek.txt
```

*-i* parametresi yaptığımız değişikliklerin dosyaya kaydolmasını sağlar. Bu örnek tüm satırların başına <code>"</code> koymayı sağlar.

