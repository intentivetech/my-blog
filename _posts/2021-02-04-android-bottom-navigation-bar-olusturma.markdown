---
layout: post
title:  "Android Bottom Navigation Bar Oluşturma"
date:   2021-02-04 12:08:22 +0300
categories: android 
sidebar: []
tags: android bottomnavigationbar
---

Merhaba, Android Uygulamarı içerisinde basit bir alt menün nasıl oluşturulduğunu adım adım göstereceğim. Bottom Navigation Bar denilen alt menü elemanlarının 3‘ten az ve 5’ten fazla olmaması gerekir.

[Bottom Navigation](https://i.ibb.co/8bSdrvn/bottomnav1.png)

Android Studio içerisinde aşağıdaki resimde gösterilen adımları takip ederek eklersek alt menü bizim için otomatik olarak oluşturulacaktır. Bizde Android Studio'nun bize sunduğu bu özellikleri kullanarak işlemleri gerçekleştirelim. MainActivity adında bir Bottom Navigation Activity oluşturalım.

[Bottom Navigation](https://i.ibb.co/cFwvWqw/bottomnav2.png)

## Fragment Oluşturma

Fragmentler Activityler gibi kullanıcı arayüzlerini oluştururlar. Fragmentler activitylere göre daha hızlıdır ve yeni bir activity çağırmaktan daha performanslıdır. Fragment’i kullanabilmek için öncelikle bir activitye sahip olmamız gerekir. Daha sonra bu activity üzerinden değiştirilmelidir.

Projemize fragments adında paket ekleyelim. Paket eklemek için **New -> Package** seçeneğine tıklayarak paket adımızı girelim. İşlem yapacağımız sınıfları daha kolay bulabilmemiz için paketleri kullanmak gerekir. Şimdi paketin içerisine fragment ekleyebiliriz. Resimde görüldüğü gibi fragment oluşturmak için paket adının üzerine sağ tıklayarak **New -> Fragment -> Fragment(Blank)** seçeneğine tıklayarak fragment oluşturabiliriz. Her menü için ayrı fragment oluşturmamız gerekir. Bu örnekte 3 tane menü olduğu için üç tane fragment oluşturalım. (HomeFragment, ProfileFragment, AccountFragment)

[Bottom Navigation](https://i.ibb.co/Zfgnfx2/bottomnav3.png)

Her fragment aşağıdaki kodu içerecektir.

```
public class HomeFragment extends Fragment{

    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
        return inflater.inflate(R.layout.fragment_home, container, false);
    }
}
```

Şimdi menüye tıklayınca fragmentleri değiştireceğiz. Ayrıca uygulama başlatıldığında default olarak HomeFragment’in yüklenmesini sağlayacağız.
Default fragmenti çalıştırmak için **loadFragment()** fonksiyonu oluşturalım. Fonksiyon parametre olarak bir fragment nesnesi alır.

```
private boolean loadFragment(Fragment fragment) {
    if (fragment != null) {
        getSupportFragmentManager()
                .beginTransaction()
                .replace(R.id.fragment_container, fragment)
                .commit();
        return true;
    }
    return false;
}
```

Default fragmenti başlatmak için activitynin **onCreate()** methodu içerisinde **loadFragment()** fonksiyonunu çağırıp, HomeFragment nesnesini parametre olarak gönderiyoruz.

```
private boolean loadFragment(Fragment fragment) {
        if (fragment != null) {
            getSupportFragmentManager()
                    .beginTransaction()
                    .replace(R.id.fragment_container, fragment)
                    .commit();
            return true;
        }
        return false;
}
```

BottomNavigationView bileşenini tanımlayarak tıklama olayını tespit etmek **OnNavigationItemSelectedListener** interface’ini kullanıyoruz. **setItemIconTintList(null)** iconun orjinal rengini kullanmamızı sağlar.

```
@Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        loadFragment(new HomeFragment());
        BottomNavigationView navView = findViewById(R.id.nav_view);
      navView.setOnNavigationItemSelectedListener(mOnNavigationItemSelectedListener);
        navView.setItemIconTintList(null);
}
```

**onNavigationItemSelected()** methodu menüye tıklandığında çalışacaktır ve tıklanan butona göre fragmentler çağrılacaktır.

```
private BottomNavigationView.OnNavigationItemSelectedListener mOnNavigationItemSelectedListener
            = new BottomNavigationView.OnNavigationItemSelectedListener() {

        @Override
        public boolean onNavigationItemSelected(@NonNull MenuItem item) {
            Fragment fragment = null;
            switch (item.getItemId()) {
                case R.id.navigation_home:
                    fragment = new HomeFragment();
                    break;
                case R.id.navigation_dashboard:
                    fragment = new NotificationsFragment();
                    break;
                case R.id.navigation_notifications:
                    fragment = new AccountFragment();
                    break;
            }
            return loadFragment(fragment);
        }
};
```

Yukarıdaki kod bloklarının tamamını içeren **MainActivity.java**
```
public class MainActivity extends AppCompatActivity {

    private BottomNavigationView.OnNavigationItemSelectedListener mOnNavigationItemSelectedListener
            = new BottomNavigationView.OnNavigationItemSelectedListener() {

        @Override
        public boolean onNavigationItemSelected(@NonNull MenuItem item) {

            Fragment fragment = null;

            switch (item.getItemId()) {
                case R.id.navigation_home:
                    fragment = new HomeFragment();
                    break;
                case R.id.navigation_dashboard:
                    fragment = new NotificationsFragment();
                    break;
                case R.id.navigation_notifications:
                    fragment = new AccountFragment();
                    break;
            }
            return loadFragment(fragment);
        }
    };

    private boolean loadFragment(Fragment fragment) {
        if (fragment != null) {
            getSupportFragmentManager()
                    .beginTransaction()
                    .replace(R.id.fragment_container, fragment)
                    .commit();
            return true;
        }
        return false;
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        loadFragment(new HomeFragment());
        BottomNavigationView navView = findViewById(R.id.nav_view);
        navView.setOnNavigationItemSelectedListener(mOnNavigationItemSelectedListener);
        navView.setItemIconTintList(null);
    }
}
```

**strings.xml** dosyası içerisinde aşağıdaki dizeleri tanımlayalım.

```
<resources>
    <string name="app_name">Bottom Navigation Example</string>
    <string name="title_home">Anasayfa</string>
    <string name="title_notifications">Bildirimler</string>
    <string name="title_account">Profilim</string>
</resources>
```

Res klasörünün altında bir menu klasörü oluşturmalı ve içerisine bottom_nav_menu isimli bir menü oluşturmalıyız.

**bottom_nav_menu.xml**

```
    <?xml version="1.0" encoding="utf-8"?>
    <menu xmlns:android="http://schemas.android.com/apk/res/android">
        <item
            android:id="@+id/navigation_home"
            android:icon="@drawable/home_selector"
            android:title="@string/title_home" />
        <item
            android:id="@+id/navigation_notifications"
            android:icon="@drawable/notification_selector"
            android:title="@string/title_notifications" />
        <item
            android:id="@+id/navigation_dashboard"
            android:icon="@drawable/account_selector"
            android:title="@string/title_account" />
    </menu>
```

Drawable klasörünün içerisinde home_selector, notification_selector, account_selector isimlerinde 3 tane xml dosyası oluşturalım. Selector tanımlamamızın amacı farklı durumlarda farklı ikonlar göstermektir. Örneğin: menüye tıklandığında seçili olan menünün ikonunu değiştirmek gibi.

**home_selector.xml**

```
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@drawable/home_outline" android:state_checked="false"/>
    <item android:drawable="@drawable/home" android:state_checked="true"/>
</selector>
```

**Not:** Diğer xml dosyalarınıda aynı şekilde oluşturalım.

activity_main.xml dosyasının içerisini aşağıdaki şekilde değiştirelim ve menüyü dahil edelim. (app:menu=”@menu/bottom_nav_menu”)

```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/container"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".activities.MainActivity">

    <FrameLayout
        android:id="@+id/fragment_container"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_marginBottom="56dp"
        android:text="@string/title_home"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <com.google.android.material.bottomnavigation.BottomNavigationView
        android:id="@+id/nav_view"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:background="?android:attr/windowBackground"
        app:itemTextColor="#363535"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:menu="@menu/bottom_nav_menu" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

Artık projemizi çalıştırabiliriz :)

[Bottom Navigation](https://i.ibb.co/dQS2bBf/bottomnav4.gif)