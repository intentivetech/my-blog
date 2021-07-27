---
layout: post
title:  "Kotlin Programlamaya Giriş — 2"
date:   2021-07-27 15:46:26 +0300
categories: kotlin
sidebar: []
tags: kotlin programlama android
---


Kotlin programlamaya giriş yazısına devam ediyoruz.

# Sınıf Örneği Oluşturma

Yukarıdaki örneği inceleyecek olursak sınıf örneğinin nasıl oluşturulduğunu görebiliriz. 

```kotlin
val animal = Animal("Karabaş")
```

Kotlin dilinde Java'da kullanılan <code>new</code> anahtar kelimesinin kullanılmasına gerek yoktur.

# Inheritance (Miras Alma/Kalıtım)

Inheritance, nesneye dayalı programlamanın en önemli özelliklerinden biridir. Inheritance ile sınıflar birbirinden türetilebilir. Bir sınıf diğer bir sınıftan türediği zaman, türediği sınıfın bütün özelliklerini içerir. Bunun yanında kendine has özellikler de barındırabilir. Kotlin’de sınıf hiyerarşinin en tepesinde <code>Any</code> vardır. Hiçbir sınıftan türemeyen bir sınıf oluşturursanız bu sınıfın super class’ı otomatik olarak Any olur.

```kotlin
class Example // dolaylı olarak Any sınıfından miras alır
```

<code>Any</code> sınıfı üç method içerir: <code>equals()</code>, <code>hashCode()</code> ve <code>toString()</code>. Bu methodlar kotlinde bulunan tüm sınıflar için tanımlanmıştır.

```kotlin
class Example(var number: Int)

fun main(){
      val example = Example(5)
	  println(example.number.toString())
} 
```

Yukarıdaki örnekte <code>Example</code> sınıfı hiçbir sınıftan türemeyen bir sınıftır. Fakat varsayılan olarak Any sınıfını miras alır ve bu sınıfa ait olan <code>toString()</code> methodu kullanılabilir. 

Varsayılan olarak Kotlin sınıfları <code>final</code> (miras alınamaz) olarak tanımlanmıştır. Miras alınabilir hale getirmek için <code>open</code> anahtar kelimesi kullanılır. 

```kotlin
open class Example
```

Bir sınıftan miras almak için <code>:</code> işareti kullanılır.

```kotlin
open class Example {
    
    fun hello() {
        println("hello world")
    }
    
}

class SubClass : Example()

fun main(){
    val subClass = SubClass()
    subClass.hello()
} 
```

Yukarıdaki örnekte <code>Example</code> sınıfından miras alan bir <code>subClass</code> sınıfı bulunmaktadır. <code>subClass</code> sınıfı, <code>Example</code> sınıfına ait olan <code>hello()</code> methodunu kullanabilmektedir. 