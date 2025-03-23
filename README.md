# METRO SİMULASYONU VE ROTA PLANLAMA

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
```
```
## Bağlantılar ekle
### Kırmızı Hat bağlantıları
metro.baglanti_ekle("K1", "K2", 4)  # Kızılay -> Ulus (4 dakika)
metro.baglanti_ekle("K2", "K3", 6)  # Ulus -> Demetevler (6 dakika)
metro.baglanti_ekle("K3", "K4", 8)  # Demetevler -> OSB (8 dakika)
```
```
### Hat aktarma bağlantıları (aynı istasyon farklı hatlar)
metro.baglanti_ekle("K1", "M2", 2)  # Kızılay aktarma (2 dakika)
metro.baglanti_ekle("K3", "T2", 3)  # Demetevler aktarma (3 dakika)
metro.baglanti_ekle("M4", "T3", 2)  # Gar aktarma (2 dakika)
```

## TEST SENARYOLARI

```
### Senaryo 1: AŞTİ'den OSB'ye
print("\n1. AŞTİ'den OSB'ye:")
rota = metro.en_az_aktarma_bul("M1", "K4")
if rota:
    print("En az aktarmalı rota:", " -> ".join(i.ad for i in rota))

sonuc = metro.en_hizli_rota_bul("M1", "K4")
if sonuc:
    rota, sure = sonuc
    print(f"En hızlı rota ({sure} dakika):", " -> ".join(i.ad for i in rota))
```

### METRO HARİTASI GRAFİĞİ OLUŞTURMA
```
def metro_map(metro: MetroAgi):

    G = nx.Graph()

    for istasyon_id, istasyon in metro.istasyonlar.items():
        G.add_node(istasyon_id, ad=istasyon.ad, hat=istasyon.hat)

        for komsu, sure in istasyon.komsular:
            G.add_edge(istasyon_id, komsu.idx, weight=sure)


    position = nx.kamada_kawai_layout(G)

    hat_renkleri = {
        "Kırmızı Hat": "red",
        "Mavi Hat": "blue",
        "Turuncu Hat": "orange"}

    labels = {node: metro.istasyonlar[node].ad for node in G.nodes()}
    renkler = [hat_renkleri[metro.istasyonlar[node].hat] for node in G.nodes()]

    plt.figure(figsize=(12, 8))
    nx.draw(G, position, with_labels=True, labels=labels, node_color=renkler, node_size=600, font_size=8, font_weight='bold', edge_color='gray')
    edge_labels = nx.get_edge_attributes(G, 'weight')
    for k in edge_labels:
        edge_labels[k] = f"{edge_labels[k]} dk"

    nx.draw_networkx_edge_labels(G, position, edge_labels=edge_labels)
    plt.title("Metro Haritası")
    plt.show()
metro_map(metro)
```
## OUTPUTS
![Alt text](main/metroMap.png)




