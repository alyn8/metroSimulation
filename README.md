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
  
  BFS, bir graf üzerinde genişliğine arama yapar. Başlangıç düğümünden başlar ve tüm komşularını ziyaret eder.
  
  Her adımda, bir düğümün tüm komşuları keşfedilir ve kuyruğa eklenir.
  
  Hedef düğüme ulaşıldığında, en kısa yol (en az aktarmalı yol) bulunmuş olur.
  
  Neden Kullanıldı :question: :
  En az aktarmalı rotayı bulmak için idealdir çünkü BFS, hedefe en kısa adım sayısıyla ulaşır.

### A* Algoritması:

  A*, hem başlangıç düğümünden mevcut düğüme olan gerçek maliyeti (g(n)), hem de mevcut düğümden hedef düğüme olan tahmini maliyeti (h(n)) kullanır.
  Toplam maliyet f(n) = g(n) + h(n) şeklinde hesaplanır.
  Öncelikli kuyruk (priority queue) kullanarak, en düşük f(n) değerine sahip düğümleri öncelikle işler.
  Hedef düğüme ulaşıldığında, en kısa süreli rota bulunmuş olur.
  
  Neden Kullanıldı :question: :
  En iyi çözümü garanti eder (eğer sezgisel fonksiyon doğruysa).
  Özellikle büyük ölçekli graf yapılarında daha verimlidir.    

