---
layout: post
title:  "Android — View Binding"
date:   2021-09-15 12:43:26 +0300
categories: android
sidebar: []
tags: kotlin programlama android
---

## View Binding

View binding, view ile etkileşime giren kodu daha kolay yazmanıza olanak tanıyan bir özelliktir. Bir modülde view binding etkinleştirildiğinde, o modülde bulunan her XML layout dosyası için bir binding sınıfı oluşturur. Böylece bu layout içerisinde bulunan viewlerin sahip oldukları ID üzerinden doğrudan erişim sağlanabilir. View bindingi etkinleştirdikten sonra <code>activity_mail.xml</code> adında bir layout oluşturduğunuzu varsayalım. Arkaplanda otomatik olarak <code>ActivityMainBinding.kt</code> dosyası oluşturulur. 

View binding, <code>findViewById</code>'nin yerini alır.

# Kurulum 

View bindingi etkinleştirmek için <code>build.gradle</code> dosyasında <code>viewBinding</code> build seçeneğini <code>true</code> olarak ayarlayın.

```kotlin
android {
    ...
    buildFeatures {
        viewBinding true
    }
}
```

Eğer binding sınıfının oluşmasını istemiyorsanız <code>tools:viewBindingIgnore="true"</code> niteliğini ekleyebilirsiniz.

```xml
<LinearLayout
        ...
        tools:viewBindingIgnore="true" >
    ...
</LinearLayout>
```

# Kullanım

Projede view binding etkinleştirilmişse, her layout dosyası için binding sınıfı oluşturulur. Her binding sınıfı, root view ve tüm viewlerin ID'sini içerir. 

Örneğin, <code>result_profile.xml</code> adında bir layout dosyası olduğunu varsayalım. 

```xml
<LinearLayout ... >
    <TextView android:id="@+id/name" />
    <ImageView android:cropToPadding="true" />
    <Button android:id="@+id/button"
        android:background="@drawable/rounded_button" />
</LinearLayout>
```

Oluşturulan binding sınıfına <code>ResultProfileBinding</code> adı verilir. Bu sınıfın iki view elemanı vardır: <code>name</code> adlı bir <code>TextView</code> ve <code>button</code> adlı bir <code>Button</code>. Layout içindeki <code>ImageView</code>'in idsi yoktur. Bu yüzden binding sınıfı içerisinde örneği bulunmamaktadır. 

Her binding sınıfı <code>getRoot()</code> metodunu içerir. <code>getRoot()</code> metodu, root viewin örneğini sağlar. 

# Activity İçerisinde Kullanımı

Bir activityde kullanmak üzere binding sınıfının bir örneğini ayarlamak için <code>onCreate()</code> methodunda aşağıdaki adımları gerçekleştirin:

1. Oluşturulan binding sınıfında bulunan statik <code>inflate()</code> methodunu çağırın. Bu, activitynin kullanması için binding sınıfının bir örneğini oluşturur.
2. <code>getRoot()</code> metodunu çağırarak root view örneği alın. 
3. Root viewi, ekranda etkin görünüm yapmak için <code>setContentView()</code> öğesine iletin.

```kotlin
private lateinit var binding: ResultProfileBinding

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    binding = ResultProfileBinding.inflate(layoutInflater)
    val view = binding.root
    setContentView(view)
}
```

Artık viewlerden herhangi birini kullanmak için binding sınıfının örneğini kullanabilirsiniz:

```kotlin
binding.name.text = viewModel.name
binding.button.setOnClickListener { viewModel.userClicked() }
```

# Fragment İçerisinde Kullanımı 

Bir fragmentta kullanmak üzere binding sınıfının bir örneğini ayarlamak için <code>onCreateView()</code> methodunda aşağıdaki adımları gerçekleştirin:

1. Oluşturulan binding sınıfında bulunan statik <code>inflate()</code> methodunu çağırın. Bu, fragmentın kullanması için binding sınıfının bir örneğini oluşturur.
2. <code>getRoot()</code> metodunu çağırarak root view örneği alın. 
3. <code>onCreateView()</code> metodundan root view döndürün.

```kotlin
private var _binding: ResultProfileBinding? = null
// This property is only valid between onCreateView and
// onDestroyView.
private val binding get() = _binding!!

override fun onCreateView(
    inflater: LayoutInflater,
    container: ViewGroup?,
    savedInstanceState: Bundle?
): View? {
    _binding = ResultProfileBinding.inflate(inflater, container, false)
    val view = binding.root
    return view
}

override fun onDestroyView() {
    super.onDestroyView()
    _binding = null
}
```

Artık viewlerden herhangi birini kullanmak için binding sınıfının örneğini kullanabilirsiniz:

```kotlin
binding.name.text = viewModel.name
binding.button.setOnClickListener { viewModel.userClicked() }
```

---
**NOT**

Fragmentlar viewlerinden daha uzun yaşar. Bu yüzden <code>onDestroyView()</code> metodunda binding sınıfının örneğini temizleyin.

---

# findViewById'den farklılıklar

View binding işleminin <code>findViewById</code> kullanımına göre önemli avantajları vardır:

- **Null safety:** View binding viewlerin doğrudan referanslarını oluşturduğu için, geçersiz bir id nedeniyle <code>NullPointerException</code> hatası alma riski yoktur. 
- **Type safety:** Her binding sınıfındaki alanlar, XML dosyasında yer alan viewlerle eşleşen türe sahiptir. Bu da <code>ClassCastException</code> alma riski olmadığı anlamına gelir. 

Bu farklılıklar, layout ve kodunuz arasındaki uyumsuzlukların, çalışma zamanında değil derleme zamanında başarısız olmasına neden olacağı anlamına gelir. Bu özelliği sayesinde uygulamanın hata fırlatıp kapanmasını engeller. 

# Data Binding ile Karşılaştırma

View Binding ve Data Binding, viewlere doğrudan başvurmak için kullanabileceğiniz binding sınıfları oluşturur. Bununla birlikte, view bindingin daha basit kullanım durumlarını ele alması amaçlanmıştır ve data bindinge göre aşağıdaki faydaları sağlar:

- **Daha hızlı derleme:** View binding, anotasyon işleme gerektirmez, bu nedenle derleme süreleri daha hızlıdır.
- **Kullanım Kolaylığı:** View binding, özel olarak etiketlenmiş XML layout dosyaları gerektirmez, bu nedenle uygulamalarınızda benimsemek daha hızlıdır.