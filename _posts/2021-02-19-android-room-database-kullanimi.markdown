---
layout: post
title:  "Android Room Database Kullanımı"
date:   2021-02-19 10:17:22 +0300
categories: android 
sidebar: []
tags: android database room
---

Room bir ORM (Object Relational Mapping) kütüphanesidir. ORM, kod blokları ile veritabanı arasında oluşan bir köprüdür. Room, veritabanında oluşturulan alanları Java nesnelerine dönüştürmeyi sağlar. En yaygın kullanım durumu, ilgili verileri ön belleğe almaktadır. Böylece, cihaz ağa erişemediğinde kullanıcı çevrimdışıyken içeriğe erişebilir. 

Room kütüphanesi, SQLite veritabanı üzerinde bir soyutlama katmanı sağlar. 

Room kullanmanın avantajları: 

- Her @Query ve @Entity derleme zamanında kontrol edilir, bu da uygulamanızı çalışma zamanında çökme sorunlarından korur ve yalnızca sözdizimini değil, aynı zamanda eksik tabloları da kontrol eder.
- Tekrarlayan ve hataya açık standart kod yazmayı en aza indirir
- Diğer mimari bileşenlerle kolayca entegre edilir (LiveData, ViewModel vb.)

Sağladığı kolaylıklar nedeni ile Android tarafından doğrudan SQLite kullanılması yerine Room kullanılması önerilir.

Room Database 3 ana bileşene sahiptir.

1. Entity 
2. Dao
3. Database Sınıfı

# 1. Entity 

Veritabanı içindeki bir tabloyu temsil eder. Room, <code>@Entity</code> anotasyonuna sahip her sınıf için bir tablo oluşturur, sınıfta bulunan alanlar tablodaki sütunlara karşılık gelir. 

**Entity Anotasyonları**

Varlıklarımızı modellemeye başlamadan önce, bazı yararlı anotasyonları ve özniteliklerini bilmemiz gerekir.

**@Entity** — Bu anotasyona sahip her model sınıfının DB'de bir mapping tablosu olacaktır.

**@PrimaryKey** — Adından da anlaşılacağı gibi, bu anotasyon Entity'nin birincil anahtarına işaret eder. *autoGenerate* — true olarak ayarlanırsa, SQLite sütun için benzersiz bir kimlik oluşturacaktır.

<code>@PrimaryKey(autoGenerate = true)</code>

**@ColumnInfo** — Sütun hakkında özel bilgilerin belirlenmesine izin verir.

<code>@ColumnInfo(name = "column_name")</code>

**@Embeded** — İç içe geçmiş alanlara doğrudan SQL sorgularında başvurulabilir.

# 2. DAO

DAO, SQL sorgularını belirtir. Veritabanındaki verileri sorgulamak, güncellemek, eklemek ve silmek için yöntemler sağlar.

- DAO bir interface veya abstract bir sınıf olmalıdır.

# 3. Database Sınıfı

Veritabanını tutan ve uygulamanızın kalıcı verilerine temel bağlantı için ana erişim noktası görevi gören veritabanı sınıfıdır. Bir veritabanı oluşturmak için RoomDatabase'i extend eden abstract bir sınıf tanımlamamız gerekir. Bu sınıf <code>@Database</code> anotasyonuna sahip olmalıdır. 

Database sınıfı aşağıdaki koşulları sağlamalıdır:

- RoomDatabase'i extend eden abstract bir sınıf olmak zorundadır.
- Veritabanıyla ilişkili Entity'lerin listesi anotasyon olarak eklenmelidir.

![Room DB](https://i.ibb.co/R2RwQLG/roomdb.png)

Database sınıfı, uygulamanıza bu veritabanıyla ilişkili DAO örneklerini sağlar. DAO'lar ise veritabanından Entity'leri almak için kullanabilir. Uygulama, ilgili tablolardan satırları güncellemek veya eklemek üzere yeni satırlar oluşturmak için tanımlanmış veri Entity'lerini de kullanabilir.

## Room Örneği

# 1. Adım: Gradle Bağımlılıklarının Ekleyin

Kotlin kullananlar plugins içerisine kotlin kapt eklemelidir.

```
plugins {
    id 'kotlin-kapt'
}
```

Projenize Room kütüphanesini eklemek için, <code>build.gradle</code> dosyasını açın ve aşağıdaki satırları ekleyin:

```
def roomVersion = '2.2.6'
implementation "androidx.room:room-runtime:$roomVersion"
kapt "androidx.room:room-compiler:$roomVersion"
implementation "androidx.room:room-ktx:$roomVersion"
testImplementation "androidx.room:room-testing:$roomVersion"
```

# 2. Adım: Model Sınıfı Oluşturun

Room, <code>@Entity</code> anotasyonu bulunan her sınıf için bir tablo oluşturur; sınıftaki alanlar tablodaki sütunlara karşılık gelir. Bu nedenle, Entity sınıfları herhangi bir mantık içermeyen küçük model sınıfları olma eğilimindedir. 

Örnek olarak bir Word Sınıfı oluşturalım. Örnek uygulamanın verileri girilen kelimelerdir ve bu değerleri tutmak için basit bir tabloya ihtiyacınız olacaktır.  

![Word Table](https://i.ibb.co/0BqhCYV/word-table.png)

Room, Entity aracılığı ile tablolar oluşturmaya olanak sağlar. Word adında yeni bir data sınıfı oluşturun. 

Word Sınıfı:

```
@Entity(tableName = "word_table")
data class Word(
    @PrimaryKey
    @ColumnInfo(name = "word")
    val word : String
)
```

Word sınıfını Room veritabanı için anlamlı kılmak için anotasyonlar eklenmesi gerekir. Anotasyonlar, Word sınıfının her bir bölümünün veritabanındaki bir girişle nasıl ilişkili olduğunu tanımlar.

Anotasyonları inceleyelim:

- <code>@Entity(tableName = "word_table")</code> Her **@Entity** sınıfı bir SQLite tablosunu temsil eder. Tablonun adının oluşturduğunuz sınıf adından farklı olmasını istiyorsanız tablo adını belirtebilirsiniz. Burada tablo adı "word_table" olarak adlandırılır. 
- <code>@PrimaryKey</code> Her Entity'nin bir primary key'e ihtiyacı vardır. Bu örnekte zaten bir alan bulunduğu için word alanı primary key olarak tanımlanmıştır. 
- <code>@ColumnInfo(name = "word")</code> Tablodaki alanın değişken adından farklı olmasını istiyorsanız alan adını değiştirebilirsiniz. Burada "word" olarak adlandırılmıştır. 


> 📝 **NOT**
>
> Primary keye aşağıdaki gibi anotasyon ekleyerek benzersiz anahtarları otomatik olarak oluşturabilirsiniz:
>
> <code>@PrimaryKey(autoGenerate = true)</code>

# 3. Adım: DAO Oluşturun

Aşağıdaki sorguları sağlayan DAO oluşturalım:

- Alfabetik olarak sıralanan tüm kelimeleri alma
- Kelime ekleme
- Kelime silme

1. WordDao adında yeni bir kotlin sınıfı oluşturun.
2. Aşağıdaki kodu DAO sınıfının içerisine ekleyin.

```
@Dao
interface WordDao {

    @Query("SELECT * FROM word_table ORDER BY word ASC")
    fun getAlphabetizedWords(): List<Word>

    @Insert(onConflict = OnConflictStrategy.IGNORE)
    suspend fun insert(word: Word)

    @Query("DELETE FROM word_table")
    suspend fun deleteAll()
}
```

DAO sınıfını inceleyelim:

- WordDao bir interface'dir; DAO'lar interface veya abstract olmalıdır.
- **@Dao** anotasyonu belirtilen sınıfı Room için bir DAO sınıfı olarak tanımlar.
- **@Insert** anotasyonu, herhangi bir SQL sorgusu oluşturmanızı gerektirmeyen özel bir anotasyondur. (Satırları silmek için **@Delete**, güncellemek için **@Update** anotasyonları da vardır)
- <code>onConflict = OnConflictStrategy.IGNORE</code> Seçilen onConflict stratejisi, listedeki ile tamamen aynı bir alan varsa yeni girilen alanı yok sayar. 
- Birden fazla alanı silmek için bir anotasyon yoktur, bu nedenle **@Query** anotasyonu eklenerek sorgu yazılmıştır.

# 4. Adım: Room Veritabanı Oluşturun

Yeni bir veritabanı oluşturmak için öncelikle <code>RoomDatabase</code> sınıfını extend eden bir abstract sınıf oluşturulması gerekir. <code>WordRoomDatabase</code> adında bir kotlin sınıfı oluşturun ve aşağıdaki satırları ekleyin:

```
@Database(entities = [Word::class], version = 1, exportSchema = false)
public abstract class WordRoomDatabase : RoomDatabase() {

    abstract fun wordDao(): WordDao

    companion object {
        @Volatile
        private var INSTANCE: WordRoomDatabase? = null

        fun getDatabase(context: Context): WordRoomDatabase {
            return INSTANCE ?: synchronized(this) {
                val instance = Room.databaseBuilder(
                    context.applicationContext,
                    WordRoomDatabase::class.java,
                    "word_database"
                ).build()
                INSTANCE = instance
                instance
            }
        }
    }
}
```

Kodu inceleyelim:

- Veritabanı abstract ve RoomDatabase'i extend etmelidir
- Sınıfın veritabanı olduğunu belirten <code>@Database</code> anotasyonu eklenir ve veritabanına ait entityleri ve sürüm numarasını ayarlamak için anotasyon parametreleri kullanılır.
- Her DAO sınıfı için abstract bir "getter" methodu oluşturulmalıdır.
- Veritabanının birden çok örneğinin aynı anda açılmasını önlemek için Singleton WordRoomDatabase tanımladık. Singleton, sistem çalıştığı sürece nesnenin tek bir defa oluşturulacağını garantilemek için kullanılır. 
- <code>getDatabase</code> methodu singleton bir veritabanı nesnesi döndürür. Veritabanı nesnesine ilk kez erişildiğinde DatabaseBuilder kullanılarak "word_database" adında veritabanı oluşturulur.

# 5. Adım: Verileri Yönetin

Verileri yönetmek için öncelikle veritabanının bir nesnesini oluşturmanız gerekir. Daha sonra veri ekleme, silme gibi işlemleri gerçekleştirebilirsiniz. 

```
class MainActivity : AppCompatActivity() {

    private lateinit var wordRoomDatabase : WordRoomDatabase

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        wordRoomDatabase = WordRoomDatabase.getDatabase(applicationContext)
        GlobalScope.launch {
            insertData()
            listData()
        }
    }

    private suspend fun insertData()
    {
        val word = Word("Hello")
        wordRoomDatabase.wordDao().insert(word)
    }

    private suspend fun listData()
    {
        val list = wordRoomDatabase.wordDao().getAlphabetizedWords()
        Log.d("list", list.toString())
    }
}
```

Kodu inceleyelim:

- WordRoomDatabase sınıfından nesne oluşturulduktan sonra DAO'ya erişerek CRUD işlemleri gerçekleştirilmelidir.
- Room Database, düşük UI performansından kaçınmak için main thread üzerinde sorgu göndermenize izin vermez. Bu yüzden main threadi kitlememek ve kesintisiz akışı kesmemek için coroutineler kullanılmaktadır. Coroutine başlatmak için coroutine builder kullanılmaktadır. Launch, mevcut iş parçacığını engellemeden bir coroutine başlatan coroutine builder olarak tanımlanır. Suspend fonksiyonlar ise mevcut threadi bloklamadan askıya alınma işlevini sağlarlar. Ayrıca suspend fonksiyonlar coroutine üzerinden çağırılır.
- Uygulama çalıştırıldığında Hello kelimesi veritabanında bulunan "word_table" tablosuna eklenecektir.
- Kelimenin eklenip eklenmediğini göstermek için *listData* fonksiyonu çalıştırılarak veriler Log'da gösterilmiştir. Log'da <code>D/list: [Word(word=Hello)]</code> şeklinde görebilirsiniz.

Herkese iyi çalışmalar dilerim.








