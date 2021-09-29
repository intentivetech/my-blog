---
layout: post
title:  "Android — Farklı Ekran Boyutları İle Çalışma"
date:   2021-09-02 12:11:26 +0300
categories: android
sidebar: []
tags: kotlin programlama android
---

Android, telefonlardan tabletlere ve televizyonlara kadar birçok farklı cihaz türünde çalışacak şekilde tasarlanmıştır. Uygulamanızın tüm bu cihazlarda başarılı olabilmesi için farklı ekran konfigürasyonlarına uyum sağlayan esnek bir UI sağlaması gerekir. Mümkün olduğu kadar çok ekran boyutu destekleyerek daha fazla kullanıcıya ulaşılabilir.

# ConstraintLayout 

Uygulamanın farklı ekran boyutlarını desteklemesi için ekran boyutundaki küçük değişikliklere yanıt verebilen bir düzen oluşturmak gerekir. Farklı ekran boyutları için düzen oluşturmanın en iyi yolu <code>ConstraintLayout</code> kullanmaktır. <code>ConstraintLayout</code>, widget'ları esnek bir şekilde konumlandırmanıza ve boyutlandırmanıza olanak tanıyan bir layout'tur. İç içe view grupları kullanmadan karmaşık yapıları düz bir hiyerarşide oluşturmamızı sağlar. <code>RelativeLayout</code>'a benzer fakat kullanımı daha kolaydır.

ConstraintLayoutta bir görünümün konumunu tanımlamak için, görünüme en az bir yatay ve bir dikey constraint eklemelisiniz. Her constraint, başka bir view bileşenine bağlantıyı veya hizalamayı temsil eder.

![constraint layout](https://i.ibb.co/xGR875k/constraint-layout-constrain-left.gif)

# Sabit Boyutlardan Kaçının

Layout düzeninizin esnek olduğundan ve farklı ekran boyutlarına uyum sağladığından emin olmak için, çoğu görünüm bileşeninin genişliği ve yüksekliği için sabit kodlanmış boyutlar yerine <code>wrap_content</code> veya <code>match_parent</code> kullanmalısınız.

<code>wrap_content</code>, view bileşeninin boyutunun içerdiği değer kadar olmasını sağlar.
<code>match_parent</code>, view bileşeninin mümkün olduğunca genişlemesini sağlar.

Örneğin; 

```xml
<TextView
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:text="@string/lorem_ipsum" />
```

![size](https://developer.android.com/images/screens_support/layout-match-parent_2x.png)

# Alternatif Layout Oluşturma

Telefon için tasarladığınız arayüz tablette aynı şekilde görünmeyebilir. Bu yüzden, uygulamanın belirli ekran boyutları için farklı layoutlar sağlamalıdır. Farklı bir arayüz gerektiren her ekran yapılandırması için bir tane olmak üzere ek <code>res/layout/</code> dizinleri oluşturarak ekrana özel arayüzler sağlayabilir ve ardından layout dizini adına bir ekran yapılandırma niteleyicisi ekleyebilirsiniz (600dp'ye sahip ekranlar için layout-w600dp gibi).

Alternatif layout oluşturma adımları:

1. Layout dosyasını açarak toolbarda bulunan <code>Orientation for Preview</code> seçeneğine tıklayın.
2. Listelenen seçeneklerden <code>Create Landscape Variant</code> veya <code>Create Other</code> seçeneğine tıklayın. 
3. Eğer <code>Create Other</code> seçeneğine tıkladıysanız, <code>Select Resource Directory</code> penceresi görünür. Burada, solda bulunan seçeneklerden bir ekran niteleyici seçin ve bunu seçilmiş niteleyiciler listesine ekleyin. Niteleyici eklemeyi tamamladığınızda Tamam'ı tıklayın.

Bu şekilde 2. bir layout dosyası oluşur, oluşan bu dosya içerisinde düzenlemeler yapabilirsiniz.

![alternative layout](https://i.stack.imgur.com/qNm2Q.png)

# Smallest Width

"smallest width" ekran boyutu niteleyicisi, minimum genişliğe sahip ekranlar için alternatif layoutlar oluşturmamıza olanak tanır. Bu özellik sayesinde cihazın sabit genişliğinin en küçük değeri belirtilerek, belirtilen ekranlarda bu hazırlamış olduğumuz ekranlarda belirlemiş olduğumuz resourcelerin kullanılmasını sağlar. Ayarlarından sadece ekran genişliğinin en azı kaç olacaksa onu yazabiliriz. Ekranın yatay yada dikey olması bu genişlik değerini etkilemez. 

<code>res/layout/klasör_ismi-swboyutdp</code> şeklinde dizin oluşturulur. Örneğin, dizinlerde dosyanın farklı sürümlerini aşağıdaki gibi oluşturarak el cihazları ve tabletler için optimize edilmiş main_activity adlı bir düzen oluşturabilirsiniz:

```
res/layout/main_activity.xml           # 600dp'den küçük el cihazları için
res/layout-sw600dp/main_activity.xml   # 600dp'den büyük tabletler için
```

Smallest width, ekranın iki kenarından en küçük olan kenarını belirtir. Diğer en küçük genişlik değerlerinin tipik ekran boyutlarına nasıl karşılık geldiği aşağıda açıklanmıştır:

- 320dp: tipik bir telefon ekranı (240x320 ldpi, 320x480 mdpi).
- 480dp: büyük telefon ekranı ~5" (480x800 mdpi).
- 600dp: 7” tablet (600x1024 mdpi).
- 720dp: 10 ”tablet (720x1280 mdpi, 800x1280 mdpi, vb.).

![smallest width](https://m.media-amazon.com/images/G/01/DeveloperBlogs/AmazonDeveloperBlogs/legacy/viv1._CB520506451_.png)

# Screen Width, Screen Height

 Bu özellik sayesinde cihazın genişliğinin ve/veya yüksekliğinin en az olması gereken değeri belirtilerek, belirtilen ekranlarda bu hazırlamış olduğumuz ekranlarda belirlemiş olduğumuz resourcelerin kullanılmasını sağlar. Ekran yön değiştirdiğinde değişen ekran genişliği ve/veya yüksekliğine göre bu ekrana geçiş sağlanabilir. Ayarlarından sadece ekran genişliğinin/yüksekliğinin en az kaç olacaksa onu yazabiliriz. Mesela bir tablet için 600 dp en küçük değerdir. Bu özellik sayesinde belli bir boyuttan büyük ekranlarda iki ekran gibi özellikler kullanılabilir. Ekran yatay yada dikey pozisyonda iken genişlik ve yükseklik değeri değişir. Diğer özelliklerde olduğu gibi bu özellikleride ister tek başına istek başka özelliklerle birlikte kullanabiliriz. <code>res/layout/klasör_ismi-wboyutdp</code> veya <code>res/layout/klasör_ismi-hboyutdp</code> şeklinde oluşur. 

```
res/layout/main_activity.xml        
res/layout-w600dp/main_activity.xml  
res/layout-h600dp/main_activity.xml                                
```

# Orientation 

Yalnızca "smallest width" ve "available width (screen width, screen height)" niteleyicilerinin kombinasyonlarını kullanarak tüm boyut çeşitlerini destekleyebilmenize rağmen, kullanıcı dikey ve yatay yönler arasında geçiş yaptığında kullanıcı deneyimini de değiştirmek isteyebilirsiniz. Bunun için dizin adına <code>port</code> veya <code>land</code> anahtar kelimelerini ekleyebilirsiniz.

```
res/layout/main_activity.xml                
res/layout-land/main_activity.xml           
res/layout-sw600dp/main_activity.xml        
res/layout-sw600dp-land/main_activity.xml   
```

- Portrait : Cihaz dikey pozisyondayken çalışır
- Landspace : Cihaz yatay pozisyondayken çalışır

