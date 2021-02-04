---
layout: post
title:  "Kotlin Encrypted Shared Preferences Kullanımı"
date:   2021-02-04 13:47:22 +0300
categories: android 
sidebar: []
tags: android kotlin security 
---

Android uygulamalarımızda veriyi depolamak ve gerektiğinde kullanabilmemiz için pek çok farklı yöntemler bulunmaktadır. Bu yöntemlerden biride SharedPreferences’dir. SharedPreferences, uygulama içerisinde key-value şeklinde veri depolamamıza yarar ve kullanımı oldukça basittir. Fakat kötü niyetli kişilerin de bu verileri okuması oldukça basittir. Bu nedenle SharedPreferences içerisinde hangi verilerin depolanacağına ve bu verilerin şifrelenmesine dikkat etmek gerekir. EncryptedSharedPreferences burada devreye girer. Basit bir örnek yapalım böylece farkı daha iyi görebilirsiniz.


Androidx Security kütüphanesini kullanabilmek için build.gradle dosyası içerisine aşağıdaki satırı ekleyelim.

```
implementation "androidx.security:security-crypto:1.1.0-alpha02"
```

Verilerinizi şifrelemeye geçmeden önce, bir şifreleme anahtarı oluşturmak gerekir. Bu anahtara Master Key denir ve Android Keystore içerisinde AES şifreleme kullanılarak depolanmaktadır.

```
val masterKey = MasterKey.Builder(applicationContext, MasterKey.DEFAULT_MASTER_KEY_ALIAS).setKeyScheme(MasterKey.KeyScheme.AES256_GCM).build()
```

Master Key oluşturmak ve kullanabilmek için **AES256_GCM_SPEC** adlı varsayılan bir anahtar oluşturma özelliği verilmiştir.

```
val sharedPreferences = EncryptedSharedPreferences.create(applicationContext, PREFS_NAME, masterKey, EncryptedSharedPreferences.PrefKeyEncryptionScheme.AES256_SIV, EncryptedSharedPreferences.PrefValueEncryptionScheme.AES256_GCM)
```

Daha sonra EncryptedSharedPreferences örneği oluşturmamız gerekir. Bunun için Context, daha önce oluşturduğumuz Master Key, Preference Adı ve key-value değerlerine ihtiyacımız vardır. Son iki parametre key-value değerlerinin şifrelenmesi içindir ve kütüphane tarafından sunulan tek seçenek bunlardır.

EncryptedSharedPreferences örneğini oluşturduktan sonra, verileri saklamak ve kullanabilmek için SharedPreferences gibi kullanabiliriz.

Örnek olarak bir test verisi saklayalım ve veriyi görüntüleyelim.

```
sharedPreferences.edit().putString("test","test").apply()
val data = sharedPreferences.getString("test","")
Log.d("ShowData", data)
```

Kodun son hali aşağıdaki gibi olacaktır.

```
val masterKey = MasterKey.Builder(applicationContext,MasterKey. DEFAULT_MASTER_KEY_ALIAS) 
        .setKeyScheme(MasterKey.KeyScheme.AES256_GCM) 
        .build() 
val sharedPreferences = EncryptedSharedPreferences.create(
        applicationContext, 
        PREFS_NAME, 
        masterKey, 
        EncryptedSharedPreferences.PrefKeyEncryptionScheme. AES256_SIV, 
        EncryptedSharedPreferences.PrefValueEncryptionScheme . AES256_GCM) 
sharedPreferences.edit().putString("test", "test").apply() 
val data = sharedPreferences.getString("test", "") 
Log.d("ShowData",data)
```

Verimizin şifrelendiğini anlayabilmek için Shared Preferences dosyasının içerisine göz atalım. **Devices File Exprolerer -> data -> data -> uygulamanızın paket adı -> shared_prefs** klasörünün altında XML dosyasını görüntüleyebilirsiniz.

Normal SharedPreferences kullansaydık karşılaşacağımız XML çıktısı aşağıdaki gibi olacaktı.

[Encrypted1](https://i.ibb.co/b2gYTQ5/encryp1.png)

Fakat EncryptedSharedPreferences kullanıldığında hem key değeri hem de value değeri şifrelenmiş olur. Key-value verilerinin şifrelendiğini ve depolandığını görebilirsiniz.

[Encrypted2](https://i.ibb.co/MG1RQxV/encryp2.png)

Key-value değerleri için keyset bulunmaktadır. Bu keysetler, Shared Preferences verilerini şifrelemek ve şifreleri çözmek için kullanılan kriptografik anahtarları içerir. Daha önce oluşturduğumuz Master Key, bu keysetleri şifrelemek için kullanılır.

Android’in SharedPreferences özelliği, verileri depolamak için oldukça kullanışlıdır. Fakat bu veriler hassas olduğunda şifrelemek iyi bir çözüm olacaktır. İyi çalışmalar dilerim..

