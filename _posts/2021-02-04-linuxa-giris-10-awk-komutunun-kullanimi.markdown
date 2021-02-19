---
layout: post
title:  "Linux’a Giriş — 10 — Awk Komutunun Kullanımı"
date:   2021-02-04 11:38:22 +0300
categories: linux 
sidebar: []
tags: linux awk
---

# $ awk Komutu

Awk, verileri işlemek ve raporlar oluşturmak için kullanılan bir komut dosyası dilidir. Awk komut programlama dili, derleme gerektirmez ve kullanıcının değişkenleri, sayısal işlevleri, dizgi işlevlerini ve mantıksal işleçleri kullanmasına izin verir. 

Awk, bir programcının, bir belgenin her satırında aranacak metin kalıplarını ve bir eşleşme bulunduğunda yapılacak eylemi tanımlayan ifadeler biçiminde küçük ama etkili programlar yazmasını sağlayan bir yardımcı programdır.

# awk Komutu İle Neler Yapılabilir?

- Bir dosyayı satır satır tarar
- Her giriş satırını alanlara böler
- Giriş alanlarını modelle karşılaştırır
- Eşleşen hatlarda eylem gerçekleştirir

```bat
awk '{print $1}' ornek.txt
```

Bir önceki yazıda oluşturduğumuz <code>ornek.txt</code> dosyası için bu komutu yazarsak 1. sütunu yani isim listesini ekrana getirir. Varolan dosyayı değiştirerek aşağıdaki şekilde düzenleyin.

```
Fatih elmasuyu 5 7
Suzan portakalsuyu 3 9
Melih kavunsuyu 2 5
Melih kavunsuyu 7 2
Rasim kirazsuyu 8 4
Tarık portakalsuyu 4 5
Lale şeftalisuyu 5 2
Suzan portakalsuyu 1 8
Melih kayısısuyu 2 5
Ayşe mangosuyu 6 3
Galip havuçsuyu 7 8
Osman karpuzsuyu 3 5
Betül narsuyu 5 1
```

```bat
awk '/kavunsuyu/ {print}' ornek.txt 
```

Yukarıdaki örnekte awk komutu kavunsuyu kelimesi ile eşlesen satırları gösterir.

```bat
awk '{print $1,$4}' ornek.txt 
```

Yukarıdaki örnekte $1 isimleri, $4 ise 4. satırdaki verileri temsil eder.

```bat
awk '{sonuc=$3*$4; print $0 ,sonuc}' ornek.txt
```

Yukarıdaki örnek komut 3. ve 4. satırdaki değerleri çarparak bir sonuc adında bir değişkene atar, ekrana hem tüm dosya içeriğini hemde sonuc değişkeninin değerlerini yazar.

```bat
awk '{print NR,$0}' ornek.txt 
```

Yukarıdaki örnekte, NR ile awk komutu satır numarasıyla birlikte tüm satırları yazdırır.

