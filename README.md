# REMIT - Anı Yönetim Uygulaması

REMIT, kullanıcıların yerler, oyunlar, tarifler, izlenceler, okumalar ve dinlenceler gibi kategoriler altında anılarını kaydedebileceği, düzenleyebileceği ve yönetebileceği bir masaüstü uygulamasıdır. Uygulama, Java Swing kullanılarak geliştirilmiştir ve MySQL veritabanı ile çalışmaktadır.

## Özellikler

- **Kullanıcı Kaydı ve Girişi**
  - Kullanıcılar hesap oluşturabilir, giriş yapabilir ve şifrelerini değiştirebilir.
- **Profil Yönetimi**
  - Kullanıcılar profil bilgilerini görüntüleyebilir, kullanıcı adını ve şifresini güncelleyebilir.
- **Yerler Modülü**
  - Kullanıcılar yer kategorisinde yeni anılar ekleyebilir, mevcut anılarını düzenleyebilir veya silebilir.
- **Kategoriler**
  - Yerler, oyunlar, tarifler, izlenceler, okumalar ve dinlenceler için ayrı kategoriler bulunmaktadır.
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
│   └── DikdortgenGUI.java              # Ana kategori ekranı
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
Ana sınıf olarak OpeningScreen veya SecondScreen sınıflarını çalıştırarak uygulamayı başlatın.

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

#### `DikdortgenGUI`
- Anıların kategorilere ayrıldığı ana ekran.
- Her kategori için özel ikonlar ve yönlendirme bağlantıları içerir.
- "Profilim" düğmesi ile kullanıcı profil sayfasına geçiş yapılır.

---

### **5. Yerler Modülü**

#### `Yerler`
- Kullanıcının yer kategorisine ait fotoğraflarını görüntülediği ve yönetebildiği arayüzdür.
- Fotoğraflar grid düzeninde gösterilir.

#### `YerEditScreen`
- Kullanıcının bir fotoğrafın adını, açıklamasını ve görselini düzenlemesini sağlar.

#### `YeniYerEkleScreen`
- Yeni bir yer eklemek için kullanıcıya form sunar.
- Fotoğraf yükleme ve önizleme desteği içerir.

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
