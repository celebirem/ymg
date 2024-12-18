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
   - Toplam öğrenci sayısı, grup sayısına bölünerek her grup için eşit öğrenci atanmıştır (10 öğrenci/grup).  
   - Her thread başlatıldığında, temsil ettiği grubun tüm öğrencilerini kontrol eder ve mesaj yazdırır.  

3. **Sonuç Mesajı**:  
   - Tüm thread'lerin çalışması tamamlandığında, her grubun temsilci seçtiğini belirten bir mesaj yazdırılır.

### Kodun Kullanımı
1. Kodunuzu çalıştırmak için bir Python derleyicisi (örneğin: Python 3.7 veya üstü) gereklidir.
2. Kodu çalıştırdıktan sonra, çıktıda her grup için şu mesajları görürsünüz:

```Soru 1
Grup 1: 10 öğrenci kontrol edildi.
Grup 2: 10 öğrenci kontrol edildi.
...
Her grup bir temsilci seçti.
```

### Kod Öne Çıkan Noktalar
- Thread’ler doğru bir şekilde oluşturulmuş ve başlatılmıştır.  
- Her grup eşit öğrenci sayısıyla senkronize edilmiştir.  

---

## Soru 2: Thread ve Semafor Kullanarak Yemekhane Kuyruğu Senkronizasyonu  

### Açıklama  
Bu problemde, bir üniversite yemekhanesindeki 3 tezgahı ve yemek almak isteyen 50 öğrenciyi simüle ediyoruz. Her tezgahın aynı anda en fazla 5 öğrenciye hizmet verebileceği varsayılmıştır. Thread ve semaforlar yardımıyla bu süreç doğru şekilde senkronize edilmiştir.

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

### Kodun Kullanımı
1. Kodunuzu çalıştırmak için bir Python derleyicisi gereklidir.  
2. Çıktıda şu mesajları görürsünüz:

```Soru 2
Tezgah 1: 1 öğrenci yemek aldı. Kalan öğrenci: 49
Tezgah 2: 1 öğrenci yemek aldı. Kalan öğrenci: 48
...
Tezgah 1 doldu, öğrenciler bekliyor.
...
Tüm öğrenciler yemek aldı. Program sonlanıyor.
```

### Kod Öne Çıkan Noktalar
- Semafor, her tezgahın kapasitesini sınırlandırmak için doğru şekilde kullanılmıştır.  
- Senkronizasyon, birden fazla thread'in aynı anda düzgün çalışmasını sağlamıştır.  
- Çıktı formatı verilen kriterlere tam olarak uyacak şekilde düzenlenmiştir.  

---

## Genel Bilgiler
- **Gereksinimler**:
- Python 3.7 veya üstü.  
- Çok çekirdekli işlemci olması, thread'lerin paralel çalışmasını daha iyi gözlemlemenizi sağlar.  

- **Öne Çıkan Teknikler**:
- **Thread Yönetimi**: Python’un `threading` modülü ile thread’ler oluşturulmuş ve yönetilmiştir.  
- **Semafor Kullanımı**: Kapasite sınırlamaları ve kaynak paylaşımı semaforlarla sağlanmıştır.  
- **Senkronizasyon**: `Lock` ile thread'ler arasında veri tutarlılığı sağlanmıştır.  

---

## Dosya Yapısı
- **soru1.py**: Bir sınıfı 5 gruba ayırarak çoklu thread yönetimini simüle eder.  
- **soru2.py**: Yemekhanedeki tezgah kapasitesini ve öğrenci senkronizasyonunu simüle eder.  

---

## Sonuç
Bu proje, thread ve semafor gibi işletim sistemi kavramlarını uygulamalı olarak anlamanıza yardımcı olur. Her iki soru da belirlenen kriterlere uygun olarak çözülmüş ve test edilmiştir.
