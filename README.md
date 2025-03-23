# METRO AĞI SİMULASYONU VE ROTA PLANLAMA

Bu proje, bir metro ağı üzerinde en az aktarmalı ve en hızlı rotaları bulur vebu metro ağını temsil eden bir graf yapısı oluşturur. Proje, kullanıcıların iki istasyon arasında en uygun rotayı bulmasına yardımcı olur. Ayrıca, metro ağını görselleştirerek harita üzerinde gösterir.

## KÜTÜPHANELER VE KULLANILAN TEKNOLOJİLER

* networkx: Graf oluşturma, yönetme ve görselleştirme işlemleri için kullanıldı.

* matplotlib: Grafın görselleştirilmesi ve haritanın çizilmesi için kullanıldı.

* heapq: Dijkstra algoritmasında öncelikli kuyruk (priority queue) yapısını uygulamak için kullanıldı.

* collections:

  * defaultdict: Hatların istasyon listelerini saklamak için kullanıldı.

  * deque: BFS algoritmasında kuyruk yapısını uygulamak için kullanıldı.

* typing: Python'da tür ipuçları (type hints) eklemek için kullanıldı.

## ALGORİTMALARIN ÇALIŞMA MANTIĞI

### BFS (Breadth-First Search) Algoritması:
  
  * BFS, bir graf üzerinde genişliğine arama yapar. Başlangıç düğümünden başlar ve tüm komşularını ziyaret eder.
  
  * Her adımda, bir düğümün tüm komşuları keşfedilir ve kuyruğa eklenir.
  
  * Hedef düğüme ulaşıldığında, en kısa yol (en az aktarmalı yol) bulunmuş olur.
  
   Neden Kullanıldı:question::
  * En az aktarmalı rotayı bulmak için idealdir çünkü BFS, hedefe en kısa adım sayısıyla ulaşır.

### A* Algoritması:

  * A*, hem başlangıç düğümünden mevcut düğüme olan gerçek maliyeti (g(n)), hem de mevcut düğümden hedef düğüme olan tahmini maliyeti (h(n)) kullanır.
    
  * Toplam maliyet f(n) = g(n) + h(n) şeklinde hesaplanır.
    
  * Öncelikli kuyruk (priority queue) kullanarak, en düşük f(n) değerine sahip düğümleri öncelikle işler.
    
  * Hedef düğüme ulaşıldığında, en kısa süreli rota bulunmuş olur.
  
  Neden Kullanıldı:question::
  * En iyi çözümü garanti eder (eğer sezgisel fonksiyon doğruysa).
    
  * Özellikle büyük ölçekli graf yapılarında daha verimlidir.


## ÖRNEK KULLANIM VE TEST SONUÇLARI



```
## Metro ağı oluştur
metro = MetroAgi()

## İstasyonlar ekle
metro.istasyon_ekle("K1", "Kızılay", "Kırmızı Hat")
metro.istasyon_ekle("K2", "Ulus", "Kırmızı Hat")
metro.istasyon_ekle("K3", "Demetevler", "Kırmızı Hat")
metro.istasyon_ekle("K4", "OSB", "Kırmızı Hat")

### Mavi Hat
metro.istasyon_ekle("M1", "AŞTİ", "Mavi Hat")
metro.istasyon_ekle("M2", "Kızılay", "Mavi Hat")  # Aktarma noktası
metro.istasyon_ekle("M3", "Sıhhiye", "Mavi Hat")
metro.istasyon_ekle("M4", "Gar", "Mavi Hat")

### Turuncu Hat
metro.istasyon_ekle("T1", "Batıkent", "Turuncu Hat")
metro.istasyon_ekle("T2", "Demetevler", "Turuncu Hat")  # Aktarma noktası
metro.istasyon_ekle("T3", "Gar", "Turuncu Hat")  # Aktarma noktası
metro.istasyon_ekle("T4", "Keçiören", "Turuncu Hat")

## Bağlantılar ekle
### Kırmızı Hat bağlantıları
metro.baglanti_ekle("K1", "K2", 4)  # Kızılay -> Ulus (4 dakika)
metro.baglanti_ekle("K2", "K3", 6)  # Ulus -> Demetevler (6 dakika)
metro.baglanti_ekle("K3", "K4", 8)  # Demetevler -> OSB (8 dakika)

### Mavi Hat bağlantıları
metro.baglanti_ekle("M1", "M2", 5)  # AŞTİ -> Kızılay (5 dakika)
metro.baglanti_ekle("M2", "M3", 3)  # Kızılay -> Sıhhiye (3 dakika)
metro.baglanti_ekle("M3", "M4", 4)  # Sıhhiye -> Gar (4 dakika)

### Turuncu Hat bağlantıları
metro.baglanti_ekle("T1", "T2", 7)  # Batıkent -> Demetevler (7 dakika)
metro.baglanti_ekle("T2", "T3", 9)  # Demetevler -> Gar (9 dakika)
metro.baglanti_ekle("T3", "T4", 5)  # Gar -> Keçiören (5 dakika)

### Hat aktarma bağlantıları (aynı istasyon farklı hatlar)
metro.baglanti_ekle("K1", "M2", 2)  # Kızılay aktarma (2 dakika)
metro.baglanti_ekle("K3", "T2", 3)  # Demetevler aktarma (3 dakika)
metro.baglanti_ekle("M4", "T3", 2)  # Gar aktarma (2 dakika)

## Test senaryoları
print("\n=== Test Senaryoları ===")

### Senaryo 1: AŞTİ'den OSB'ye
print("\n1. AŞTİ'den OSB'ye:")
rota = metro.en_az_aktarma_bul("M1", "K4")
if rota:
    print("En az aktarmalı rota:", " -> ".join(i.ad for i in rota))

sonuc = metro.en_hizli_rota_bul("M1", "K4")
if sonuc:
    rota, sure = sonuc
    print(f"En hızlı rota ({sure} dakika):", " -> ".join(i.ad for i in rota))

### Senaryo 2: Batıkent'ten Keçiören'e
print("\n2. Batıkent'ten Keçiören'e:")
rota = metro.en_az_aktarma_bul("T1", "T4")
if rota:
    print("En az aktarmalı rota:", " -> ".join(i.ad for i in rota))

sonuc = metro.en_hizli_rota_bul("T1", "T4")
if sonuc:
    rota, sure = sonuc
    print(f"En hızlı rota ({sure} dakika):", " -> ".join(i.ad for i in rota))

### Senaryo 3: Keçiören'den AŞTİ'ye
print("\n3. Keçiören'den AŞTİ'ye:")
rota = metro.en_az_aktarma_bul("T4", "M1")
if rota:
    print("En az aktarmalı rota:", " -> ".join(i.ad for i in rota))

sonuc = metro.en_hizli_rota_bul("T4", "M1")
if sonuc:
    rota, sure = sonuc
    print(f"En hızlı rota ({sure} dakika):", " -> ".join(i.ad for i in rota))

### Senaryo 4: Ulus'tan Keçiören'e
print("\n4. Ulus'tan Keçiören'e:")
rota = metro.en_az_aktarma_bul("K2", "T4")
if rota:
    print("En az aktarmalı rota:", " -> ".join(i.ad for i in rota))

sonuc = metro.en_hizli_rota_bul("K2", "T4")
if sonuc:
    rota, sure = sonuc
    print(f"En hızlı rota ({sure} dakika):", " -> ".join(i.ad for i in rota))

### Metro haritasını çiz
metro_map(metro)




