---
layout: post
title:  "Android — Observable Data Objeleri ile Çalışma"
date:   2021-09-22 14:03:26 +0300
categories: android
sidebar: []
tags: kotlin programlama android
---

Gözlenebilirlik(Observability), bir nesnedeki değişiklikler hakkında diğerlerini bilgilendirmeyi sağlar. Data Binding kütüphanesi objeleri, fieldları ve collectionları gözlemlenebilir hale getirmeyi sağlar. 

Data binding, veri nesnelerinize, verileri değiştiğinde listener olarak bilinen diğer nesneleri bildirme yeteneği vermek için kullanılabilir. Üç farklı türde gözlemlenebilir sınıf vardır: objeler, fieldlar ve collectionlar. Bu gözlemlenebilir veri nesnelerinden biri UI'a bağlandığında ve veri nesnesinin bir özelliği değiştiğinde, UI otomatik olarak güncellenir.

# Observable fields

Eğer oluşturduğunuz sınıf sadece bir kaç özelliğe sahipse bu durumda özellikleri gözlemlenebilir hale getirmek için <code>Observable Fields</code> kullanılabilir.

```kotlin
class User {
    val firstName = ObservableField<String>()
    val lastName = ObservableField<String>()
    val age = ObservableInt()
}
```

Field değerine erişmek için <code>get()</code> ve <code>set()</code> kullanılır.

```kotlin
user.firstName = "Google"
val age = user.age
```

# Observable collections

Bazı uygulamalar, verileri tutmak için dinamik yapılar kullanır. Observable collectionlar, bir key kullanarak bu yapılara erişim sağlar. <code>ObservableArrayMap</code> sınıfı, key, aşağıdaki örnekte gösterildiği gibi, <code>String</code> gibi bir değer olduğunda kullanışlıdır:

```kotlin
ObservableArrayMap<String, Any>().apply {
    put("firstName", "Google")
    put("lastName", "Inc.")
    put("age", 17)
}
```

Layout içerisinde, map keyler aracılığıyla kullanılabilir:

```xml
<data>
    <import type="android.databinding.ObservableMap"/>
    <variable name="user" type="ObservableMap&lt;String, Object&gt;"/>
</data>
…
<TextView
    android:text="@{user.lastName}"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"/>
<TextView
    android:text="@{String.valueOf(1 + (Integer)user.age)}"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"/>
```

# Observable objects

Observable interfaceini uygulayan bir sınıf, observable object üzerindeki özellik değişikliklerinden haberdar olmak isteyen listenerların kaydına izin verir.
