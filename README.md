# REMIT - Anı Yönetim Uygulaması

REMIT, kullanıcıların yerler, oyunlar, tarifler, izlenceler, okumalar ve dinlenceler gibi kategoriler altında anılarını kaydedebileceği, düzenleyebileceği ve yönetebileceği bir masaüstü uygulamasıdır. Uygulama, Java Swing kullanılarak geliştirilmiştir ve MySQL veritabanı ile çalışmaktadır.

## Özellikler

- **Kullanıcı Kaydı ve Girişi**
  - Kullanıcılar hesap oluşturabilir, giriş yapabilir ve şifrelerini değiştirebilir.
- **Profil Yönetimi**
  - Kullanıcılar profil bilgilerini görüntüleyebilir, kullanıcı adını ve şifresini güncelleyebilir.
- **Kategoriler**
  - Yerler, oyunlar, tarifler, izlenceler, okumalar ve dinlenceler için ayrı kategoriler bulunmaktadır.
- **Yerler Modülü**
  - Kullanıcılar yer kategorisinde yeni anılar ekleyebilir, mevcut anılarını düzenleyebilir veya silebilir. Diğer kategoriler için de aynı işlevler geçerlidir.
- **Fotoğraf Yönetimi**
  - Her kategoriye fotoğraf eklenebilir, fotoğraflar düzenlenebilir ve önizlenebilir.
- **Şifreli Veritabanı Bağlantısı**
  - Veriler MySQL veritabanında saklanır ve uygulama `DatabaseConnection` sınıfı üzerinden bağlanır.

---

## Proje Yapısı

```proje_dizini
src/main/java/org/example/remit
├── Database
│   ├── DatabaseConnection.java         # MySQL veritabanı bağlantısı
│   └── DatabaseUtil.java               # Görsellerin kaydedilmesi ve alınması
├── Screen
│   ├── OpeningScreen.java              # Uygulama başlangıç ekranı
│   ├── SecondScreen.java               # Giriş ve kayıt ekranı
│   ├── LoginScreen.java                # Kullanıcı giriş ekranı
│   ├── LoginScreenDatabase.java        # Kullanıcı doğrulama
│   ├── RegisterScreen.java             # Kullanıcı kayıt ekranı
│   ├── RegisterScreenDatabase.java     # Kullanıcı kayıt işlemleri
│   └── MainScreen.java                 # Ana kategori ekranı
├── profil
│   ├── Profil.java                     # Profil ekranı
│   ├── KullaniciAdiDegistir.java       # Kullanıcı adı değiştirme ekranı
│   ├── SifreDegistir.java              # Şifre değiştirme ekranı
│   └── HesapSil.java                   # Hesap silme işlemi
├── yerler
│   ├── Yerler.java                     # Yer kategorisi ana ekranı
│   ├── YeniYerEkleScreen.java          # Yeni yer ekleme ekranı
│   ├── YeniYerEkleScreenDatabase.java  # Yeni yer ekleme veritabanı işlemleri
│   └── YerEditScreen.java              # Yer düzenleme ekranı
├── izlence
├── dinlence
├── okuma
├── tarif
├── oyun                                # Bu sınıfların içeriği de "yerler" sınıfı ile benzer yapıdadır.


```
## Kullanılan Teknolojiler

- **Programlama Dili:** Java 17
- **Arayüz:** Swing
- **Veritabanı:** MySQL
- **Bağımlılık Yönetimi:** Maven
- **IDE:** IntelliJ IDEA
- **Diğer Araçlar:** JDBC, File IO, Properties API
- **Görsel Yönetimi:** ImageIO, BufferedImage

---
## Gereksinimler
- Java 11 veya üstü bir sürüm
- MySQL 8 veya üstü bir sürüm
- Maven
- IDE: Intellij IDEA önerilir.

---
## Kurulum
1. **Projeyi Klonlayın:**
```bash
git clone https://github.com/kullaniciadi/remit.git
cd remit
```
2. **Veritabanını Ayarlayın:**
MySQL üzerinde aşağıdaki sorguları çalıştırarak veritabanını oluşturun:
```sql
CREATE DATABASE remit;

USE remit;

CREATE TABLE kayit (
    kullaniciAd VARCHAR(45) PRIMARY KEY,
    kullanıcıMail VARCHAR(45) UNIQUE NOT NULL,
    kullanıcıSifre VARCHAR(45) NOT NULL
);

CREATE TABLE user_images (
    id INT AUTO_INCREMENT PRIMARY KEY,
    kullaniciAd VARCHAR(50),
    yerAdi VARCHAR(255),
    image_data LONGBLOB,
    aciklama TEXT,
    category VARCHAR(50),
    FOREIGN KEY (kullaniciAd) REFERENCES kayit(kullaniciAd)
);
```
3. **Bağımlılıkları Yükleyin:**
Maven kullanarak bağımlılıkları yükleyin:
```bash
mvn clean install
```
4. **Veritabanı Ayarlarını Yapılandırın:**
src/main/resources/config.properties dosyasını oluşturun ve aşağıdaki bilgileri ekleyin:
```properties
db.url=jdbc:mysql://localhost:3306/remit
db.user=root
db.password=sifre
```

5. **Projeyi Çalıştırın:**
Ana sınıf olarak OpeningScreen sınıfını çalıştırarak uygulamayı başlatın.

---
## Kullanım
- Başlangıç ekranı
  - Uygulama açıldığında bir giriş ekranı ile karşılaşırsınız. Buradan giriş yapabilir veya yeni bir hesap oluşturabilirsiniz.
- Profil Yönetimi
  - Profil ekranından kullanıcı adınızı değiştirebilir, şifrenizi güncelleyebilir veya hesabınızı silebilirsiniz.
- Kategori Ekranları
  - Ana kategori ekranından istediğiniz kategoriye geçiş yapabilirsiniz.
- Yeni Kayıt Ekleyin
  - Kategorilere ait yeni kayıtlar ekleyebilir, görseller yükleyebilir ve açıklama yazabilirsiniz.
- Düzenleme ve Silme
  - Kayıtları düzenlemek için üzerine tıklayın veya silmek için “Sil” butonunu kullanın.

---

## Sınıflar ve Modüller

### **1. Database Modülü**
#### `DatabaseConnection`
- **Amaç:** Veritabanı bağlantısını yönetir.
- **Özellikler:**
  - MySQL bağlantısı sağlar.
  - Bağlantı bilgilerini (`URL`, `USER`, `PASSWORD`) `config.properties` dosyasından okur.

#### `DatabaseUtil`
- **Amaç:** Fotoğrafların veritabanına kaydedilmesini ve yüklenmesini sağlar.
- **Metotlar:**
  - `saveImage`: Dosyayı ve kullanıcı adını veritabanına kaydeder.
  - `getImage`: Kullanıcı adına bağlı bir görseli döndürür.

---

### **2. Profil Modülü**
#### `Profil`
- Kullanıcı profiline özel bir arayüz sağlar. Şu işlemler yapılabilir:
  - Kullanıcı adı değiştirme.
  - Şifre değiştirme.
  - Hesap silme.

#### `KullaniciAdiDegistir`
- Kullanıcı adını değiştirmek için bir arayüz sunar. Veritabanında kullanıcı adını günceller.

#### `SifreDegistir`
- Kullanıcıların eski şifrelerini doğrulayıp yeni şifre belirlemelerini sağlar.

#### `HesapSil`
- Kullanıcı hesaplarını veritabanından siler ve uygulamayı kapatır.

---

### **3. Kullanıcı Yönetimi Modülü**

#### `RegisterScreen`
**Amaç:** Kullanıcının sisteme kaydolmasını sağlar.

- **Form Bilgi Girişi:** Kullanıcı adı, e-posta ve şifre bilgilerini alır.
- **Doğrulama:** Bilgiler eksikse hata mesajı verir.
- **Veritabanı İşlemi:** Kullanıcı bilgilerini `kayit` tablosuna kaydeder.
- **Yönlendirme:** Kayıt işlemi tamamlandıktan sonra `SecondScreen` ekranına geçiş yapar.

**Önemli İlişkiler:**
- `kayit` tablosuyla çalışır ve kullanıcı adının benzersizliğini kontrol eder.
- `DatabaseConnection` sınıfını kullanarak veritabanı bağlantısı sağlar.

#### `LoginScreen`
**Amaç:** Kullanıcıların sisteme giriş yapmasını sağlar.

- **Form Bilgi Girişi:** Kullanıcı adı ve şifre bilgilerini alır.
- **Doğrulama:** Bilgiler eksikse veya yanlışsa hata mesajı verir.
- **Veritabanı İşlemi:** Kullanıcı bilgilerini `kayit` tablosunda doğrular.
- **Yönlendirme:** Giriş başarılıysa `DikdortgenGUI` ekranına geçiş yapar.

**Önemli İlişkiler:**
- `kayit` tablosuyla çalışarak kullanıcı doğrulaması yapar.
- `DatabaseConnection` sınıfını kullanarak veritabanı bağlantısı sağlar.

---

### **4. Ana Ekran Modülü**

#### `OpeningScreen`
- Uygulama açıldığında gösterilen başlangıç ekranıdır.
- "BAŞLA" düğmesi ile giriş ekranına yönlendirir.

#### `SecondScreen`
- Giriş ve kayıt ekranlarına yönlendiren bir ekrandır.

#### `MainScreen`
- Anıların kategorilere ayrıldığı ana ekran.
- Her kategori için özel ikonlar ve yönlendirme bağlantıları içerir.
- "Profilim" düğmesi ile kullanıcı profil sayfasına geçiş yapılır.

---

### **5. Yerler Modülü**

#### `Yerler`
- Kullanıcının yer kategorisine ait fotoğraflarını görüntülediği ve yönetebildiği arayüzdür.
- Fotoğraflar grid düzeninde gösterilir.
- Kullanıcı, fotoğraflara tıklayarak düzenleme ekranına geçebilir veya “Sil” butonuyla kayıtları silebilir.

#### `YerEditScreen`
- Kullanıcının bir fotoğrafın adını, açıklamasını ve görselini düzenlemesini sağlar.
- Mevcut veriler (ad, açıklama, görsel) düzenlenebilir form alanlarına yüklenir.
- Güncelleme sonrası veritabanında değişiklikler kaydedilir.

#### `YeniYerEkleScreen`
- Yeni bir yer eklemek için kullanıcıya form sunar.
- Kullanıcı, yer adı ve açıklamasını girerek bir fotoğraf yükleyebilir.
- Fotoğraf seçimi sonrası görsel önizleme özelliği sunar.

#### `YeniYerEkleScreenDatabase`
- Yeni bir yer eklemek için kullanıcıya form sunar.
- Görsel dosyası, Blob formatında veritabanına kaydedilir.

---

## Veritabanı Yapısı

### **1. `kayit` Tablosu**
```sql
CREATE TABLE `kayit` (
  `kullaniciAd` varchar(45) NOT NULL,
  `kullanıcıMail` varchar(45) NOT NULL,
  `kullanıcıSifre` varchar(45) NOT NULL,
  PRIMARY KEY (`kullaniciAd`, `kullanıcıMail`, `kullanıcıSifre`),
  UNIQUE KEY `kullaniciAd_UNIQUE` (`kullaniciAd`)
);
```
---
## OOP Prensiplerinin Sağlanması

**1. Encapsulation (Kapsülleme)**
- Alanlar (fields) genellikle private olarak tanımlanmıştır. Bu sayede, sınıfın dışından doğrudan erişim engellenmiş ve veri gizliliği sağlanmıştır.
- Verilere erişim ve değiştirme işlemleri, yalnızca ilgili sınıfın getter/setter metodları veya sınıfın içindeki iş mantıklarıyla yapılır.

**2. Inheritance (Kalıtım)**
- Projede Java Swing bileşenleri (örneğin JFrame, JPanel, JDialog) kullanılarak kalıtım sağlanmıştır. Bu, kullanıcı arayüzü sınıflarının temel Swing özelliklerini miras almasını ve üzerine yeni özellikler eklenmesini mümkün kılmıştır.
- *JPanel* ve *JButton* gibi Swing bileşenleri özelleştirilmiş ve yeni davranışlar eklenmiştir:

**3. Polymorphism (Çok Biçimlilik)**
- **Override (Üzerine Yazma):** Swing bileşenleri özelleştirilerek varsayılan davranışların üzerine yazılmıştır. Örneğin, *OvalButton* sınıfında paintComponent metodu, düğmenin tasarımını değiştirmek için özelleştirilmiştir.
- **Interface Kullanımı:** Runnable arabirimi, geri çağırma fonksiyonları için uygulanmıştır.

**4. Abstraction (Soyutlama)**
- Projede, veritabanı işlemleri soyutlanarak iş mantığından ayrılmıştır. Veritabanı bağlantısı, *DatabaseConnection* sınıfı üzerinden soyutlanmıştır. Bu, her sınıfın kendi bağlantısını oluşturmaktansa merkezi bir yapı kullanmasını sağlar.
- Veritabanı işlemleri, ayrı bir sınıf olan *YeniYerEkleScreenDatabase* içinde toplanmıştır. Böylece GUI sınıfları (ör. YeniYerEkleScreen) yalnızca arayüz ile ilgilenir.

---
## SOLID Prensiplerinin Sağlanması

**1. Single Responsibility Principle (Tek Sorumluluk Prensibi)**
- Projede her sınıf, belirli bir görevi yerine getirmek üzere tasarlanmıştır:
  - *DatabaseConnection*, veritabanı bağlantısını yönetir.
  - *YeniYerEkleScreenDatabase, LoginScreenDatabase, RegisterScreenDatabase* gibi sınıflar, veritabanı işlemleriyle ilgilenir.
  - *Yerler, YerEditScreen ve YeniYerEkleScreen* yerlerle ilgili GUI işlemleri için tasarlanmıştır.
  - *Profil, SifreDegistir, KullaniciAdiDegistir ve HesapSil* profil yönetimiyle ilgili işlemleri sağlar.

Bu yaklaşım ile, kodun okunabilirliğini arttırmak ve her sınıfın yalnızca bir işlevi yerine getirmesi amaçlanmıştır. Böylece herhangi bir değişiklik gerektiğinde yalnızca ilgili sınıf güncellenebilir.

**2. Open/Closed Principle (Açık/Kapalı Prensibi)**
- Yeni özellikler eklenirken mevcut sınıflar değiştirilmeden yeni sınıflar ve işlevler eklenmiştir:
  - Örneğin, Yerler, Oyunlar, İzlenceler gibi kategoriler için ayrı sınıflar oluşturulmuş ve her biri veritabanı erişimi ve GUI yönetimi gibi işlevleri bağımsız olarak sağlamıştır.
  - Yerler gibi sınıflar, dinamik kategorilere göre verileri göstermek için genişletilebilecek şekilde tasarlanmıştır.
 
Bu tasarım, projeye yeni bir kategori (örn. Müzikler) eklenmek istendiğinde, mevcut kodların değiştirilmesine gerek kalmadan yeni sınıflar eklenerek genişletilmesini sağlar.

**3. Liskov Substitution Principle (Liskov’un Yerine Geçme Prensibi)**
- Swing bileşenleri (JPanel, JButton, JFrame) genişletilmiş ve yeni davranışlar eklenmiştir:
  - Örneğin; *OvalButton* sınıfı, özelleştirilmiş bir düğme davranışı sağlar ancak standart *JButton* gibi her yerde kullanılabilir.
  - Bu, *JButton* yerine *OvalButton* kullanılmasının mevcut işleyişi bozmamasını sağlar.
  - Geri çağırma mekanizmaları da benzer şekilde tasarlanmıştır; Runnable arabirimi kullanılarak ekran güncellemeleri standart bir yöntemle gerçekleştirilir.
 
Bu sayede sınıflar, değiştirilmeden birbirinin yerine geçebilir veya genişletilebilir.

**4. Interface Segregation Principle (Arayüz Ayrımı Prensibi)**
- 	Projede, büyük ve karmaşık arayüzler yerine, küçük ve belirli görevler için yapılandırılmış sınıflar kullanılmıştır:
  - Örneğin, Swing tabanlı arayüzler ve veritabanı işlemleri farklı sınıflarda ayrılmıştır: *YeniYerEkleScreen*, Kullanıcıdan veri alır ve arayüzü yönetir; *YeniYerEkleScreenDatabase* ise Verilerin MySQL veritabanına kaydedilmesini sağlar.

Bu ayrım, her bir sınıfın yalnızca kendi işlevine odaklanmasını ve diğer sınıflarla gereksiz bağımlılıklardan kaçınılmasını sağlar.

**5. Dependency Inversion Principle (Bağımlılıkların Ters Çevrilmesi Prensibi)**
- Projede veritabanı bağlantısı merkezi bir sınıf olan DatabaseConnection aracılığıyla yönetilir. Böylece diğer sınıflar doğrudan *DriverManage*r gibi düşük seviyeli bileşenlere bağlı değildir.
- *DatabaseConnection* sınıfı soyut bir bağlantı sağlar. Eğer ileride veritabanı sistemi değiştirilmek istenirse, yalnızca bu sınıfın değiştirilmesi yeterlidir. Uygulamanın geri kalanı bu değişimden etkilenmez.
- Benzer şekilde, geri çağırma işlemleri için Runnable arabirimi kullanılmıştır. Bu, farklı işlem türlerinin kolayca uyarlanabilmesini sağlar.
  
---
## CRUD Prensiplerinin Sağlanması

**1. Create(Oluşturma)**
- Projede kullanıcıların ve anıların oluşturulması için çeşitli mekanizmalar sağlanmıştır:
  - *RegisterScreen*, kullanıcıların kayıt olmasını sağlayan bir ekran sunar. Kullanıcı adı, e-posta ve şifre bilgileri alınıp veritabanına eklenir.
  - *RegisterScreenDatabase* sınıfı, kullanıcı verilerini kayit tablosuna kaydeder.
  - *YeniYerEkleScreen*, kullanıcıların yer kategorisine ait yeni kayıtlar eklemesini sağlar. Yer adı, açıklama ve fotoğraf bilgileri alınıp veritabanına kaydedilir.
  - *YeniYerEkleScreenDatabase* sınıfı, bu bilgileri veritabanındaki user_images tablosuna ekler.

**2. Read(Okuma)**
- Projede verilerin okunması ve kullanıcıya gösterilmesi için mekanizmalar tasarlanmıştır:
  - Yerler sınıfı, yer kategorisindeki tüm kayıtları kullanıcıya grid düzeninde gösterir.
  - Veriler, veritabanından SELECT sorgusu ile okunur. Okunan fotoğraflar ve yer adları bir panel üzerinde dinamik olarak listelenir.
  - *Profil* sınıfı, kullanıcıya “Hoş Geldiniz” mesajı ve kullanıcı adı gibi bilgileri gösterir. Kullanıcı adı dinamik olarak profilde güncellenir.
 
**3. Update(Güncelleme)**
- Projede verilerin güncellenmesi için kullanıcıya düzenleme ekranları sağlanmıştır:
  - *SifreDegistir* sınıfı, kullanıcının mevcut şifresini kontrol ederek yeni şifreyi günceller.
  - *KullaniciAdiDegistir* sınıfı, kullanıcı adının güncellenmesini sağlar.
  - *YerEditScreen*, kullanıcının bir yer kaydına ait adı, açıklamayı ve fotoğrafı düzenlemesine olanak tanır.

**4. Delete(Silme)**
- Projede kullanıcıların ve anıların silinmesi için seçenekler sunulmuştur:
  - *HesapSil* sınıfı, kullanıcının hesabını kayıt tablosundan siler. Kullanıcının hesabı silindikten sonra uygulama kapanır.
  - *Yerler* sınıfındaki “Sil” butonu, bir yer kaydını veritabanından kaldırır. Silinen kayıt, arayüzden de dinamik olarak kaldırılır



