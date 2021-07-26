---
layout: post
title:  "Kotlin Programlamaya Giriş"
date:   2021-07-14 10:34:26 +0300
categories: kotlin
sidebar: []
tags: kotlin programlama android
---

## Neden Kotlin Kullanmalıyız? 

Kotlin, hızlı derlenir, daha güvenli kod yazmayı sağlar, basit ve sade bir syntaxe sahiptir ve uygulamaların boyutunun artmasını engeller. Kotlin ile yazılan kod parçaları daha kısa ve daha az ayrıntılıdır bu da daha az hata yapabileceğiniz anlamına gelir. Kotlin, Java dilinden ilham alan bir dildir ancak pek çok farklı ek özellik içeren geliştirilmiştir bir sürümdür. Android uygulamarı geliştirmek için Kotlinde bulunan temel yapı ve temel kavramları bilmek gerekir. Daha önce Java ile proje geliştiren biri olarak kotlin öğrendikten sonra bir daha asla Java kullanmak istemeyebilirsiniz :)

## Kotlin Temel Syntax

# Değişken Tanımlama

Kotlin'de değişkenleri tanımlamak için iki anahtar kelime bulunmaktadır. <code>val</code> ve <code>var</code>

- <code>val</code> değeri asla değişmeyen değerler için kullanılır. 
- <code>var</code> değeri değişebilen değerler için kullanılır.

Değişkenin değeri 15 olan <code>Int</code> tipinde bir değişken tanımlayalım.

```kotlin
var count: Int = 15
```

<code>var</code> anahtar kelimesi count değişkenine yeniden değer atayabileceğiniz anlamına gelir.

```kotlin
var count: Int = 15
count = 20
```

Örnek olarak değeri değiştirilemeyen bir değişken oluşturalım.

```kotlin
val language: String = "Kotlin"
```

# Tip Çıkarımı

Kotlin derleyicisi atanan değerin türüne bağlı olarak değişkenin türünü bulabilir.

```kotlin
val language = "Kotlin"
val upperCaseName = language.toUpperCase()
```

Değişkenin türünün <code>String</code> olduğunu belirtmeden yukarıdaki şekilde de kullanabiliriz. 

# Null Kontrolü

Kotlinde bulunan tip sistemi, <code>NullPointerException</code> hatasını ortadan kaldırmayı amaçlar. Özellikle Java dahil bir çok programlama dilinde null bir nesneye erişmeye çalışırken <code>NullPointerException</code> hatası ile karşılaşılmaktadır. Bu yüzden Kotlin'de referans tipleri <code>null</code> (boş olabilen) yada <code>non-null</code> (boş olamayan) olarak 2'ye ayrılır. Örneğin, <code>String</code> tipindeki bir değişken null olamaz.

```kotlin
var language: String = "Java" 
language = null // hata oluşur
```

Boş bir değer tutmak için değişkenin tipi <code>String?</code> olarak belirtilmelidir. Yani, null olabilecek değişkenlerin sonuna **?** eklemek gerekmektedir.

```kotlin
var newLanguage: String? = "Kotlin" 
newLanguage = null // hata oluşmaz
```

**Null Olabilen Değişkenlerin Kontrolü**

**1. if ile kontrol**

Kotlin'de null olabilen değişkenleri kontrol etmek için bir çok yöntem bulunmaktadır. Diğer dillerde sıklıkla kullandığımız if ile null kontrolü yapmaktır. Örnek olarak null olabilen bir değişken tanımlayarak değişkenin null olup olmadığını direkt olarak kontrol edelim.

```kotlin
val nullableValue: String? = "Hello World"

if(nullableValue != null && nullableValue.length > 0) {
    print("Değişkenin uzunluğu ${nullableValue.length}") 
}
else {
    print("Boş string")
}
```

**NOT:** if ile kontrol işlemi sadece <code>val</code> olan yani değeri asla değiştirilemeyen değişkenlerde çalışır. Aksi takdirde <code>nullableValue</code> değeri kontrol işleminden sonra <code>null</code> olarak atanabilir.

**2. Safe call operatörü ?.**

Null olabilen bir değişken üzerindeki özelliğe erişmek için kullanılabilen ikinci yöntem ise safe call operatörünü <code>?.</code> kullanmaktır. Önce null olabilen bir değişken tanımlayarak safe call operatörünü kullanmadan değişkenin özelliklerine erişmeye çalışalım.

```kotlin
    val b: String? = null
    println(b.length)
```

Kodu çalıştırdığımızda null olabilen bir değişkenin özelliğine erişmek için safe call operatörünü kullanmalısınız uyarısı gelecektir. Şimdi de safe call operatörünü ekleyerek kodu çalıştıralım.

```kotlin
    val b: String? = null
    println(b?.length)
```

<code>b</code> değişkeni <code>null</code> olmasına rağmen kodu çalıştırdığımızda herhangi bir hata ile karşılaşmadık. Böylece olası <code>NullPointerException</code> hatasını engellemiş olduk.

Yalnızca null olmayan değerler üzerinde işlem yapmak için <code>let</code> kullanılmaktadır. <code>a</code> ve <code>b</code> değişkeni null değilse çıktı yazdıralım.

```kotlin
val a: String? = null
a?.let {
    println("a değişkeni null olduğu için çalışmaz")
}
    
val b: String? = "Kotlin"
b?.let {
    println("bu değer null değildir")
}
```

**3. Elvis Operatörü ?:**

Atama işlemi gerçekleştirilirken, atanan değer null olabilen bir değerse; değeri null değilse değişkenin kendisini null ise farklı bir değer atanmasını sağlar. Basit bir örnekle ne demek istediğimizi anlayalım.

```kotlin
val a: Int = if (b != null) b.length else -1
```

<code>b</code> değişkeninin değeri null değilse b'nin uzunluğunu, null ise -1 değerini atama işlemi gerçekleştirdik.

Uzun uzun <code>if</code> kullanmak yerine, elvis operatörünü (?:) kullanarak aynı işlemi gerçekleştirelim.

```kotlin
val a: Int = b?.length ?: -1
```
<code>?:</code> ifadesinin solu boş değilse elvis operatörü o değeri döndürür, aksi takdirde sağ tarafta bulunan ifadeyi döndürür. 

**4. !! operatörü**

<code>NullPointerException</code> hatası almak isteyenlerin kullandığı bir yöntemdir. Değişkenleri null olmayan tipe dönüştürür fakat değişkenin değeri nullsa hata fırlatır.

```kotlin
val nullable: String? = null
nullable!!.length //hata fırlatır
```

# Paket Tanımlama ve İçe Aktarma

Paket tanımlama işlemleri dosyanın en üst satırında bulunmalıdır.

```kotlin
package my.demo

import kotlin.text.*
```

# Standart Çıktıya Yazma İşlemi

<code>print</code> girilen veriyi standart çıktıya yazdırır. 

```kotlin
print("Hello")
println("World")
```

<code>println</code> girilen veriyi standart çıktıya yazdırır ancak bir satır sonu ekler. Böylece daha sonra yazdırılan veri bir sonraki satırda listelenir.

# Fonksiyon Tanımlama 

Dönüş türü olmayan fonksiyon.

```kotlin
fun write(): Unit {
    print("hello")
}
```

Kotlin'de bulunan Unit, Java'daki void'e karşılık gelir ancak kullanılması isteğe bağlıdır. Aynı fonksiyonu Unit kullanmadan da yazabiliriz.

```kotlin
fun write() {
    print("hello")
}
```

Dönüş türü <code>Int</code> olan ve parametre olarak iki adet <code>Int</code> değişken alan fonksiyon.

```kotlin
fun sum(a: Int, b: Int): Int {
    return a + b
}
```

# Sınıf ve Örnek Oluşturma

Bir sınıf oluşturmak için <code>class</code> anahtar kelimesi kullanılır.

```kotlin
class Animal
```

# Constructor Oluşturma

Constructor, bulunduğu class’dan nesne üretimi sırasında çalışacak olan metoddur. Bir sınıf oluştururken constructor tanımlamasak bile içerisinde default olarak constructor bulunur. Kotlin dilinde bir sınıf <code>primary constructor</code> ve bir veya birden fazla <code>secondary constructor</code>'a sahip olabilir.  

1. Primary Constructor (Birincil kurucu) 

Aşağıda <code>name</code> adında parametre alan bir primary constructor örneği bulunmaktadır. Primary constructor tanımlamak için <code>constructor</code> keywordü kullanılır. 

```kotlin
class Animal constructor(name: String) { /*...*/ }
```

Eğer herhangi bir **annotations (@Inject)** yada **visibility modifiers (public, protected, private)** kullanılmıyorsa <code>constructor</code> keywordünün kullanılmasına gerek yoktur. 

```kotlin
class Animal(name: String) { /*...*/ }
```

Her iki şekilde de constructor tanımlanabilir. 

Primary constructorlar herhangi bir kod bloğu içermezler. Bu yüzden kod yazmak için initializer block kullanılır. <code>init</code> anahtar kelimesi kullanılarak tanımlanır.

```kotlin
class Animal constructor(name: String) { 

    init {
        println("animal name ${name}")
    }

}
```




