---
layout: post
title:  "Android Google Places API Place Autocomplete Kullanımı"
date:   2021-02-04 13:30:22 +0300
categories: android 
sidebar: []
tags: android googleplacesapi autocomplate
---

Merhabalar, bu yazımda sizlere Place Autocomplete API’yi projemize nasıl dahil edebileceğimizden bahsedeceğim.

![Google Places Api](https://i.ibb.co/3TKqgCb/googleplaces1.png)

Place Autocomplete, otomatik olarak mekan adı tamamlamayı sağlar. Kullanıcı herhangi bir arama yapmaya başladığı anda yerlerin bir listesini sunar.

Place Autocomplete’i uygulamamıza dahil etmek için iki seçenek bulunmaktadır:

1. AutocompleteSupportFragment kullanarak eklenebilir.
2. AutoComplete Intent kullanarak eklenebilir.

Öncelikle AutocompleteSupportFragment’ın nasıl kullanılacağına bakalım.

**NOT:** *Google Cloud Platform’undan Places API’yi etkinleştirip API Key oluşturmanız gerekmektedir. API Key’i nasıl oluşturacağınızı bilmiyorsanız daha önceki yazımdan faydalanabilirsiniz.*

Key aldıktan ve Places API’yi etkinleştirdikten sonra values klasörü altında google_maps_api.xml adında bir dosya oluşturalım. Oluşturduğumuz dosya içerisine keyi ekleyelim.

**google_maps_api.xml**

```xml
<resources>
    <string name="google_maps_key" templateMergeStrategy="preserve" translatable="false">API_KEYINIZI_YAZINIZ</string>
</resources>
```

Manifest dosyası içerisinde bu keyi belirtmemiz gerekiyor.

**AndroidManifest.xml**

```xml
<meta-data
    android:name="com.google.android.geo.API_KEY"
    android:value="@string/google_maps_key" />
```

Bir Activity oluşturalım. Places API’yi başlatmak için aşağıdaki satırları activityimize ekleyelim. Her iki yöntemde de Place API’yi kesinlikle başlatmamız gerekmektedir.

```java
String apiKey = getString(R.string.google_maps_key);
if (!Places.isInitialized()) {
    Places.initialize(getApplicationContext(), apiKey);
}
PlacesClient placesClient = Places.createClient(this);
```

## 1- AutocompleteSupportFragment Kullanımı

Oluşturduğumuz activitynin layoutuna AutocompleteSupportFragmenti ekleyelim.

**activity_autocomplete_example.xml**
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".AutocompleteActivity">

    <fragment
        android:id="@+id/autocomplete_fragment"
        android:name="com.google.android.libraries.places.widget.AutocompleteSupportFragment"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.0"
        tools:layout_editor_absoluteX="16dp" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

Oluşturduğumuz AutocompleteSupportFragmentin bir nesnesini oluşturarak bir PlaceSelectionListener ekleyelim. PlaceSelectionListener seçtiğimiz yer ile ilgili bize yanıt döndürür.

```java
//autocompletefragment nesnesi oluşturur
AutocompleteSupportFragment autocompleteFragment = (AutocompleteSupportFragment)
        getSupportFragmentManager().findFragmentById(R.id.autocomplete_fragment);
//döndürülecek yer verisi türlerini belirtir
autocompleteFragment.setPlaceFields(Arrays.asList(Place.Field.ID, Place.Field.NAME));

//selected olayı gerçekleştiğinde kod bloğu çalıştırmaya yarar
autocompleteFragment.setOnPlaceSelectedListener(new PlaceSelectionListener() {
    
    //selected işlemi gerçekleşince çalışacak kod bloğu
    @Override
    public void onPlaceSelected(Place place) {
        Toast.makeText(getApplicationContext(), "Place: " + place.getName() + ", " + place.getId(),Toast.LENGTH_LONG).show();
    }
    //hata oluştuğunda çalışacak kod bloğu
    @Override
    public void onError(Status status) {
        Toast.makeText(getApplicationContext(),"An error occurred: " + status,Toast.LENGTH_LONG).show();
    }
});
```

Bu adımları gerçekleştirdikten sonra activity sınıfının son hali aşağıdaki gibi olacaktır.

**AutocompleteExampleActivity.java**

```java
public class AutocompleteExampleActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_autocomplete_example);

        String apiKey = getString(R.string.google_maps_key);
        if (!Places.isInitialized()) {
            Places.initialize(getApplicationContext(), apiKey);
        }
        PlacesClient placesClient = Places.createClient(this);

        //autocompletefragment nesnesi oluşturur
        AutocompleteSupportFragment autocompleteFragment = (AutocompleteSupportFragment)
                getSupportFragmentManager().findFragmentById(R.id.autocomplete_fragment);
        //döndürülecek yer verisi türlerini belirtir
        autocompleteFragment.setPlaceFields(Arrays.asList(Place.Field.ID, Place.Field.NAME));

        //selected olayı gerçekleştiğinde kod bloğu çalıştırmaya yarar
        autocompleteFragment.setOnPlaceSelectedListener(new PlaceSelectionListener() {

            //selected işlemi gerçekleşince çalışacak kod bloğu
            @Override
            public void onPlaceSelected(Place place) {
                Toast.makeText(getApplicationContext(), "Place: " + place.getName() + ", " + place.getId(), Toast.LENGTH_LONG).show();
            }

            //hata oluştuğunda çalışacak kod bloğu
            @Override
            public void onError(Status status) {
                Toast.makeText(getApplicationContext(), "An error occurred: " + status, Toast.LENGTH_LONG).show();
            }
        });
   }
 }
 ```

 İşte bu kadar :)

![Google Places Api2](https://i.ibb.co/2nTWt7j/googleplaces2.png)

## 2- AutoComplete Intent Kullanımı

Bu işlemi gerçekleştirmek için öncelikle uygulamamızda bulunan toolbar içerisine search iconu ekleyelim. Search iconu eklemek için: **res -> Android resource file -> Resource type : Menu** seçeneğini seçerek dosyamıza bir isim verelim. Oluşturduğumuz dosya içerisine aşağıdaki satırları ekleyelim.

**toolbar_menu.xml**
```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <item
        android:id="@+id/search"
        android:icon="@drawable/ic_search"
        android:title="@string/action_search"
        app:showAsAction="always" />
</menu>
```

Oluşturduğumuz menüyü toolbar içerisine eklememiz için onCreateOptionsMenu() methodunu activity içerisine ekleyelim.

```java
@Override
public boolean onCreateOptionsMenu(Menu menu) {
    getMenuInflater().inflate(R.menu.toolbar_menu, menu);
    return true;
}
```

Daha sonra hangi elemana tıklandığının kontrolünü yapabilmek için onOptionsItemSelected() methodunu ekleyelim.

```java
@Override
public boolean onOptionsItemSelected(MenuItem item) {
    switch (item.getItemId()) {
        case R.id.search:
            onSearchCalled();
            return true;
        default:
            return false;
    }
}
```

Search iconuna tıklandığında Autocomplete.IntentBuilder’ı başlatmak için onSearchCalled() methodunu oluşturalım. startActivityForResult() methodu activityler arası geçiş yapılmasını sağlar.

```java
public void onSearchCalled() {
    //döndürülecek yer verisi türlerini belirtir
    List<Place.Field> fields = Arrays.asList(Place.Field.ID, Place.Field.NAME);

    // autocomplete intentini başlatır
    Intent intent = new Autocomplete.IntentBuilder(
            AutocompleteActivityMode.FULLSCREEN, fields)
            .build(this);
    startActivityForResult(intent, AUTOCOMPLETE_REQUEST_CODE);
}
```

Activityler arası geçiş yapıldıktan sonra yeni activityde bulunan bilginin önceki activitye iletilmesi için onActivityResult() methodu kullanılır.

```java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    if (requestCode == AUTOCOMPLETE_REQUEST_CODE) {
        //başarılı sonuç döndürüldüğünde çalışacak kod bloğu
        if (resultCode == RESULT_OK) {
            Place place = Autocomplete.getPlaceFromIntent(data);
            Toast.makeText(getApplicationContext(), "Place: " + place.getName() + ", " + place.getId(), Toast.LENGTH_LONG).show();
        }
        //hata oluştuğunda çalışacak kod bloğu
        else if (resultCode == AutocompleteActivity.RESULT_ERROR) {
            Status status = Autocomplete.getStatusFromIntent(data);
            Toast.makeText(getApplicationContext(), status.getStatusMessage(), Toast.LENGTH_LONG).show();
        } 
    }
}
```

Bu adımları da gerçekleştirdikten sonra activity sınıfının son hali aşağıdaki gibi olacaktır.

**AutocompleteExampleActivity.java**
```java
public class AutocompleteExampleActivity extends AppCompatActivity {

    private static final int AUTOCOMPLETE_REQUEST_CODE = 1;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_autocomplete_example);

        String apiKey = getString(R.string.google_maps_key);
        if (!Places.isInitialized()) {
            Places.initialize(getApplicationContext(), apiKey);
        }
        PlacesClient placesClient = Places.createClient(this);
    }

    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.toolbar_menu, menu);
        return true;
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        switch (item.getItemId()) {
            case R.id.search:
                onSearchCalled();
                return true;
            default:
                return false;
        }
    }


   public void onSearchCalled() {
    //döndürülecek yer verisi türlerini belirtir
    List<Place.Field> fields = Arrays.asList(Place.Field.ID, Place.Field.NAME);

    // autocomplete intentini başlatır
    Intent intent = new Autocomplete.IntentBuilder(
            AutocompleteActivityMode.FULLSCREEN, fields)
            .build(this);
    startActivityForResult(intent, AUTOCOMPLETE_REQUEST_CODE);
}

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (requestCode == AUTOCOMPLETE_REQUEST_CODE) {
            //başarılı sonuç döndürüldüğünde çalışacak kod bloğu
            if (resultCode == RESULT_OK) {
                Place place = Autocomplete.getPlaceFromIntent(data);
                Toast.makeText(getApplicationContext(), "Place: " + place.getName() + ", " + place.getId(), Toast.LENGTH_LONG).show();
            }
            //hata oluştuğunda çalışacak kod bloğu
            else if (resultCode == AutocompleteActivity.RESULT_ERROR) {
                Status status = Autocomplete.getStatusFromIntent(data);
                Toast.makeText(getApplicationContext(), status.getStatusMessage(), Toast.LENGTH_LONG).show();
            }
        }
    }
}
```

Projemizi çalıştırdığımızda uygulamaya ait ekran görüntüsü aşağıdaki gibi olacaktır.


![Google Places Api 3](https://i.ibb.co/6Z48YFG/googleplaces3.png)

Böylece Google Places API Place Autocomplete servisini nasıl kullanacağımızı öğrenmiş olduk. Daha fazla bilgi için [Google Maps dokümantasyonunu inceleyebilirsiniz.](https://developers.google.com/places/android-sdk/autocomplete)