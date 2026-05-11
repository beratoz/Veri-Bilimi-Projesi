# S&P 500 Sektör ETF'leri — Makroekonomik Olay Çalışması (Event Study) 📊

Bu proje, küresel çapta etki yaratan büyük makroekonomik olayların (seçimler, pandemiler, savaşlar) S&P 500 sektör ETF'leri ve temel emtialar üzerindeki fiyatlama ve volatilite davranışlarını inceleyen bir finansal veri bilimi çalışmasıdır.

Çalışma, olayların varlık fiyatları üzerindeki etkisini ölçmek için literatürde sıkça kullanılan istatistiksel bir yöntem olan **Olay Çalışması (Event Study)** metodolojisi ile hazırlanmıştır.

---

## 🎯 Proje Amacı

Finansal piyasalar, makroekonomik şoklara tek tip bir tepki vermez. Bazı sektörler bu şoklardan derin yaralar alırken, bazıları defansif karakter göstererek piyasadan pozitif ayrışabilir. Bu projenin temel amacı:

- Şok anlarında sektörlerin nasıl tepki verdiğini ölçmek,
- Klasik "güvenli liman" algılarını veri ile test etmek,
- Portföy yöneticileri için risk yönetimine dair rasyonel, eyleme dönüştürülebilir stratejiler üretmektir.

---

## 📂 Veri ve Metodoloji

Çalışmada, piyasanın genel yönünü temsil etmek üzere **S&P 500 Endeks ETF'i (SPY)** baz alınmış ve varlıkların piyasadan ayrışma seviyeleri **Anormal Getiri (AR)** ve **Kümülatif Anormal Getiri (CAR)** metrikleriyle ölçülmüştür.

- **Anormal Getiri (AR):** Hisse/Sektör Logaritmik Getirisi − SPY Logaritmik Getirisi
- **Kümülatif Anormal Getiri (CAR):** Olay penceresi boyunca AR değerlerinin birikimli toplamı.

### İncelenen Makroekonomik Olaylar (5 Adet)

| # | Tarih | Olay | Kategori |
|---|---|---|---|
| 1 | Kasım 2016 | ABD Başkanlık Seçimleri | Politik |
| 2 | Mart 2020 | COVID-19 Pandemisi | Sağlık / Ekonomik |
| 3 | Şubat 2022 | Rusya – Ukrayna Savaşı | Jeopolitik |
| 4 | Nisan 2024 | İsrail – İran Gerilimi | Jeopolitik |
| 5 | Kasım 2024 | ABD Başkanlık Seçimleri | Politik |

### Sektör ve Varlık Evreni

Analiz, 3 gruba ayrılmış **10 farklı varlık** üzerinden gerçekleştirilmiştir:

- **Odak Sektörler:** `XLK` (Teknoloji), `XLF` (Finans), `XLE` (Enerji), `XLV` (Sağlık), `ITA` (Savunma Sanayi)
- **Kontrol Grubu:** `XLP` (Tüketim Temel), `XLY` (Tüketim İsteğe Bağlı), `XLRE` (Gayrimenkul)
- **Emtia / Makro:** `GLD` (Altın), `CL=F` (Ham Petrol – WTI)

### Olay Pencereleri

Farklı zaman dilimlerindeki etkileri ölçmek için üç farklı pencere kullanılmıştır:

- **T = 0 (Olay Günü):** Anlık şok tepkisi.
- **[−3, +3] Kısa Pencere:** Olayın 7 günlük kısa vadeli sindirilme süreci.
- **[−10, +10] Geniş Pencere:** Geciken tepkileri ve 21 günlük tam fiyatlanma süreci.

---

## 📈 Temel Bulgular ve Sonuçlar

Jupyter Notebook üzerinde yapılan Keşifsel Veri Analizi (EDA) ve İstatistiksel (T-Testi) değerlendirmeler sonucunda şu ekonomik eğilimler saptanmıştır:

- **Negatif Ayrışan Sektörler:** Ham Petrol (`CL=F`) ve Enerji (`XLE`) sektörleri, makro şoklarda piyasa genelinden çok daha ağır yara alan varlıklardır.
- **Pozitif Ayrışan (Defansif) Sektörler:** Finans (`XLF`) ve Sağlık (`XLV`) sektörleri kriz anlarında defansif tampon görevi görerek SPY endeksine kıyasla pozitif ayrışma eğilimi göstermiştir.
- **Güvenli Liman Yanılgısı:** Altın (`GLD`), kriz anlarında mutlak anlamda değer kazansa da SPY'a göreceli (anormal) getiri bazında negatif ayrışmıştır. Sadece altına dayalı bir korunma (hedge) stratejisinin eksik kalabileceği görülmüştür.
- **Volatilite Şoku Getiriden Daha Güçlüdür:** Olay pencerelerinde yön tahmini zor olsa da fiyat dalgalanmalarındaki (10 günlük volatilite) sıçrama son derece kesindir. Piyasanın **"ikinci moment" (risk)** tepkisi, **"birinci moment" (getiri yönü)** tepkisinden daha belirgindir.
- **İstatistiksel Kısıtlılık:** 5 adet olay incelendiğinden örneklem büyüklüğü nedeniyle p-değerleri %5 anlamlılık düzeyinin üzerinde (p > 0.05) çıkmıştır. Ancak ekonomik büyüklükler ve yönlü trendler, portföy yönetimi açısından son derece tutarlıdır.

---

## 💼 Portföy Yönetimi İçin Çıkarımlar

Bu çalışmanın sonuçlarına dayanarak, makroekonomik belirsizlik dönemlerinde uygulanabilecek stratejiler:

1. **Enerji / Emtia Ağırlığını Azaltın:** Makro risk alarmları çaldığında enerji sektörünün ve petrolün portföy ağırlığı düşürülmeli veya opsiyonlar ile korunmalıdır.
2. **Sağlık ve Finans'ı Tampon Yapın:** Defansif karakteri veriyle desteklenen `XLV` ve `XLF`, kriz dönemlerinde portföy dengesini sağlamak için ağırlıklandırılabilecek en iyi adaylardır.
3. **Volatilite Hedeflemesi (VaR) Kullanın:** Kriz anlarında *"piyasa ne yöne gidecek?"* sorusundan ziyade *"piyasa ne kadar sallanacak?"* sorusuna odaklanılmalı; pozisyon büyüklükleri artan volatiliteye göre dinamik olarak küçültülmelidir.

---

## 🛠️ Depo İçeriği (Repository Structure)

```
📦 sp500-event-study
 ┣ 📜 sp500_olay_calismasi_eda_G.ipynb
 ┣ 📜 Sunum_SP500_Olay_Calismasi.pdf
 ┗ 📜 sp500_olay_calismasi_ham_veri.csv
```

- **`sp500_olay_calismasi_eda_G.ipynb`** — Python ile yazılmış; veri temizleme, AR/CAR hesaplamaları, görselleştirmeler (CAR grafikleri, Volatilite grafikleri, p-değerli bar chart'lar) ve istatistiksel testleri içeren ana Jupyter Notebook dosyası.
- **`Sunum_SP500_Olay_Calismasi.pdf`** — Araştırmanın metriklerini, teorik altyapısını ve sonuçlarını özetleyen Türkçe sunum dosyası.
- **`sp500_olay_calismasi_ham_veri.csv`** — Çalışmada kullanılan ham S&P 500 ve sektör ETF'lerine ait finansal veri seti.

---

## ⚙️ Gereksinimler ve Kurulum

Projeyi kendi bilgisayarınızda çalıştırmak için aşağıdaki Python kütüphanelerine ihtiyacınız bulunmaktadır:

```bash
pip install pandas numpy matplotlib seaborn scipy
```

İlgili dizine gidin ve Jupyter Notebook'u başlatın:

```bash
jupyter notebook
```

Ardından `sp500_olay_calismasi_eda_G.ipynb` dosyasını çalıştırarak analizleri inceleyebilirsiniz.
