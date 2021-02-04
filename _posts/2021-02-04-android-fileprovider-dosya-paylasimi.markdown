---
layout: post
title:  "Android FileProvider Kullanarak Dosya Paylaşımı"
date:   2021-02-04 12:41:22 +0300
categories: android 
sidebar: []
tags: android fileprovider
---

Bir dosyayı diğer uygulamalarla paylaşmanız gerektiğinde ilk aklınıza gelebilecek yöntem, dosyayı harici depolamaya kaydetmek olabilir. Örneğin, bir kamera uygulamasıyla bir resim çekmeniz gerekiyorsa, kamera uygulamasının resmi kaydedeceği bir dosya belirtmeniz gerekir ve harici depolamayı kullanmak cazip gelebilir. Ancak bunu yapmak için kullanıcıdan **WRITE_EXTERNAL_STORAGE** izni istemek zorundasınız. İşte burada devreye FileProvider giriyor.

FileProvider, içerik URI’si oluşturarak geçici erişim izinlerini kullanarak okuma ve yazma erişimi vermenizi sağlar. Böylece, bizim uygulamanın **WRITE_EXTERNAL_STORAGE** iznine sahip olmasına gerek kalmadan önbellek dizinindeki dosyalara geçici erişim sağlayabilirsiniz.

İçerik URI’si, tüm sağlayıcının sembolik adını (authority) ve bir tabloya işaret eden bir yolunu (path) içerir.

## Uygulama

Bir FileProvider yalnızca önceden belirttiğiniz dizinlerdeki dosyalar için bir içerik URI’si oluşturabilir. Bu dosyaları tanımlamak için önce res klasörünün altına bir xml dizini oluşturalım. res/xml/ dizinin altına <code>file_provider_paths.xml</code> dosyası oluşturalım.

**file_provider_paths.xml**
```
<paths xmlns:android="http://schemas.android.com/apk/res/android">
    <files-path name="my_images" path="images/"/>
    ...
</paths>
```

Oluşturduğumuz path dosyasını FileProvider ile bağlamamız için manifest dosyasının içerisinde bir sağlayıcı tanımamız gerekiyor.

```
<provider
    android:name="androidx.core.content.FileProvider"
    android:authorities="${applicationId}.provider"
    android:exported="false"
    android:grantUriPermissions="true">
    <meta-data
        android:name="android.support.FILE_PROVIDER_PATHS"
        android:resource="@xml/file_provider_paths"/>
</provider>
```

Android sistemi tarafından tüm sağlayıcıları ayırt etmek için authority kullanılır. Bu yüzden android:authorities eşsiz bir değer olmalıdır. Örneğin uygulamanızın paket adını kullanabilirsiniz.

Diğer uygulamaların FileProvider’ı kullanmasını engellemek için **android:exported="false"** olarak tanımlıyoruz.

Okuma/yazma yetkisine izin vermek için **android:grantUriPermissions="true"** olarak tanımlanmak zorundadır.

Galeriden resim seçeceğimiz bir uygulama yapalım.

```
public class MainActivity extends AppCompatActivity {

    private static final int REQUEST_GALLERY_PHOTO = 1;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        dispatchGalleryIntent();
    }

    private void dispatchGalleryIntent() {
        Intent pickPhoto = new Intent(Intent.ACTION_PICK,
                MediaStore.Images.Media.EXTERNAL_CONTENT_URI);
        pickPhoto.addFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION);
        startActivityForResult(pickPhoto, REQUEST_GALLERY_PHOTO);
    }

    @Override
    public void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (resultCode == Activity.RESULT_OK) {
            Uri selectedImage = data.getData();
            //örnek olarak dosyayı sunucuya gönderebilirsiniz...
        }
    }
}
```

dispatchGalleryIntent() methodu içerisinde içerik URI’si içeren bir Intent oluşturduk. ACTION_PICK, uygulamalardan herhangi birinden bir resim seçebilmeyi sağlar. İçerik URI’sını izinler eklemek için Intent.setFlags() kullanılır. Activity’i bir “requestCode” ile beraber başlatmak istemeniz durumunda çağrılır startActivityForResult kullanılır. Activity başlatıldıktan sonra resim seçme işlemi başarıyla gerçekleştirildiyse seçili dosyayı sunucuya gönderebilirsiniz. İyi çalışmalar..