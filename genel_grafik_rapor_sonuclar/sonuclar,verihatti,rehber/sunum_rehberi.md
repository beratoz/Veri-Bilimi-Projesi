# S&P 500 Sektör ETF'leri — Makroekonomik Olay Çalışması
## Sınıf Sunumu Konuşma Rehberi

---

## BÖLÜM 1: TEMELİ ATMAK

### Açılış — Dinleyicinin dikkatini çek

> "Şubat 2020'yi düşünün. COVID haberleri gelmeye başlıyor, borsa düşüyor. Ama aynı gün sağlık sektörü hisseleri endeksin üzerinde performans gösterirken, enerji hisseleri çok daha sert düşüyor. Neden? İşte bugün tam olarak bu soruyu cevaplamaya çalışacağız."

### Olay Çalışması (Event Study) nedir?

Bir haber ya da şok geldiğinde piyasanın o habere nasıl tepki verdiğini ölçen bir yöntem. Bir nevi "piyasanın röntgenini çekmek" — ama sadece belirli bir olay anında.

### Neden fiyatlara değil de Anormal Getiri'ye bakıyoruz?

Fiyat tek başına yanıltıcıdır çünkü piyasa zaten her gün hareket eder. Anormal Getiri (AR), bir hissenin o gün "normalde" kazanacağının üzerinde veya altında ne kadar getiri sağladığını gösterir. Basitçe:

**AR = Sektörün günlük getirisi − S&P 500'ün (SPY) aynı günkü getirisi**

Eğer SPY %2 düştüğü gün enerji sektörü %5 düştüyse, enerji sektörünün anormal getirisi −3%'tür. Yani enerji, piyasanın genel düşüşünün ötesinde ekstra %3 kaybetmiş demektir.

### CAR ne anlama geliyor?

CAR (Kümülatif Anormal Getiri), bu günlük anormal getirilerin birkaç gün boyunca toplanmış hali. Neden? Çünkü bir olay tek günde sindirilmeyebilir; piyasa birkaç gün boyunca tepki vermeye devam edebilir. CAR, o birkaç günlük toplam ayrışmayı yakalıyor.

---

## BÖLÜM 2: ARKA PLANDAKİ MOTOR (Kod ve Veri)

### Veri Seti

Bu çalışmada 5 büyük makroekonomik olay inceleniyor:

1. **2016 ABD Seçimleri** — Trump'ın sürpriz zaferi
2. **COVID-19 Pandemisi (2020)** — Küresel kapanmalar
3. **Rusya-Ukrayna Savaşı (2022)** — Enerji ve jeopolitik şok
4. **2024 ABD Seçimleri** — Politik belirsizlik
5. **İsrail-İran Gerilimi (2024)** — Bölgesel çatışma riski

Her olay için 10 farklı sektör/varlık ETF'inin günlük verilerini topladım. Bunları 3 gruba ayırdım:

- **Odak Sektörler:** XLK (Teknoloji), XLF (Finans), XLE (Enerji), XLV (Sağlık), ITA (Savunma)
- **Kontrol Grubu:** XLP (Tüketim Temel), XLY (Tüketim İsteğe Bağlı), XLRE (Gayrimenkul)
- **Emtia/Makro:** GLD (Altın), CL=F (Ham Petrol)

### Veri Hazırlama Süreci

Notebook'ta ilk adım veriyi temizlemek: sonsuz değerler (inf), eksik günler gibi sorunlar giderildi. Ardından SPY endeksinin günlük getirisi her satıra eşleştirildi ve her sektör için **AR = Sektör Getirisi − SPY Getirisi** hesaplandı.

### Olay Pencereleri — Neden farklı boyutlarda?

Olay penceresi, "olaydan kaç gün önce ve sonrasına bakıyoruz" demek. Üç farklı pencere kullandım:

- **T=0 (Olay Günü):** Sadece haberin geldiği gün — anlık şok tepkisi
- **[-3, +3] gün:** Olaydan 3 gün önce ile 3 gün sonrası — kısa vadeli sindirim süreci
- **[-10, +10] gün:** Daha geniş bir pencere — haberin sızması (öncesi) ve tam sindirilmesi (sonrası)

Neden birden fazla pencere? Çünkü bazı olaylar anında fiyatlanır (seçim sonuçları gibi), bazıları ise günlerce etki yaratır (COVID gibi). Farklı pencereler, farklı dinamikleri yakalıyor.

---

## BÖLÜM 3: TABLOLAR NE SÖYLÜYOR?

### Ana Tablo: Sektör Bazlı CAR[-3, +3] Sonuçları

> "Şimdi en heyecanlı kısma geliyoruz: veriler bize ne diyor?"

| Sektör | CAR[-3,+3] | Yorum |
|---|---:|---|
| **CL=F (Ham Petrol)** | **−4.29%** | En sert negatif ayrışma |
| **XLE (Enerji)** | **−3.49%** | Enerji net zarar gören |
| GLD (Altın) | −2.60% | "Güvenli liman" beklentisinin altında |
| XLP (Tüketim Temel) | −1.18% | Hafif negatif |
| XLRE (Gayrimenkul) | −1.06% | Hafif negatif |
| XLY (Tüketim İst.) | −0.61% | Nötr |
| XLK (Teknoloji) | +0.22% | Nötr — piyasayla eş hareket |
| ITA (Savunma) | +0.45% | Nötr |
| **XLV (Sağlık)** | **+1.16%** | Defansif karakter |
| **XLF (Finans)** | **+1.56%** | En güçlü pozitif ayrışma |

### Hikaye 1: Negatif Ayrışanlar — Enerji ve Petrol

Ham petrol (CL=F) ve enerji sektörü (XLE) makro şoklarda en çok yara alan varlıklar. COVID'de CL=F tek başına −16.4% CAR kaydetmiş. Bu mantıklı: küresel belirsizlik arttığında talep beklentileri düşüyor ve enerji fiyatları sert geriliyor.

### Hikaye 2: Pozitif Ayrışanlar — Sağlık ve Finans

Sağlık sektörü (XLV) beş olayın dördünde pozitif CAR üretmiş — klasik "defansif sektör" tanımını doğruluyor. İnsanlar kriz dönemlerinde de ilaç almaya ve hastaneye gitmeye devam eder.

Finans (XLF) ise ilginç bir tablo çiziyor: 2016 seçimlerinde +6.9% CAR ile en yüksek pozitif ayrışmayı göstermiş. Faiz ve düzenleme beklentileri finans sektörünü olumlu etkilemiş olabilir.

### Hikaye 3: Altın Sürprizi

Altın (GLD) genellikle "güvenli liman" olarak bilinir, ama bu çalışmada SPY'a göre negatif ayrışma göstermiş (CAR = −2.6%). Yani mutlak getirisi pozitif olsa bile, endeksi yenemediği dönemler var. Bu, altının her krizde otomatik olarak hedge işlevi gördüğü varsayımını sorgulatıyor.

### İstatistiksel Anlamlılık Meselesi

> "Şimdi sınıfta birileri soracaktır: p-değerleri ne diyor? Dürüst cevap: hiçbir sektör klasik %5 eşiğinin altına inemedi."

**Ama bu 'etki yok' demek değil.** Neden?

Elimizde sektör başına sadece **5 olay** var. 5 gözlemle t-testi yapmak, bir odayı 5 kişiyle anket yapıp "Türkiye'nin görüşü budur" demek gibi bir şey. Ekonomik olarak −4.3% gibi kayda değer bir CAR değeri bile bu kadar küçük örneklemle istatistiksel anlamlılığa ulaşamıyor. En düşük p-değerimiz 0.21 (XLE, T=0) — yani eğilim var ama "kanıtlamak" için daha fazla olaya ihtiyacımız var.

**Önemli ders:** İstatistiksel anlamlılık yokluğu ≠ Ekonomik anlam yokluğu. Eğilimler tutarlı ve yorumlanabilir; sadece örneklem küçük.

### Olay Bazlı Öne Çıkanlar

Birkaç dikkat çekici olay-sektör kombinasyonu:

- **COVID + Enerji (XLE):** CAR = −17.8% — En sert darbe
- **COVID + Petrol (CL=F):** CAR = −16.4% — Talep şoku
- **2016 Seçim + Finans (XLF):** CAR = +6.9% — Trump rally etkisi
- **2016 Seçim + Savunma (ITA):** CAR = +6.2% — Savunma harcaması beklentisi
- **COVID + Sağlık (XLV):** CAR = +3.0% — Defansif karakter doğrulanıyor

---

## BÖLÜM 4: KAPANIŞ

### Bu analiz ne işe yarar?

Bu çalışma üç pratik mesaj veriyor:

**1. Risk yönetimi için yol haritası.** Makro şok dönemlerinde enerji ve emtia ağırlığını azaltmak, sağlık ve finans sektörüne yönelmek mantıklı bir strateji olabilir. Portföy yöneticileri bu tür analizleri "şok geldiğinde ne yapmalıyım" sorusuna cevap aramak için kullanır.

**2. "Güvenli liman" klişelerini sorgulamak.** Altının her krizde koruma sağladığı varsayımı bu verilerde tam doğrulanmıyor. Hedge stratejisi kurarken tek bir varlığa değil, çeşitlendirilmiş araçlara (dolar, kısa vadeli tahvil, opsiyonlar) bakmak gerekiyor.

**3. Volatilite sinyali getiriden daha güçlü.** Grafiklerimizde olay günü civarında volatilitedeki sıçrama çok net görünüyor. Yani "hangi yöne gidecek" sorusundan önce "ne kadar sallanacak" sorusu daha güvenilir bir sinyal veriyor. Bu da VaR ve volatilite hedeflemesi gibi risk yönetim araçlarının değerini gösteriyor.

### Son söz

> "Bu çalışma 5 olaylık küçük bir örneklem üzerine kurulu — istatistiksel olarak kesin kanıtlar sunmuyor. Ama ekonomik olarak tutarlı bir hikaye anlatıyor: makro şoklarda enerji ve emtia negatif, sağlık ve finans pozitif ayrışıyor. Örneklemi 30-40 olaya çıkarırsak, muhtemelen bu eğilimleri istatistiksel olarak da kanıtlayabiliriz. Bu çalışma, o büyük analizin pilot denemesidir."

---

*Not: Bu rehber, sınıf sunumunda doğal bir anlatım akışı sağlamak için hazırlanmıştır. Her bölüm arasında grafikleri (CAR olay penceresi, volatilite şoku, boxplot, sektör ortalama CAR) göstermeyi unutmayın.*
