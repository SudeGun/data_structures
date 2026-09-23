# Data Structures & Algorithms Lab Projects

Bu repo, **BBM 203: Software Laboratory I (Data Structures)** dersi kapsamında C++11 standardında geliştirilmiş veri yapıları ve simülasyon projelerini içermektedir.

Tüm veri yapıları (Bağlı Listeler, Döngüsel Kuyruklar, Yığınlar, AVL ve Kırmızı-Siyah Ağaçlar), STL konteynerleri (`std::vector`, `std::list`, `std::queue`, `std::stack`, `std::map`) kullanılmadan, **sıfırdan dinamik bellek yönetimi (`new` / `delete`) ve gösterici (pointer) manipülasyonlarıyla** manuel olarak uygulanmıştır.

---

## 📁 Proje İçerikleri

### 1. [PA1: Rail Marshal System (H.U.T.T.)](./203-1)
* **Kapsam:** Ulusal demiryolu yük ağı için sınıflandırma, tren oluşturma ve sevk simülasyonu.
* **Kullanılan Veri Yapıları:**
  * **İki Boyutlu Bağlı Liste Matrisi (Classification Yard):** Hedef ve kargo türüne göre indekslenen, hücrelerinde vagonları ağırlıklarına göre azalan sırada tutan yapı (`blockTrains[Destination][CargoType]`).
  * **Çift Yönlü Bağlı Liste (Doubly Linked List - `WagonList`):** Vagonların eklenmesi (`insertSorted`), ağırlık ve güvenlik kurallarına göre birleştirilmesi (`appendList`), dinamik ayrılması (`detach`) ve bellek tahliyesi (`clear`).
  * **Bekleme Hattı / Kuyruk Mantığı (`TrainTrack`):** Sadece arkadan ekleme yapılan ve yalnızca en öndeki trenin ayrılabildiği sevk hattı.
* **Öne Çıkan Algoritma ve Kurallar:**
  * **Güvenli Katar Birleştirme:** Aynı kargo grubundaki vagonların bir arada tutulması, en ağır vagona göre blokların sıralanması.
  * **Tehlikeli Madde (Hazardous) Kısıtı:** Bir trende en fazla bir tehlikeli vagon bulunabilmesi ve ağırlığına bakılmaksızın lokomotiften en uzak noktada (en sonda) konumlandırılması.
  * **Dinamik Kuplör Emniyeti & Tren Bölme (Split):** Sondan başa doğru kümülatif yük kontrolü yapılarak `maxCouplerLoad` limitini aşan noktadan trenin dinamik olarak bölünmesi ve yeni tren olarak hatta eklenmesi.
  * **Otomatik Sevk (Auto-Dispatch):** Hat toplam ağırlığı belirlenen eşik değeri aştığında öndeki trenin sevk edilmesi.

---

### 2. [PA2: QuakeAssist Emergency Coordination System (TDRA)](./203-2)
* **Kapsam:** Deprem sonrası lojistik destek ve arama-kurtarma operasyonlarını koordine eden acil durum yönetim simülasyonu.
* **Kullanılan Veri Yapıları:**
  * **Dinamik Döngüsel Kuyruk (`RequestQueue` - FIFO):** Statik kapasite dolduğunda otomatik olarak kapasitesini ikiye katlayan (dynamic circular array) yardım (`Supply`) ve kurtarma (`Rescue`) kuyrukları.
  * **Dinamik Yığın (`MissionStack` - LIFO):** Ekiplere atanan görevleri tutan, kapasite aşımında büyüyebilen görev yığını.
* **Öne Çıkan Algoritma ve Kurallar:**
  * **Acil Durum Karar Motoru (Emergency Decision Engine):** Kuyruk başlarındaki talepler için dinamik skorlama:
    $$\text{Supply Score} = \text{EmergencyLevel} \times 10 + \min(\text{amount}, 50)$$
    $$\text{Rescue Score} = \text{EmergencyLevel} \times 10 + (\text{numPeople} \times \text{riskMultiplier})$$
    Eşitlik durumunda kurtarma (Rescue) görevlerine öncelik tanınması.
  * **Görev Aşımı ve Geri Alma (Rollback Mechanism):** Bir ekibin anlık iş yükü `maxLoadCapacity` değerini aştığında, yığındaki atanmış tüm taleplerin LIFO sırasıyla pop edilip kuyruklara orijinal sıraları bozulmadan geri iade edilmesi (`re-enqueue`).

---

### 3. [PA3: Artifact Stewardship System (H.U.D.A.M.)](./203-3)
* **Kapsam:** Tarihi araştırma eserlerinin kataloglanması, araştırmacılara atanması ve dengeli veri yapılarında yönetilmesi.
* **Kullanılan Veri Yapıları:**
  * **AVL Ağacı (`AVLTree` - Artifact Catalog):** Eser ID'sine göre sıralanan, ekleme ve silme işlemlerinde denge faktörünü (balance factor) korumak için tek ve çift rotasyonlar (LL, RR, LR, RL) uygulayan $\mathcal{O}(\log N)$ arama garantili yapı.
  * **Kırmızı-Siyah Ağaç (`RedBlackTree` - Researcher Directory):** Yüksek araştırmacı sirkülasyonunu yönetmek için renk dönüşümleri, sol/sağ rotasyonlar ve silme düzeltme (delete fix-up) kurallarıyla dengelenen yapı.
* **Öne Çıkan Algoritma ve Kurallar:**
  * **Çift Yapı Senkronizasyonu (Cross-Tree Synchronization):** Araştırmacı kapasitesi dolmadığı sürece eserin araştırmacıya bağlanması, eser değerinin artırılması ve araştırmacı sistemden çıkarıldığında (`FIRE_RESEARCHER`) denetlediği tüm eserlerin otomatik olarak boşa çıkarılması (`unassigned`).
  * **Ağaç Dolaşımları (Tree Traversals):** RBT düğümlerinin seviye bazlı renk bilgileriyle yazdırılması; eserlerin post-order, araştırmacıların ise pre-order sıralama ile istatistiksel raporlanması.

---

## 🛠 Kullanılan Teknolojiler ve Standartlar

* **Dil:** C++11
* **Veri Yapıları:** 
  * Doubly Linked List
  * Circular Dynamic Queue
  * Dynamic Array Stack
  * AVL Tree
  * Red-Black Tree
* **Bellek & Gösterici Yönetimi:** Raw Pointers, Dynamic Memory Allocation (`new`/`delete`), Destructor temizliği (Memory Leak / Dangling Pointer önleme)
* **Girdi / Çıktı:** Dosya akışları (File I/O) ve komut ayrıştırıcı (Command Parsing)
