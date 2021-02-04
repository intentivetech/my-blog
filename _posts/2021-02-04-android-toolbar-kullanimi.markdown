---
layout: post
title:  "Android Toolbar Kullanimi"
date:   2021-02-04 13:29:22 +0300
categories: android 
sidebar: []
tags: android toolbar
---

Merhabalar, bu yazımda sizlere toolbar oluşturmayı ve oluşturduğumuz toolbar içerisine menü elemanlarının nasıl eklendiğinden bahsedeceğim.

[Toolbar]("https://i.ibb.co/2ybn8nS/toolbar1.png")

Öncelikle toolbarı ekleyeceğimiz activityi oluşturalım.

**ToolbarActivity.java**
```
public class ToolbarActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_toolbar);
    }
}
```

Toolbarı kullanmak için varsayılan olarak gelen action barı devre dışı bırakmamız gerekiyor. Bunun için manifest dosyası içerisinde **android:theme="@style/AppTheme.NoActionBar"** satırını ekleyelim.

**AndroidManifest.xml**
```
<activity android:name=".ToolbarActivity"
    android:theme="@style/AppTheme.NoActionBar">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
```

NoActionBar temasını kullanabilmek için styles dosyası içerisinde tanımlamamız gerekmektedir.

**styles.xml**
```
<resources>
    <style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
    </style>

    <style name="AppTheme.NoActionBar">
        <item name="windowActionBar">false</item>
        <item name="windowNoTitle">true</item>
    </style>
</resources>
```

Bu işlemleri gerçekleştirdikten sonra toolbarımızı oluşturabiliriz.

**toolbar.xml**

```
<com.google.android.material.appbar.AppBarLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_marginBottom="20dp"
    android:theme="@style/AppTheme">

    <androidx.appcompat.widget.Toolbar
        android:id="@+id/toolbar"
        android:layout_width="match_parent"
        android:layout_height="?attr/actionBarSize"
        android:background="?attr/colorPrimary">

    </androidx.appcompat.widget.Toolbar>

</com.google.android.material.appbar.AppBarLayout>
```

Buradaki bileşenlerin değerini kendi istediğiniz şekilde değiştirebilirsiniz. Oluşturduğumuz toolbarı activity_toolbar.xml dosyası içerisine include edelim.

**activity_toolbar.xml**
```
<?xml version="1.0" encoding="utf-8"?>
<androidx.coordinatorlayout.widget.CoordinatorLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <include
        layout="@layout/toolbar" />
</androidx.coordinatorlayout.widget.CoordinatorLayout>
```

Bu adımları gerçekleştirdikten sonra activity sınıfımız içerisinde action barın yerine oluşturduğumuz toolbarın kullanılacağını belirtmemiz gerekmektedir.

**ToolbarActivity.java**
```
public class ToolbarActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_toolbar);
        Toolbar toolbar = (Toolbar) findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);
        setTitle("Toolbar Example");
    }
}
```

Basit bir toolbar oluşturduk. Fakat henüz istediğimiz görüntüyü elde edemedik.

[Toolbar2]("https://i.ibb.co/3Crzqzb/toolbar2.png")

Oluşturduğumuz toolbar içerisine menü ekleyelim. Menü eklemek için: **res -> Android resource file -> Resource type : Menu** seçeneğini seçerek dosyamıza bir isim verelim. Oluşturduğumuz dosya içerisine menü elemanlarını ekleyelim.

**toolbar_menu.xml**
```
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <item android:id="@+id/action_search"
        android:icon="@drawable/ic_search"
        android:title="Search"
        app:showAsAction="always" />

    <item android:id="@+id/action_like"
        android:icon="@drawable/ic_like"
        android:title="@string/like"
        app:showAsAction="always"/>

    <group android:checkableBehavior="single"
        android:id="@+id/settings">
         <item
              android:id="@+id/action_settings"
              android:title="@string/settings" />
    </group>
</menu>
```

**app:showAsAction-> menü** elemanının görünürlüğü ile ilgilidir. “always” elemanın her durumda görüneceği anlamına gelir. Örneğin “ifroom” seçersek menüde yer varsa gösterilir.

Oluşturduğumuz menüyü toolbar içerisine eklememiz için onCreateOptionsMenu() methodunu activity içerisine ekleyelim.

```
@Override
public boolean onCreateOptionsMenu(Menu menu) {
    getMenuInflater().inflate(R.menu.toolbar_menu, menu);
    return true;
}
```

Daha sonra hangi elemana tıklandığının kontrolünü yapabilmek için onOptionsItemSelected() methodunu ekleyelim.

```
@Override
public boolean onOptionsItemSelected(MenuItem item) {
    int id = item.getItemId();
    switch (id){
        case R.id.action_like:
            //like iconuna tıklandığında yapılacaklar
            break;
        case R.id.action_search:
            //search iconuna tıklandığında yapılacaklar
            break;
    }
    return super.onOptionsItemSelected(item);
}
```

**ToolbarActivity** dosyamızın son hali aşağıdaki gibi olacaktır.

```
public class ToolbarActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_toolbar);
        Toolbar toolbar = (Toolbar) findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);
        setTitle("Toolbar Example");
    }

    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.toolbar_menu, menu);
        return true;
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        int id = item.getItemId();
        switch (id){
            case R.id.action_like:
                //like iconuna tıklandığında yapılacaklar
                break;
            case R.id.action_search:
                //search iconuna tıklandığında yapılacaklar
                break;
        }
        return super.onOptionsItemSelected(item);
 }
 ```