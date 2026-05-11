# S&P 500 Sektör ETF'leri — Makroekonomik Olay Çalışması (Event Study)

## 📌 Proje Özeti

Bu proje, ABD borsasındaki makroekonomik olayların S&P 500 sektör ETF'lerine etkisini **olay çalışması (event study)** yöntemiyle analiz etmektedir. Proje kapsamında sektörlerin olaylara verdikleri tepkiler **Anormal Getiri (AR)** ve **Kümülatif Anormal Getiri (CAR)** metrikleri üzerinden ölçülmüş, istatistiksel anlamlılıkları **t-testleri** ile sınanmıştır.

---

## 🎯 Araştırma Sorusu

> Makroekonomik şoklar (pandemi, savaş, seçim vb.) karşısında farklı sektörler piyasadan anlamlı ölçüde ayrışıyor mu?

---

## 📊 Veri ve Kapsam

### Veri Kaynağı
- **Yahoo Finance** (`yfinance` kütüphanesi) üzerinden günlük kapanış fiyatları ve hacim verileri çekilmiştir.
- **Tarih aralığı:** 2010 – 2026

### Semboller (12 adet)

| Sembol | Tür | Açıklama |
|--------|-----|----------|
| `SPY`  | Gösterge Endeks | S&P 500 ETF (benchmark) |
| `^VIX` | Volatilite | CBOE Volatilite Endeksi |
| `XLK`  | Odak Sektör | Teknoloji |
| `XLV`  | Odak Sektör | Sağlık |
| `ITA`  | Odak Sektör | Havacılık ve Savunma |
| `XLE`  | Odak Sektör | Enerji |
| `XLF`  | Odak Sektör | Finans |
| `XLP`  | Kontrol Grubu | Temel Tüketim |
| `XLY`  | Kontrol Grubu | İsteğe Bağlı Tüketim |
| `XLRE` | Kontrol Grubu | Gayrimenkul |
| `GLD`  | Emtia / Makro | Altın |
| `CL=F` | Emtia / Makro | Ham Petrol Vadeli |

### İncelenen Makroekonomik Olaylar (5 adet)

| Olay | T0 Tarihi |
|------|-----------|
| ABD Seçimleri 2016 | 2016-11-08 |
| COVID-19 Pandemisi 2020 | 2020-02-24 |
| Rusya-Ukrayna Savaşı 2022 | 2022-02-24 |
| İsrail-İran Gerginliği 2024 | 2024-04-13 |
| ABD Seçimleri 2024 | 2024-11-05 |

---

## 🧮 Metodoloji

### 1. Veri Toplama (Data Pipeline)

`Veri_Hatti.ipynb` notebook'u, OOP yapısında tasarlanmış `VeriMuhendisi` sınıfı ile Yahoo Finance'ten otomatik veri çekimi yapar. Rate-limit sorunlarını aşmak için `time.sleep()` gecikmeleri uygulanmaktadır.

### 2. Anormal Getiri (AR) Hesaplama

Piyasanın genel hareketinden arındırılmış getiri:

```
AR = Sektör Log Getirisi − SPY Log Getirisi
```

Bu sayede bir sektörün belirli bir günde piyasanın üzerinde mi altında mı performans gösterdiği ölçülür.

### 3. Kümülatif Anormal Getiri (CAR)

Belirli bir olay penceresi boyunca günlük AR değerlerinin toplamı. Projede kullanılan pencereler:
- **CAR [-3, +3]** — Olay etrafında 7 günlük dar pencere
- **CAR [-10, +10]** — 21 günlük geniş pencere
- **CAR [-20, +20]** — Görselleştirme amaçlı tam pencere

### 4. İstatistiksel Testler (Hipotez Sınaması)

Her sektör ETF'i için **tek örneklem t-testi** (H₀: μ_CAR = 0) uygulanmıştır:
- **T0 Testi:** Olay günü anormal getirinin sıfırdan farklı olup olmadığı
- **CAR [-3, +3] Testi:** Kısa vadeli kümülatif etkinin anlamlılığı
- **Pencere [-10, +10] Testi:** Geniş pencerede kümülatif etki

### 5. Feature Engineering

`hazirlik.ipynb` notebook'unda ham veriden türetilen özellikler:

| Özellik | Açıklama |
|---------|----------|
| Log Getiri | Günlük logaritmik getiri |
| Volatilite (10g / 30g) | Kayan pencere standart sapması |
| RSI (14 gün) | Relative Strength Index |
| SMA Uzaklık (20 gün) | Kapanışın hareketli ortalamaya uzaklığı (%) |
| Log Hacim | İşlem hacminin logaritması |
| Hacim Değişimi | Günlük hacim değişim oranı |

---

## 📁 Proje Yapısı

```
DS_projesi/
│
├── .gitignore                          # Git hariç tutma kuralları
├── README.md                           # Proje dokümantasyonu
│
├── hazirlik.ipynb                      # Feature engineering & veri hazırlığı
├── sp500_olay_calismasi_eda_GELISMIS.ipynb  # Ana EDA & hipotez testleri
│
├── grafikler/                          # Görselleştirme çıktıları
│   ├── 01_CAR_olay_penceresi.png       # CAR grafiği [-20, +20]
│   ├── 02_Volatilite_soku.png          # Volatilite şoku grafiği
│   ├── 03_Korelasyon_isi_haritasi.png  # Korelasyon heatmap
│   ├── 04_Sektor_AR_boxplot.png        # Sektör bazlı AR dağılımı
│   └── 05_Sektor_ortalama_CAR.png      # Ortalama CAR karşılaştırması
│
└── sonuclar,verihatti,rehber/
    ├── Veri_Hatti.ipynb                # Veri boru hattı (VeriMuhendisi sınıfı)
    ├── olay_bazli_CAR_m3p3.csv         # Olay × Sektör CAR [-3,+3] sonuçları
    ├── sektor_ttest_CAR_m3p3.csv       # CAR [-3,+3] t-test sonuçları
    ├── sektor_ttest_T0.csv             # T0 (olay günü) t-test sonuçları
    └── sektor_ttest_pencere_m10p10.csv # Geniş pencere t-test sonuçları
```

---

## 🔬 Temel Bulgular

### T-Test Sonuçları (CAR [-3, +3])

| Sektör | Grup | Ort. CAR | t-istatistik | p-değeri | Anlamlı (p<0.05) |
|--------|------|----------|--------------|----------|-------------------|
| CL=F | Emtia/Makro | -4.29% | -1.378 | 0.240 | Hayır |
| XLF | Odak Sektör | +1.56% | +1.104 | 0.331 | Hayır |
| XLV | Odak Sektör | +1.16% | +1.091 | 0.337 | Hayır |
| GLD | Emtia/Makro | -2.60% | -1.065 | 0.347 | Hayır |
| XLE | Odak Sektör | -3.49% | -0.964 | 0.390 | Hayır |
| XLRE | Kontrol | -1.06% | -0.872 | 0.432 | Hayır |
| XLP | Kontrol | -1.18% | -0.719 | 0.512 | Hayır |
| XLY | Kontrol | -0.61% | -0.651 | 0.551 | Hayır |
| XLK | Odak Sektör | +0.22% | +0.171 | 0.872 | Hayır |
| ITA | Odak Sektör | +0.45% | +0.145 | 0.892 | Hayır |

### Yorumlar

- **İstatistiksel anlamlılık:** Hiçbir sektörde p < 0.05 eşiği aşılamamıştır. Ancak bu "etki yok" anlamına gelmemektedir — **5 olay** üzerinden yapılan t-testlerinin istatistiksel gücü yetersizdir.

- **Ekonomik eğilimler tutarlıdır:**
  - **Negatif ayrışan:** CL=F (Petrol, -4.3%), XLE (Enerji, -3.5%), GLD (Altın, -2.6%)
  - **Pozitif ayrışan:** XLF (Finans, +1.6%), XLV (Sağlık, +1.2%)
  - **Nötr:** XLK (Teknoloji), ITA (Savunma), XLY (İst. Tüketim)

- **Volatilite etkisi:** Olay günü civarında 10 günlük volatilitede net bir sıçrama gözlenmiştir. Risk (2. moment) etkisi, getiri (1. moment) etkisinden daha belirgindir.

---

## 📈 Görselleştirmeler

| Grafik | Açıklama |
|--------|----------|
| `01_CAR_olay_penceresi.png` | XLK, XLE, XLV sektörlerinin CAR eğrileri [-20, +20] |
| `02_Volatilite_soku.png` | Olay penceresi etrafında volatilite değişimi [-10, +10] |
| `03_Korelasyon_isi_haritasi.png` | Özellikler arası korelasyon matrisi |
| `04_Sektor_AR_boxplot.png` | Sektör bazlı Anormal Getiri dağılımları |
| `05_Sektor_ortalama_CAR.png` | Sektörlerin ortalama CAR karşılaştırması |

---

## ⚙️ Kurulum ve Çalıştırma

### Gereksinimler

```
Python >= 3.9
pandas
numpy
scipy
matplotlib
seaborn
yfinance
```

### Kurulum

```bash
git clone https://github.com/beratoz/Veri-Bilimi-Projesi.git
cd Veri-Bilimi-Projesi
pip install pandas numpy scipy matplotlib seaborn yfinance
```

### Çalıştırma Sırası

1. **Veri Çekimi:** `sonuclar,verihatti,rehber/Veri_Hatti.ipynb` — Yahoo Finance'ten ham veri çeker ve ana veri setini üretir.
2. **Analiz:** `sp500_olay_calismasi_eda_GELISMIS.ipynb` — EDA ve hipotez testlerini çalıştırır, grafikleri üretir.
3. **Feature Engineering (Opsiyonel):** `hazirlik.ipynb` — İleri düzey modelleme için genişletilmiş veri seti üretir.

> ⚠️ **Not:** `yfinance` API'si rate-limit uygulayabilir. Veri çekimi sırasında `time.sleep()` gecikmeleri kullanılmaktadır.

---

## 🛠️ Kullanılan Teknolojiler

- **Python 3.9+** — Ana programlama dili
- **pandas & NumPy** — Veri manipülasyonu ve hesaplamalar
- **yfinance** — Finansal veri çekimi (Yahoo Finance API)
- **scipy** — İstatistiksel testler (tek örneklem t-test)
- **matplotlib & seaborn** — Veri görselleştirme
- **Jupyter Notebook** — İnteraktif analiz ortamı

---

## 🔮 Gelecek Çalışmalar

- [ ] Olay örneklem sayısının artırılması (5 → 20-30 olay) — istatistiksel gücü yükseltmek için
- [ ] Pipeline'ın `.py` scriptlerine dönüştürülmesi
- [ ] Sektörler arası nedensellik analizi (Granger causality)
- [ ] Alternatif benchmark modelleri (CAPM, Fama-French)

---

## 📄 Lisans

Bu proje akademik amaçlı geliştirilmiştir.
