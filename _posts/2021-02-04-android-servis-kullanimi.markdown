---
layout: post
title:  "Android Servis Kullanımı"
date:   2021-02-04 12:25:22 +0300
categories: android 
sidebar: []
tags: android servis
---

## Servis Nedir?

Kullanıcıdan bağımsız olarak arka planda çalışan ve arayüzü olmayan bileşenlere Service(Servis) denir.
Servisler, genel olarak uzun süren işlemlerin arka planda yapılmasını sağlamak için geliştirilen bir bileşendir. Örneğin müzik çalar uygulamasında dinlediğimiz müziklerin arka planda da çalmaya devam etmesini sağlar. Yani müzik uygulaması Servis kullanılmasaydı müzik uygulamasını kapattığımız gibi müzik de kesilirdi.

[Servis1](https://i.ibb.co/LgnSjTT/servis1.png)

## Servis Oluşturma

Öncelikle Servis oluşturmak için Service sınıfını extend eden normal bir sınıf oluşturacağız.

**startService()**

startService() methodu çağırıldığında servis başlatılır. İstediğimiz bir activity içerisinden çağırabiliriz.

**onCreate()**

Servis oluşturulduğunda onCreate() methodu çağırılır. Yalnızca arka plan hizmeti bileşeni ilk kez oluşturulduğunda bir kez çağrılır.

**onStartCommand()**

Arka planda yapacağımız işleri bu method içerisinde belirtiriz.

**stopService()**

Servisi sonlandırmak için kullanılır.

**onDestroy()**

Bir servis kullanılmadığında ve yok edildiğinde, sistem bu metodu çağırır. Servis thread, kayıtlı dinleyici, alıcı vb. sistem kaynaklarını boşaltmak için bu metodu kullanır.

## Hadi Kodlayalım :)

Boş bir activity oluşturalım. İçerisine 2 tane buton ekleyelim. Butonlardan 1. sine tıklandığında servisimizi başlatıp, 2. sine tıklandığında servisimizi durduracağız.
Butonları içeren activity_main.xml dosyamız aşağıdaki gibidir.

```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">


    <Button
        android:id="@+id/btnStart"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="8dp"
        android:layout_marginLeft="8dp"
        android:layout_marginTop="240dp"
        android:layout_marginEnd="8dp"
        android:layout_marginRight="8dp"
        android:text="Start Service"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.498"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/btnStop"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="8dp"
        android:layout_marginLeft="8dp"
        android:layout_marginTop="8dp"
        android:layout_marginEnd="8dp"
        android:layout_marginRight="8dp"
        android:layout_marginBottom="8dp"
        android:text="Stop Service"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/btnStart"
        app:layout_constraintVertical_bias="0.118" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

Bir servis bileşeni **startService()** metodu kullanılarak başlatılır, **stopService()** methodu kullanılarak durdurulur. Bu methodlar parametre olarak intent nesnesi alır.

**MainActivity.java**
```
package com.twinstech.mediumexamples;

import androidx.appcompat.app.AppCompatActivity;
import android.content.Intent;
import android.os.Build;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;

public class MainActivity extends AppCompatActivity {

    private Button btnStart;
    private Button btnStop;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        btnStart = (Button) findViewById(R.id.btnStart);
        btnStop = (Button) findViewById(R.id.btnStop);

        btnStart.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                startHelloService();
            }
        });

        btnStop.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                stopHelloService();
            }
        });
    }

    public void startHelloService() {
        Intent intent = new Intent(getApplicationContext(),HelloService.class);
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            startForegroundService(intent);
        } else {
            startService(intent);
        }
    }

    public void stopHelloService() {
        Intent intent = new Intent(getApplicationContext(),HelloService.class);
        stopService(intent);
    }
}
```

Start butonuna tıklandığında oluşturmuş olduğumuz startHelloService() methodu çalışacaktır.

Intent Sınıfı parametre olarak Context ve Sınıf ister. Çalıştırmak istediğimiz Servis Sınıfını parametre olarak gönderiyoruz ve **startService(intent)** methodunu kullanarak servisimizi başlatıyoruz. Aynı şekilde stopHelloService() methodunu da kodlayabilirsiniz.

**NOT:** *Android 8 (Oreo) ve üzeri sürümlerde servis başlatmak için **startForegroundService()** methodu kullanılır. Foreground Servisleri her zaman kullanıcıyı bilgilendirmek için bildirimleri(notifications) kullanır.*

**HelloService.java**

```
package com.twinstech.mediumexamples;

import android.app.Notification;
import android.app.NotificationChannel;
import android.app.NotificationManager;
import android.app.Service;
import android.content.Context;
import android.content.Intent;
import android.os.Build;
import android.os.IBinder;
import android.widget.Toast;
import androidx.core.app.NotificationCompat;

public class HelloService extends Service {
    @Override
    public IBinder onBind(Intent ıntent) {
        return null;
    }

    @Override
    public void onCreate() {
        super.onCreate();
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            NotificationManager notificationManager = (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
            String NOTIFICATION_CHANNEL_ID = "my_channel_id_01";

            NotificationChannel notificationChannel = new NotificationChannel(
                    NOTIFICATION_CHANNEL_ID, "My app no sound", NotificationManager.IMPORTANCE_LOW
            );
            notificationChannel.setDescription("");
            notificationChannel.setSound(null,null);
            notificationChannel.enableLights(false);
            notificationChannel.enableVibration(false);
            notificationManager.createNotificationChannel(notificationChannel);
            Notification notification = new NotificationCompat.Builder(this, NOTIFICATION_CHANNEL_ID)
                    .setContentTitle("")
                    .setContentText("").build();
            startForeground(1, notification);
        }
    }

    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        Toast.makeText(this, "Servis başlatıldı", Toast.LENGTH_LONG).show();
        return START_NOT_STICKY;
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
        Toast.makeText(this, "Servis durduruldu", Toast.LENGTH_LONG).show();
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            stopForeground(true);
        }
    }
}
```

Servis oluşturulduğunda **onCreate()** çalışır. Foreground Servisini başlatırken kullanıcıyı bilgilendirmek için bildirimleri kullandığından bahsetmiştim. Bu yüzden Android 8 ve üzeri sürümlerde çalışması için bildirim oluşturmamız gerekiyor. [Bildirimler hakkında detaylı bilgi için tıklayınız.](https://developer.android.com/training/notify-user/build-notification)

**onStartCommand()** içerisinde arka planda çalıştırmak istediğiniz kodları yazabilirsiniz. Servisin başlatıldığını anlamak için örnek olarak ekranda “Servis başlatıldı” mesajını gösterdim. **onDestroy()** servis durdurulduğunda çalışır, Android 8 için oluşturduğumuz bildirimi burada silmemiz gerekir. Bunun için stopForeground(true) methodu kullanılmaktadır.

**AndroidManifest.xml**
```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.twinstech.mediumexamples">

    <application
        android:allowBackup="true"
        android:icon="@drawable/logo"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

        <service android:name=".HelloService" />

    </application>
</manifest>
```

Oluşturduğumuz Servis Sınıfını Manifest dosyasında belirtmemiz gerekir.

[Servis2](https://i.ibb.co/ygN21JH/servis2.gif)

