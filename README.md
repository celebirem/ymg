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
  
### Kod
```Soru 1
import threading

# Grup thread sınıfı
class Grup(threading.Thread):
    def __init__(self, grup_no, ogrenci_sayisi):
        super().__init__()
        self.grup_no = grup_no
        self.ogrenci_sayisi = ogrenci_sayisi

    def run(self):
        # Öğrencileri kontrol etme
        print(f"Grup {self.grup_no}: {self.ogrenci_sayisi} öğrenci kontrol edildi.")

# Toplam öğrenci sayısı ve grup sayısı
toplam_ogrenci = 50
grup_sayisi = 5
ogrenci_per_grup = toplam_ogrenci // grup_sayisi

# Threadleri başlatma
grup_threadleri = []
for i in range(grup_sayisi):
    grup = Grup(grup_no=i + 1, ogrenci_sayisi=ogrenci_per_grup)
    grup_threadleri.append(grup)
    grup.start()

for thread in grup_threadleri:
    thread.join()

print("Her grup bir temsilci seçti.")

```

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

### Kod
```Soru 2
import threading
import time
import random

# Yemekhane semaforu
tezgah_kapasitesi = 5
toplam_ogrenci = 50
tezgah_sayisi = 3

semafor = threading.Semaphore(tezgah_kapasitesi)

# Ortak sayaç için kilit
lock = threading.Lock()
ogrenci_kaldi = toplam_ogrenci

# Tezgah thread sınıfı
class Tezgah(threading.Thread):
    def __init__(self, tezgah_no):
        super().__init__()
        self.tezgah_no = tezgah_no
        self.local_count = 0  # Tezgahın hizmet verdiği öğrenci sayısı

    def run(self):
        global ogrenci_kaldi

        while True:
            # Semafor izni alma işlemi
            semafor.acquire()
            with lock:
                if ogrenci_kaldi > 0:
                    ogrenci_kaldi -= 1
                    self.local_count += 1
                    print(f"Tezgah {self.tezgah_no}: 1 öğrenci yemek aldı. Kalan öğrenci: {ogrenci_kaldi}")
                else:
                    semafor.release()
                    break

            # Rastgele gecikme
            time.sleep(random.uniform(0.1, 0.5))

            # Tezgah dolduysa mesaj yazdılır
            if self.local_count % tezgah_kapasitesi == 0:
                print(f"Tezgah {self.tezgah_no} doldu, öğrenciler bekliyor.")
                time.sleep(random.uniform(0.2, 0.5))  # Bekleme süresi

            semafor.release()

# Tezgahları başlatma
tezgahlar = []
for i in range(tezgah_sayisi):
    tezgah = Tezgah(tezgah_no=i + 1)
    tezgahlar.append(tezgah)
    tezgah.start()

# Tezgahların tamamlanmasını bekleme
for tezgah in tezgahlar:
    tezgah.join()

print("Tüm öğrenciler yemek aldı. Program sonlanıyor.")

```

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
