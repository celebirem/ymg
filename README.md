# İşletim Sistemleri Uygulama Ödevi

## Amaç  
Bu proje, ödev kapsamında verilen iki soruyu çözmeyi amaçlar: 
1. Çoklu thread kullanımı ile basit sayma problemi 
2. Thread ve semaforlar ile yemekhane kuyruğu senkronizasyonu.  

---

## Soru 1: Çoklu Thread Kullanımı ile Basit Sayma Problemi  

### Açıklama  
Bu problemde, bir sınıfta toplam 50 öğrenci 5 gruba ayrılmıştır. Her grup için bir thread oluşturulmuştur. Her thread, temsil ettiği grubun 10 öğrencisini kontrol eder ve ardından bir temsilci seçildiğini bildirir.

### Çalışma Mantığı  
1. **Grup Thread'leri**:  
   - `Grup` adında bir thread sınıfı tanımlandı. Her thread bir grubun öğrencilerini temsil eder.  

2. **Thread Yönetimi**:  
   - Toplam öğrenci sayısı, grup sayısına bölünerek her grup için eşit öğrenci atanmıştır (Grup başına 1o öğrenci).  
   - Her thread başlatıldığında, temsil ettiği grubun tüm öğrencilerini kontrol eder ve mesaj yazdırır.  

3. **Sonuç Mesajı**:  
   - Tüm thread'lerin çalışması tamamlandığında, her grubun temsilci seçtiğini belirten bir mesaj yazdırılır.

### Kod Çıktısı
- Kodu çalıştırdıktan sonra, çıktı şu şekilde olur:

```Soru 1
Grup 1: 10 öğrenci kontrol edildi.
Grup 2: 10 öğrenci kontrol edildi.
...
Her grup bir temsilci seçti.
```
---

## Soru 2: Thread ve Semafor Kullanarak Yemekhane Kuyruğu Senkronizasyonu  

### Açıklama  
Bu problemde, bir yemekhanede 3 tezgah ve yemek almak isteyen 50 öğrenci var. Her tezgahın aynı anda en fazla 5 öğrenciye hizmet verebileceği varsayılmıştır. Thread ve semaforlar yardımıyla bu süreç doğru şekilde senkronize edilmiştir.

### Çalışma Mantığı
1. **Tezgah Thread'leri**:  
- `Tezgah` adında bir thread sınıfı tanımlandı. Her thread bir tezgahı temsil eder.  
- Her thread, bir semafor kullanarak tezgah kapasitesini yönetir.

2. **Semafor Kullanımı**:  
- Semafor, her tezgah için aynı anda en fazla 5 öğrencinin hizmet almasına izin verir.  
- Eğer bir tezgah dolarsa (5 öğrenci alındıysa), "Tezgah X doldu, öğrenciler bekliyor." mesajı yazdırılır.  

3. **Senkronizasyon**:  
- `lock` kullanılarak, tüm thread'lerin aynı global değişkeni (`ogrenci_kaldi`) düzgün bir şekilde güncellemesi sağlanır.  

4. **Sonuç Mesajı**:  
- Tüm öğrenciler yemek aldıktan sonra "Tüm öğrenciler yemek aldı. Program sonlanıyor." mesajı yazdırılır.  

### Kod Çıktısı
- Kodu çalıştırdıktan sonra, çıktı şu şekilde olur:

```Soru 2
Tezgah 1: 1 öğrenci yemek aldı. Kalan öğrenci: 49
Tezgah 2: 1 öğrenci yemek aldı. Kalan öğrenci: 48
...
Tezgah 1 doldu, öğrenciler bekliyor.
...
Tüm öğrenciler yemek aldı. Program sonlanıyor.
```
---

## Sonuç
Projede çözülmesi beklenen iki soru da belirlenen kriterlere uygun olarak çözülmüş ve istenen çıktılar elde edilmiştir.
