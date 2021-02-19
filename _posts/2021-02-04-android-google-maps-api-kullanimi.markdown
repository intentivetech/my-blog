---
layout: post
title:  "Android Google Maps API Kullanımı"
date:   2021-02-04 12:01:22 +0300
categories: android 
sidebar: []
tags: android mapsapi googlemaps
---

Merhabalar bugün sizlere Google Maps API’yi uygulamamız içerisinde nasıl kullanacağımızdan bahsedeceğim. Google Maps API’yi kullanabilmek için öncelikle API Key oluşturmamız gerekmektedir. API Key nasıl oluşturacağımızdan bahsedelim.

# API Key Oluşturma

Bildiğiniz üzere Google Maps dünyada en çok kullanılan haritalardan birisidir. Google Maps’i Android Projemizde kullanabilmek için öncellikle Google Developer Console adresinden bir key almamız gerekiyor.

![Google Maps](https://i.ibb.co/1rSgJvN/googlemaps1.png)

Bunun için öncellikle bir Gmail adresiyle giriş yapmak gerekiyor. Giriş yaptıktan sonra karşımıza çıkan sayfada bulunan **PROJE OLUŞTUR** yazısına tıklayalım.

![Google Maps 2](https://i.ibb.co/Wxp8CMk/googlemaps2.png)

Projemiz için bir isim belirleyelim ve oluştur butonuna tıklayalım.

![Google Maps 3](https://i.ibb.co/T0P39hc/googlemaps3.png)

Karşımıza çıkan sayfada **Maps SDK For Android**‘e tıklayalım ve etkinleştirelim.

![Google Maps 4](https://i.ibb.co/1dBLMkj/googlemaps4.png)

Solda bulunan menüye tıklayarak API’ler ve Hizmetler Sekmesinin altında bulunan Kontrol Paneline tıklayalım.

![Google Maps 5](https://i.ibb.co/sysWKSW/googlemaps5.png)

Burada Kimlik bilgilerini oluştur tıkladığımızda karşımıza 3 seçenek çıkıyor. Buradan API Anahtarını seçelim. Bu şekilde API Anahtarını almış olduk. Projemizden sadece bizim bu API’ye erişebilmemiz için kısıtlama yapmamız gerekecektir. Anahtarı kısıtla butonuna tıklayalım.

![Google Maps 4](https://i.ibb.co/Pz7jG16/googlemaps6.png)

Artık uygulamamızı kısıtlayabileceğiz. Uygulama kısıtlamaları bölümünden Android Uygulamaları seçeneğini işaretleyelim. Uygulamamızın Paket Adı ve Fingerprint bilgilerimizle beraber satır bazında ekleme yapmamız gerekecek.

![Google Maps 5](https://i.ibb.co/6m3BryQ/googlemaps7.png)

SHA-1 Sertifikasını bulabilmek için Android Studiodaki Projemizi açarak sağ bölümde bulunan GRADLE penceresini açalım. **Proje Adı -> Tasks -> signingReport** seçtikten sonra signingReport’un üzerine tıkladığımızda bize SHA-1 değerini verir. Console’dan bu SHA-1 değerini kopyalayıp yapıştırabilirsiniz.

![Google Maps 6](https://i.ibb.co/qDrzzHF/googlemaps8.png)

Daha önceden oluşturduğunuz bir Android Studio Projesi varsa Paket Adına sağ tıklayıp **New -> Activity -> Gallery -> Google Maps Activity**‘i seçerek yeni bir Activity oluşturabilirsiniz.

![Google Maps 7](https://i.ibb.co/qWW0vKq/googlemaps9.png)

Yeni Activity oluştuktan sonra **res -> values -> google_maps_api.xml** klasörünün içerisinde <code>YOUR_KEY_HERE</code> yazan
yere API Anahtarını yapıştırabilirsiniz.


![Google Maps 8](https://i.ibb.co/txjRkYv/googlemaps10.png)

Daha sonra API anahtarını Manifest dosyasının içerisine de yapıştıralım.

## Kodlamaya Başlayalım

MapsActivity’i otomatik olarak oluşturulduğunda içerisinde bazı hazır kodlar bulunuyor. Tam olarak bu kodların ne yaptığından bahsedelim.
Projemizde haritaları kullanmanın en kolay yolu **SupportMapFragment** kullanmaktır. Bunun için **activity_maps.xml** dosyamızın içerisine bir fragment bileşeni eklememiz gerekiyor.

**activity_maps.xml**
```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:map="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/map"
    android:name="com.google.android.gms.maps.SupportMapFragment"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MapsActivity" />
```

**getMapAsync()** otomatik olarak haritalar sistemini ve görünümü başlatır. **onMapReady()** harita kullanıma hazır olduğunda tetiklenir. Harita üzerinde belirli bir noktayı işaretlemek için Marker oluşturmamız gerekir. Bunun için öncelikle enlem ve boylam bilgilerini belirtmemiz gerekiyor. LatLng sınıfını kullanarak enlem ve boylam bilgilerini parametre olarak eklememiz gerekir.
Daha sonra bu enlem ve boylam üzerine Marker eklemek için **addMarker()** methodu kullanılır.

**MapsActivity.java**
```java
package testapp.sdatam.sdatam;
import android.support.v4.app.FragmentActivity;
import android.os.Bundle;
import com.google.android.gms.maps.CameraUpdateFactory;
import com.google.android.gms.maps.GoogleMap;
import com.google.android.gms.maps.OnMapReadyCallback;
import com.google.android.gms.maps.SupportMapFragment;
import com.google.android.gms.maps.model.LatLng;
import com.google.android.gms.maps.model.MarkerOptions;
public class MapsActivity extends FragmentActivity implements OnMapReadyCallback {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_maps);
        // Obtain the SupportMapFragment and get notified when the map is ready to be used.
        SupportMapFragment mapFragment = (SupportMapFragment) getSupportFragmentManager()
                .findFragmentById(R.id.map);
        mapFragment.getMapAsync(this);
    }
    @Override
    public void onMapReady(GoogleMap googleMap) {
        // Add a marker in Sydney, Australia,
        // and move the map's camera to the same location.
        LatLng sydney = new LatLng(-33.852, 151.211);
        googleMap.addMarker(new MarkerOptions().position(sydney)
                .title("Marker in Sydney"));
        googleMap.moveCamera(CameraUpdateFactory.newLatLng(sydney));
    }
}
```

![Google Maps 9](https://i.ibb.co/nfZHgbk/googlemaps11.png)