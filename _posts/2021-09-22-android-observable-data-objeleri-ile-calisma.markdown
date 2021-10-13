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

Eğer oluşturduğunuz sınıf sadece bir kaç özelliğe sahipse bu durumda özellikleri gözlemlenebilir hale getirmek için <code>Observable Fields</code> kullanılabilir. Observable fields tek bir alana sahip gözlemlenebilir nesnelerdir. Bu mekanizmayı kullanmak için kotlinde sadece okunabilir(read-only) bir özellik oluşturulur: 

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

Observable interfaceinde listener eklemek ve çıkarmak için bir mekanizma vardır. Geliştirmeyi kolaylaştırmak için Data Binding Library, listener kayıt mekanizmasını uygulayan <code>BaseObservable</code> sınıfını sağlar. BaseObservable'ı uygulayan data sınıfı, özellikler değiştiğinde bildirmekten sorumludur. Bu, aşağıdaki örnekte gösterildiği gibi, Bindable getter kullanılarak ve </code>notifyPropertyChanged()</code> metodu set edilerek kullanılır.

```kotlin
class User : BaseObservable() {

    @get:Bindable
    var firstName: String = ""
        set(value) {
            field = value
            notifyPropertyChanged(BR.firstName)
        }

    @get:Bindable
    var lastName: String = ""
        set(value) {
            field = value
            notifyPropertyChanged(BR.lastName)
        }
}
```

Data binding, veri bağlama işlemi için BR adlı bir sınıf oluşturur. <code>Bindable</code> anotasyonu, derleme sırasında BR sınıfı dosyasında bir giriş oluşturur. Data sınıfları için için base sınıf değiştirilemiyorsa, <code>Observable</code> interface, bir <code>PropertyChangeRegistry</code> nesnesi kullanılarak kaydedilmesi ve dinleyicilere verimli bir şekilde bildirilmesi için uygulanabilir. 

# Lifecycle-aware objects

Uygulamanızdaki layoutlar, verilerdeki değişiklikler hakkında UI'yi otomatik olarak bilgilendiren data binding kaynaklarına da bağlanabilir. Bu şekilde, bindingler yaşam döngüsüne duyarlıdır ve yalnızca UI ekranda göründüğünde tetiklenir. Data Binding şu anda <code>StateFlow</code> ve <code>LiveData</code>'yı desteklemektedir.

# StateFlow Kullanımı

Uygulamanız Kotlin coroutines kullanıyorsa, data binding kaynağı olarak <code>StateFlow</code> nesnelerini kullanabilirsiniz. Bir <code>StateFlow</code> nesnesini binding sınıfınızla kullanmak için StateFlow nesnesinin scopeunu tanımlamak üzere bir yaşam döngüsü sahibi belirtmeniz gerekir. Aşağıdaki örnek, binding sınıfı örneklendirildikten sonra activityi yaşam döngüsü sahibi olarak belirtir:

```kotlin
class ViewModelActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        // Inflate view and obtain an instance of the binding class.
        val binding: UserBinding = DataBindingUtil.setContentView(this, R.layout.user)

        // Specify the current activity as the lifecycle owner.
        binding.lifecycleOwner = this
    }
}
```

Data Binding, ViewModel nesneleriyle sorunsuz bir şekilde çalışır. <code>StateFlow</code> ve <code>ViewModel</code>'i aşağıdaki gibi birlikte kullanabilirsiniz:

```kotlin
class ScheduleViewModel : ViewModel() {

    private val _username = MutableStateFlow<String>("")
    val username: StateFlow<String> = _username

    init {
        viewModelScope.launch {
            _username.value = Repository.loadUserName()
        }
    }
}
```

Layout içerisinde aşağıdaki örnekte gösterildiği gibi, binding ifadelerini kullanarak ViewModel nesnenizin özelliklerini ve yöntemlerini karşılık gelen görünümlere atayın:


```xml
<TextView
    android:id="@+id/name"
    android:text="@{viewmodel.username}" />
```

Kullanıcı adı değeri değiştiğinde UI otomatik olarak güncellenir.