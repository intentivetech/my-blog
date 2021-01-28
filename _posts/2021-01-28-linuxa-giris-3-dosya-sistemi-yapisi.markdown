---
layout: post
title:  "Linux’a Giriş — 3 — Dosya Sistemi Yapısı"
date:   2021-01-28 11:41:09 +0300
categories: linux
sidebar: []
---

Linux Dosya Sistemi veya herhangi bir dosya sistemi genellikle, işletim sistemi altında bulunan ve verilerinizin bellekte konumlandırmasını işleyen bir katmandır; onsuz sistem hangi dosyanın nereden başlayıp nerede biteceğini bilemez. 

### Linux Dosya Sistemi Türleri

Linux'u yüklemeye çalıştığınızda, Linux'un aşağıdaki gibi bir çok dosya sistemi sunduğunu göreceksiniz. 

**Ext:** Sınırlamalar nedeniyle kullanılmayan eski bir sistemdir.

**Ext2:** 2 TB'lık verileri yönetmeye izin veren ilk Linux dosya sistemidir.

**Ext3:** Ext2'nin yükseltilmiş bir sürümüdür. Ext3'ün en büyük dezavantajı sunucuları desteklememesidir. 

**Ext4:** Tüm dosya sistemleri arasında en hızlı olan dosya sistemidir ve SSD diskler için çok uyumlu bir seçenektir. Linux dağıtımlarında varsayılan olarak gelen dosya sistemidir.

**JFS:** IBM tarafından geliştirilen eski bir dosya sistemidir. Önceleri büyük ve küçük dosyalar için güzel bir şekilde çalışırken, daha sonrasında dosyaların bozulmuş olduğu bir sistemdir. 

**XFS:** Eski bir dosya sistemidir, küçük dosyalarla yavaş şekilde çalışmaktadır. 

**Btrfs:** Oracle tarafından geliştirilmiştir. İyi bir performansa sahiptir fakat bazı linux dağıtımları ile uyumlu çalışmamaktadır.

![Linux Dosya Sistemi Türleri](https://i.ibb.co/JFV6WL8/dosyaturleri.png)

### Dizin Yapısı

Linux dosya sistemi, root dizininden başlayarak ağaç benzeri bir hiyerarşik yapıya sahiptir. Dizinler, alt dizinler ve veri dosyalarından oluşur. 

![Linux Dosya Sistemi](https://i.ibb.co/D16HF1f/1-6-R-IBn49hxi4y-WBMp4-KJm-A.png)

İlerlemeden önce, Unix benzeri sistemlere özgü bir noktayı vurgulamak istiyorum. Bu sistemler, işleri basit tutmayı ve her şeyi bir bayt dizisi olarak ele almayı amaçlamaktadır. Bu bayt dizileri, işletim sistemi dosyaları olarak bilinir. Yani her şey bir dosyadır. 

Linux'ta yalnızda bir kök dizin bulunmaktadır. Kök dizin, sistemdeki diğer tüm dizinlerin ve dosyaların bulunduğu yerdir ve */* ile gösterilir. Aşağıda kök dizinin altındaki dizinleri görebilirsiniz. 

![Linux Dosya Sistemi](https://i.ibb.co/3hNK4H4/linuxdirectorys.png)

# /home 

Home dizini, her kullanıcının kişisel dosyalarının ve belgelerinin sakladığı yerdir. Linux çok kullanıcılı bir ortamdır, bu nedenle her kullanıcıya yalnızca kendileri ve sistem yöneticisi tarafından erişilebilen belirli bir dizin atanır. 

# /bin ve /sbin 

bin, ikili'nin kısaltmasıdır. Linux'un temel programlarının ve uygulamalarının barındırıldığı dizindir. İkili dosyalar, derlenmiş kaynak kodunu içeren yürütülebilir dosyalardır. Neredeyse tüm temel Linux komutları burada bulunmaktadır. Örneğin *ls, cat, touch, pwd, rm, echo,…* Bu dizindeki ikili dosyalar, bir sistemin önyüklenmesi ve onarılması amacıyla minimum işlevsellik elde etmek için mevcut olmalıdır.

sbin, sistem ikilisinin kısaltmasıdır. Bin'e benzer şekilde, aynı zamanda çalıştırılabilir programları depolamak için bir yerdir. Ancak bu çalıştırılabilir programlar, sistem yapılandırması, bakımı ve yönetim görevleri için gereklidir. Yani bu dizin önyükleme, geri yükleme ve kurtarma için gerekli olan programlar için ayrılmıştır.

# /usr 

Sistem uygulamalarına ait /bin veya /sbin dizinlerinden farklı olarak kullanıcı uygulamalarına aittir.

# /etc 

Tüm sistem genelindeki yapılandırma dosyalarının depolandığı yerdir.

# /opt 

İsteğe bağlı (optional) anlamına gelir. Manuel olarak yüklenen yazılımların bulunduğu dizindir.

# /lib

lib, kütüphane anlamına gelir. Bunlar kütüphane dosyaları dizinleridir. Bu kütüphane dosyaları, diğer ikili uygulamalar arasında paylaşılan programlardır. Bin ve sbin içindeki ikili dosyalar bu kütüphane dosyalarını kapsamlı bir şekilde kullanır. 

# /var 

var değişken anlamına gelir. Bu dizin, sistem günlük dosyaları, posta ve yazıcı biriktirme dizinleri ve geçici dosyalar gibi değişken verileri içerir. Bunlar genellikle boyut olarak büyümesi beklenen dosya ve dizinlerdir. Örneğin, /var/crash, bir işlemin her çöktüğü zamanla ilgili bilgileri tutar.

# /media 

İşletim sisteminizin USB flaş sürücü gibi harici çıkarılabilir cihazlarınızı otomatik olarak bağladığı yerdir.

# /boot 

Önyükleyicinizin yaşadığı yer burasıdır. Bir bilgisayarı başlatmak için gerekli olan statik önyükleyiciyi, çalıştırılabilir çekirdek dosyasını ve yapılandırma dosyalarını içerir.

# /sys 

sys dizini, sistem ve bileşenleri hakkında yapılandırılmış bir şekilde bilgi almanızı sağlar.

# /dev 

Donanım aygıtlarının dosyalarının bulunduğu yerdir. Bu dosyalar, uygulama programlarının donanım aygıtlarınızla etkileşim kurmasına izin verir. 

# /proc

proc, süreç anlamına gelir. Bu dizinde, sistem işlemleri ve kaynakları hakkında bilgi içeren sözde dosyalar bulabilirsiniz. Örneğin, bilgisayarınızdaki her işlemin bu dizinde bu işlemle ilgili bilgileri içeren bir klasörü olacaktır. Bu dizin sanal bir dosya sistemidir ve bilgisayar kapatıldığında kaybolur.

# /run 

Modern Linux dağıtımları, bu dizini RAM çalışma zamanı verilerini depolayan geçici bir dosya sistemi (tmpfs) olarak dahil etmiştir. Bu, önyükleme işleminde çok erken başlatılan (var/run kullanılmadan önce) systemd ve udev gibi arka plan yordamlarının çalışma zamanı bilgilerini depolayabilecekleri standart bir dosya sistemi konumuna sahip oldukları anlamına gelir. Bu dizindeki dosyalar RAM'de depolandığından, kapatıldıktan sonra kaybolurlar.

# /tmp 

tmp dizini, uygulamaların bir oturum sırasında ihtiyaç duydukları geçici dosyaları depoladığı yerdir.

# /root 

root dizini, root kullanıcısı için bir dizindir. Bunu bir root kullanıcının home dizini olarak düşünebilirsiniz.

# /srv 

Bir hizmet dizinidir. Bir web sunucusu çalıştırıyorsanız, siteye özgü verileri bu klasörde depolayabilirsiniz.

# /cdrom 
Bu, CD-ROM'u bağlamak için eski bir dizindir. Ancak günümüzde CD-ROM'lar otomatik olarak medya dizinine bağlanmaktadır.

