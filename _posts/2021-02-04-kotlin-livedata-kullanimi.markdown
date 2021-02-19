---
layout: post
title:  "Kotlin LiveData Kullanımı"
date:   2021-02-04 13:43:22 +0300
categories: android 
sidebar: []
tags: android kotlin livedata
---

Bu yazımda Android Architecture Components’in bir parçası olan LiveData’dan bahsedeceğim. Android Architecture Components hakkında bilginiz yoksa [linke tıklayarak inceleyebilirsiniz.](https://developer.android.com/jetpack/guide)

## LiveData Nedir?

LiveData, bir verideki değişiklikleri izlememezi sağlayan Observable veri tutucudur. LiveData, lifecycle aware’dir. Yani Activity, Fragment ya da Servis gibi bileşenlerin yaşam döngülerine karşılık hareket edebilir. Bu özellik LiveData’nın sadece aktif yaşam döngüsündeki bileşenlerin gözlemleyicilerini güncelleştirmesini sağlar. Bu davranış, nesnenin sızmasının önlenmesini, uygulamanın olması gerekenden daha fazla iş yapmamasını ve ekran döndürme gibi durumlarda en son kaydedilen verilerin saklanmasını sağlar. Daha iyi anlamak için bir örnek yapalım.

Butona tıklandıkça TextView’deki değeri arttıran basit bir proje yapacağız. Yeni bir proje oluşturalım ve gradle dosyasının içerisine aşağıdaki satırları ekleyelim.

```
implementation 'androidx.lifecycle:lifecycle-extensions:2.2.0'
```

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="0"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="32dp"
        android:text="Değeri Göster"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

UI tarafından ihtiyaç duyulan Observable değişkenler View Model içerisinden sağlanır. Bunun için View Model sınıfımızı oluşturalım.

```kotlin
package com.hktechnology.examples

import androidx.lifecycle.MutableLiveData
import androidx.lifecycle.ViewModel


class MainViewModel : ViewModel() {

    private var count: Int = 0

    val countLiveData: MutableLiveData<Int> by lazy() {
        MutableLiveData<Int>()
    }

    fun getCount() {
        count++
        countLiveData.value = count
    }

}
```

Sayı değerini tutmak için integer bir count değişkeni tanımlayalım. Daha sonra integer değeri tutmak için LiveData objesi oluşturalım. Oluşturduğumuz integer değişkeninin değerini arttırmak ve LiveData’nın içeriğini güncellemek için getCount() methodunu oluşturalım. Şimdi MainActivity’e geçebiliriz.

```kotlin
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.widget.Button
import android.widget.TextView
import androidx.lifecycle.Observer
import androidx.lifecycle.ViewModelProvider

class MainActivity : AppCompatActivity() {

    private lateinit var mainViewModel: MainViewModel
    private lateinit var textView: TextView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        mainViewModel = ViewModelProvider(this@MainActivity).get(MainViewModel::class.java)
        var button = findViewById<Button>(R.id.button)
        textView = findViewById<TextView>(R.id.textView)

        mainViewModel.countLiveData.observe(this, Observer {
            textView.text = "$it"
        })


        button.setOnClickListener {
            mainViewModel.getCount()
        }
    }
}
``` 

MainActivity içerisinden count değişkenine erişebilmek için View Model nesnesi oluşturmamız gerekir. Count değişkeninin değeri başlangıçta 0'dır ve butona tıklandığında getCount() methoduna erişilerek bu değer arttırılmaktadır. Count değişkeninde oluşan değişiklikleri gözlemlememiz gerekir. LiveData, count değişkeninin değerini gözlemler ve TextView’e atamamızı sağlar.
Şimdi uygulamayı çalıştırıp test edebiliriz.

![LiveData](https://i.ibb.co/SXRdwLb/livedata.gif)

Gördüğünüz üzere ekranı döndürdüğümüzde count değeri değişmeyecektir. 
