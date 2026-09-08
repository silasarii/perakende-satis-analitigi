# 📊 Perakende Satış & Kârlılık Performans Paneli

Bu proje, perakende sektöründeki satış dinamiklerini, kârlılığı, bölgesel performans farklılıklarını ve ürün kategorilerini analiz etmek amacıyla Power BI ile uçtan uca geliştirilmiş etkileşimli bir analitik panelidir.

---

## 🎯 Proje Hedefleri & İş Soruları

* **Finansal Performans:** Toplam ciro, kâr ve sipariş hacmi ne seviyede?
* **Bölgesel Dağılım:** Hangi coğrafi bölgeler satışlarda başı çekiyor?
* **Zaman Serisi & Sezonsallık:** Satışlar yıl içinde nasıl dalgalanıyor, belirli dönemlerde (örneğin yıl sonu veya yaz başlangıcı) ani sıçramalar var mı?
* **Kategori Kırılımı:** Hangi ana ve alt kategoriler en yüksek kâr ve satış hacmini üretiyor?

---

## 🛠️ Teknik Yetkinlikler & Yöntem

* **Power Query (ETL & Veri Temizleme):**
  * Tüm veri setine yönelik detaylı veri profillemesi yapıldı.
  * Eksik, hatalı ve boş (`null`) değerler mantıksal kurallarla dönüştürüldü.
  * Metin standardizasyonu ve doğru veri tipi atamaları sağlandı.

* **Veri Modelleme (Star Schema Mantığı):**
  * Fact tablosu (`Satışlar`) ile Boyut tablosu (`Ürünler`) arasında 1:* (Bire Çok) ilişki kurgulandı.
  * İlişki yönü ve filtre akışı doğru işlenecek şekilde model optimize edildi.

* **DAX & Ölçü Mimarisi (Measure Branching):**
  * Dinamik satır bazlı kümülatif hesaplamalar için `SUMX` fonksiyonu kullanıldı.
  * Çoklu ürün sepetlerini hatasız saymak için `DISTINCTCOUNT` uygulandı.
  * Ölçü dallandırma (Measure Branching) yöntemiyle formüller sade ve sürdürülebilir kılındı:
    * `Toplam Satış Tutarı = SUMX('Satışlar', 'Satışlar'[Adet] * 'Satışlar'[Birim Satış Fiyatı])`
    * `Toplam Sipariş = DISTINCTCOUNT('Satışlar'[Sipariş Kodu])`
    * `Toplam Maliyet = SUMX('Satışlar', 'Satışlar'[Adet] * 'Satışlar'[Birim Maliyet])`
    * `Toplam Kar = [Toplam Satış Tutarı] - [Toplam Maliyet]`

* **Görselleştirme & Dashboard Mimarisi:**
  * Yönetici göz akışına uygun "Z Düzeni" hiyerarşisi uygulandı.
  * KPI kartları, yatay çubuk grafik (bölgesel kıyas), trend çizgisi (aylık akış) ve açılır-kapanır hiyerarşik matris tablosu bir arada sunuldu.
  * Tam dinamik çapraz filtreleme yapısı kurgulandı.

---

## 💡 Öne Çıkan Analitik Çıkarımlar

* **Bölgesel Lider:** Marmara bölgesi toplam ciroda açık ara lider konumda yer alırken, Ege ve İç Anadolu birbirine çok yakın performans sergilemektedir.
* **Sezonsallık Etkisi:** Satış trendinde Haziran ayında belirgin bir ara zirve, Eylül ayından itibaren ise yıl sonuna doğru güçlü bir yükseliş dalgası gözlemlenmektedir.
* **Kategori Performansı:** Temizlik ve Temel Gıda kategorileri en yüksek gelir ve kâr katkısını sağlamaktadır.

---

## 📁 Proje Dosya Yapısı

* `/data`: Ham ve temizlenmiş veri setleri
* `dashboard.png`: Dashboard'un görsel önizlemesi
* `*.pbix`: Power BI çalışma dosyası
