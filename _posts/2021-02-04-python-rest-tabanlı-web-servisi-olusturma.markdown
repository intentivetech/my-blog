---
layout: post
title:  "Python/Flask ile REST Tabanlı Web Servisi Oluşturma"
date:   2021-02-04 12:33:22 +0300
categories: python 
sidebar: []
tags: python flask rest
---

REST(REpresentational State Transfer), Web üzerindeki bilgisayar sistemleri arasında standartlar sağlayarak sistemlerin birbirleriyle iletişim kurmasını kolaylaştıran mimaridir. REST mimarisi ile uyumlu olan servislere **RESTful** servisler denmektedir.

REST mimarisinde istemci veri almak veya değiştirmek için sunucuya istekte bulunur. Bu isteği gerçekleştirirken:

- ne tür bir işlem yapacağımızı (veri ekleme,silme gibi.),
- istemci ve sunucunun istek veya yanıtla birlikte ek bilgiler iletebilmesini sağlayan HTTP Başlığı(HTTP Header),
- kaynak(resource)

belirtmemiz gerekir.

# HTTP Methodları

Bir REST sisteminde istekte bulunurken kullandığımız 4 temel HTTP Methodu bulunmaktadır:

- GET — belirli bir kaynaktan veri alır
- POST — belirli bir kaynaktağa veri yazar
- PUT — belirli bir kaynağı günceller
- DELETE — belirli bir kaynaktan veri siler.

[Daha detaylı bilgi için tıklayın](https://restfulapi.net/)

## Hadi Kodlayalımmm :)
 
Visual Studioda yeni bir Python projesi oluşturalım. **File -> New -> Project -> Python -> Web -> Blank Flask Web Project** adımlarını takip ederek projemizi oluşturabiliriz.

![Python1](https://i.ibb.co/MPnrZsM/python1.png)

Projeyi oluşturduktan sonra karşımıza aşağıdaki pencere çıkacaktır.

![Python2](https://i.ibb.co/s5vvc68/python2.png)

İşaretli olan seçeneğe tıklayarak, programa gerekli paketleri kendimiz yükleyeceğimizi belirtiyoruz.
Daha sonra projemize Virtualenv ekleyelim. **Solution Explorer -> Python Environments** seçeneğine sağ tıklayarak **Add Virtual Environment**’e tıklayınız.

*Virtualenv, bilgisayarınızdan bağımsız bir geliştirme ortamı sunar, siz istediğiniz modülleri bu geliştirme ortamında kurarsınız, yüklenen modüller sadece o proje için geçerli olup bilgisayarınıza yüklenmez.*

![Python3](https://i.ibb.co/WVkVptv/python3.png)

Açılan pencerede Download and Install Packages seçeneğini işaretleyerek oluştur butonuna tıklayınız.

Flask kütüphanesini projemize ekliyoruz.

```
from flask import Flask
```

Flask sınıfından app nesnesi oluşturuyoruz.

```
app=Flask(__name__)
```

İlk sayfamızı hazırlıyoruz. Flask’a hangi URL’nin fonksiyonumuzu tetiklemesi gerektiğini anlatmak için route() dekoratörünü kullanıyoruz. (/) ana dizin anlamına gelmektedir.

```
@app.route('/')
def hello():
    return "Hello World!"
```

<code__name__</code> değişkeni ile dosyamız ana program olarak mı çalıştırılmış yoksa modül olarak başka bir sayfaya mı dahil edilerek çalıştırılmış kontrol edebiliyoruz. <code>app.run(HOST,PORT)</code> komutu ile kodumuzu çalıştırıyoruz. Host ve port numarası tanımlıyoruz.

 ```
 if __name__ == '__main__':
    import os
    HOST = os.environ.get('SERVER_HOST', 'localhost')
    try:
        PORT = int(os.environ.get('SERVER_PORT', '5555'))
    except ValueError:
        PORT = 5555
    app.run(HOST, PORT)
```

Proje çalıştırıldığında karşımıza **Hello World!** yazısı çıkacaktır.

Farklı bir örnek olarak kitapları listeleyen bir method yazalım. route() dekoratörünü ile istekte bulunacağımız adresi ve Http Methodunu belirtelim. GET Methodunu kullanarak verileri alalım. Aldığımız bu verileri JSON formatına dönüştürmek için jsonify() methodundan yararlanabiliriz.

```
books = [{'id': 0,
'title': 'A Fire Upon the Deep',
'author': 'Vernor Vinge',
'first_sentence': 'The coldsleep itself was dreamless.',
'year_published': '1992'},
{'id': 1,
'title': 'The Ones Who Walk Away From Omelas',
'author': 'Ursula K. Le Guin',
'first_sentence': 'With a clamor of bells that set the swallows soaring, the Festival of Summer came to the city Omelas, bright-towered by the sea.',
'published': '1973'},
{'id': 2,
'title': 'Dhalgren',
'author': 'Samuel R. Delany',
'first_sentence': 'to wound the autumnal city.',
'published’: '1975'}
]

@app.route('/flaskweb/api/books', methods=['GET'])
def get_books():
    return jsonify(books)
```

**http://localhost:portnumarası/flaskweb/api/books** adresine istekte bulunduğunuzda verileri görebilirsiniz.