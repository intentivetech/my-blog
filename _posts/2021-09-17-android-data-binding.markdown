---
layout: post
title:  "Android — Data Binding"
date:   2021-09-17 11:07:26 +0300
categories: android
sidebar: []
tags: kotlin programlama android
---

Data binding kütüphanesi, UI bileşenlerini uygulamanızdaki veri kaynaklarına bağlamanıza olanak tanıyan bir kütüphanedir. 

Aşağıdaki kod, bir TextViewi, <code>viewModel</code> nesnesinin <code>userName</code> özelliğine bağlamak için <code>findViewById()</code> öğesini çağırır:

```kotlin
findViewById<TextView>(R.id.sample_text).apply {
    text = viewModel.userName
}
```

Aşağıdaki örnek, doğrudan layout dosyasında <code>TextView</code>e değer atamak için data binding kütüphanesinin nasıl kullanılacağını gösterir.

```xml
<TextView
    android:text="@{viewmodel.userName}" />
```

Layout dosyası içerisinde bileşenleri bağlamak, activity içerisinde bir çok UI framework kullanımı azaltır ve bu bileşenlerin kullanımı kolay hale getirir. Ayrıca uygulamanın performansını iyileştirir ve bellek sızıntılarını ortadan kaldırır. 

# Kullanımı

Uygulamanızı data binding kullanacak şekilde yapılandırmak için, uygulama modülündeki <code>build.gradle</code> dosyanızdaki <code>dataBinding</code> derleme seçeneğini aşağıdaki örnekte gösterildiği gibi etkinleştirin:

```kotlin
android {
    ...
    buildFeatures {
        dataBinding true
    }
}
```

Data binding kütüphanesi, layout içinde bulunan viewleri data nesnelerinizle bağlamak için gereken sınıfları otomatik olarak oluşturur. Data binding layout dosyaları biraz farklıdır ve <code>layout</code> tagi ile başlar. Ardından <code>data</code> elemanı ve <code>view</code> root elemanı gelir. Aşağıdaki kod, örnek bir layout dosyasını gösterir:

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android">
   <data>
       <variable name="user" type="com.example.User"/>
   </data>
   <LinearLayout
       android:orientation="vertical"
       android:layout_width="match_parent"
       android:layout_height="match_parent">
       <TextView android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:text="@{user.firstName}"/>
       <TextView android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:text="@{user.lastName}"/>
   </LinearLayout>
</layout>
```

<code>data</code> içindeki <code>user</code> değişkeni, bu layout içinde kullanılabilecek bir özelliği tanımlar.

```xml
<variable name="user" type="com.example.User" />
```

Layout içindeki ifadeler, <code>"@{}"</code> sözdizimi kullanılarak öznitelik özelliklerine yazılır. Burada <code>TextView</code> metni, <code>user</code> değişkeninin <code>firstName</code> özelliğine ayarlanır:

```xml
<TextView android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:text="@{user.firstName}" />
```

<code>User</code> adında data classımız olduğunu varsayalım.

```kotlin
data class User(val firstName: String, val lastName: String)
```

Her layout dosyası için bir binding sınıfı oluşturulur. Varsayılan olarak, sınıfın adı layout dosyasının adını temel alır ve sonuna <code>Binding</code> ekini ekler. Örneğin: <code>activity_main.xml</code> layout dosyası için oluşturulacak olan sınıf <code>ActivityMainBinding</code>'tir. 
Binding oluşturmak için önerilen yöntem:

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    val binding: ActivityMainBinding = DataBindingUtil.setContentView(
            this, R.layout.activity_main)

    binding.user = User("Test", "User")
}
```

Alternatif olarak, aşağıdaki örnekte gösterildiği gibi bir <code>LayoutInflater</code> kullanarak görünümü elde edebilirsiniz:

```kotlin
val binding: ActivityMainBinding = ActivityMainBinding.inflate(getLayoutInflater())
```

Fragment, ListView veya RecyclerView içerisinde data binding kullanıyorsanız, aşağıdaki kod örneğinde gösterildiği gibi binding sınıflarının veya <code>DataBindingUtil</code> sınıfının <code>inflate()</code> yöntemlerini kullanmayı tercih edebilirsiniz:

```kotlin
val listItemBinding = ListItemBinding.inflate(layoutInflater, viewGroup, false)
// yada
val listItemBinding = DataBindingUtil.inflate(layoutInflater, R.layout.list_item, viewGroup, false)
```

Layout içerisinde bazı operatörler kullanılabilir. Örneğin: 

```xml
android:text="@{String.valueOf(index + 1)}"
android:visibility="@{age > 13 ? View.GONE : View.VISIBLE}"
android:transitionName='@{"image_" + id}'
```

## Databinding Event Handling

DataBinding kullanırken Event Handling olayını iki şekilde yapabiliriz.

1- Method References kullanarak

2- Listener Bindings kullanarak

# Method References

Bir click olayını activity içerisinde yazmak yerine direkt olarak layout içerisinde bind edebiliriz. Bunu yapabilmek için <code>android:onClick</code> kullanılır. Bu kullanımın en büyük avantajı derleme zamanında işlenmesi, yani belirtilen metod tanımlanmamışsa derleme zamanında hata alınmasıdır. Click işlemini handle eden bir sınıf oluşturalım:

```kotlin
class MyHandlers {
    fun onClickFriend(view: View) { ... }
}
```

<code>onClickFriend()</code> metodu textView view'ine <code>android:onClick</code> kullanarak bağlanır.  

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android">
   <data>
       <variable name="handlers" type="com.example.MyHandlers"/>
       <variable name="user" type="com.example.User"/>
   </data>
   <LinearLayout
       android:orientation="vertical"
       android:layout_width="match_parent"
       android:layout_height="match_parent">
       <TextView android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:text="@{user.firstName}"
           android:onClick="@{handlers::onClickFriend}"/>
   </LinearLayout>
</layout>
```

# Listener Binding

Metod referansı bağlama ile listener bağlama arasındaki en önemli fark metod bağlama event trigger edildiğinde değil veri bağlama aşamasında oluşmasıdır. Listener bağlama,  bir olay gerçekleştiğinde çalışan bağlama ifadeleridir. Örneğin, <code>onSaveClick()</code> yöntemine sahip olan aşağıdaki <code>Presenter</code> sınıfını düşünün:

```kotlin
class Presenter {
    fun onSaveClick(task: Task){}
}
```

Ardından, click olayını <code>onSaveClick()</code> metoduna aşağıdaki gibi bağlayabilirsiniz:

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android">
    <data>
        <variable name="task" type="com.android.example.Task" />
        <variable name="presenter" type="com.android.example.Presenter" />
    </data>
    <LinearLayout android:layout_width="match_parent" android:layout_height="match_parent">
        <Button android:layout_width="wrap_content" android:layout_height="wrap_content"
        android:onClick="@{() -> presenter.onSaveClick(task)}" />
    </LinearLayout>
</layout>
```

Yukarıdaki örnekte, onClick(View) öğesine iletilen <code>view</code> parametresini tanımlamadık. Listener bağlamaları, listener parametreleri için iki seçenek sunar: metodun tüm parametrelerini yok sayabilir veya hepsini adlandırabilirsiniz. Parametreleri adlandırmayı tercih ederseniz, bunları ifadenizde kullanabilirsiniz. Örneğin, yukarıdaki ifade aşağıdaki gibi yazılabilir:

```xml
android:onClick="@{(view) -> presenter.onSaveClick(task)}"
```

Veya ifadedeki parametreyi kullanmak isterseniz Presenter sınıfını aşağıdaki gibi düzenleyebilirsiniz:

```kotlin
class Presenter {
    fun onSaveClick(view: View, task: Task){}
}
```

```xml
android:onClick="@{(theView) -> presenter.onSaveClick(theView, task)}"
```

## Imports, variables, and includes

Data binding kütüphanesi, import, variable ve include gibi özellikler sağlar. Import, layout dosyalarınızın içindeki sınıflara başvurmayı kolaylaştırır. Variable, binding ifadelerinde kullanılabilecek bir özelliği tanımlamanıza olanak tanır. Include, uygulamanız genelinde karmaşık View'leri yeniden kullanmanıza olanak tanır.

```xml
<data>
    <import type="android.view.View"/>
</data>
``` 

# Import

Import, layout dosyası içerisinde sınıfları kullanabilmeyi sağlar. <code>data</code> tagi içerisinde <code>import</code> 
anahtar kelimesi ile kullanılır. Aşağıdaki kod örneği, <code>View</code> sınıfını layout dosyasına aktarır:

```xml
<data>
    <import type="android.view.View"/>
</data>
```

<code>View</code> sınıfını import etmek, binding ifadelerinde kullanabilmemizi sağlar. Aşağıdaki örnek, <code>View</code> sınıfının <code>VISIBLE</code> ve <code>GONE</code> sabitlerinin nasıl kullanıldığını gösterir: 

```xml
<TextView
   android:text="@{user.lastName}"
   android:layout_width="wrap_content"
   android:layout_height="wrap_content"
   android:visibility="@{user.isAdult ? View.VISIBLE : View.GONE}"/>
```

# Variable 

<code>data</code> öğesinin içinde birden çok değişken kullanılabilir. Aşağıdaki örnek, <code>user</code>, <code>image</code> ve <code>note</code> değişkenlerini gösterir:

```xml
<data>
    <import type="android.graphics.drawable.Drawable"/>
    <variable name="user" type="com.example.User"/>
    <variable name="image" type="Drawable"/>
    <variable name="note" type="String"/>
</data>
```

# Include

Değişkenler, include edilen bir layout dosyasına bind edilebilir.

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:bind="http://schemas.android.com/apk/res-auto">
   <data>
       <variable name="user" type="com.example.User"/>
   </data>
   <LinearLayout
       android:orientation="vertical"
       android:layout_width="match_parent"
       android:layout_height="match_parent">
       <include layout="@layout/name"
           bind:user="@{user}"/>
       <include layout="@layout/contact"
           bind:user="@{user}"/>
   </LinearLayout>
</layout>
```

