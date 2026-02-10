# Tedarikçi Uyumluluk Sistemi - Dokümantasyon

## Genel Bakış

"Son Tedarik" uygulamasına eklenen **Tedarikçi Uyumluluk Sistemi** iki ayrı modülden oluşur:

1. **🏛️ Sertifika & Sözleşme Yönetimi**: Belge görüntüleme ve Fire Satış kontrolü
2. **📊 Uyumluluk Analizi**: Sertifika takibi, risk skorlaması ve AI destekli analiz

---

## 1. Sertifika & Sözleşme Yönetimi

### Özellikler

#### Belge Görüntüleyici
- **Sertifikalar**: PDF sertifikaları görüntüleme ve yönetme
- **Sözleşmeler**: PDF sözleşmeleri görüntüleme ve yönetme
- Dosya boyutu ve tarih bilgileri
- Dosya açma ve yol kopyalama işlevleri

#### Fire Satış Çevre Uyumluluk Kontrolü
- **Kontrol Edilen Belgeler**:
  - Çevre İzin ve Lisans Belgesi
  - Atık Kodları Lisansı (200111)
  - UATF (Ulusal Atık Taşıma Formu)
  - Tartım Fişi / Kantar Belgesi
  - Geri Kazanım Belgesi (R12)

#### AI Destekli İşlevler
- 🤖 AI ile sözleşme oluşturma
- 📧 Tedarikçiye email gönderme
- Şablon sözleşme kullanma desteği

#### Veri Kaynakları
- **Sertifikalar**: `X:\01.Public\TEDARİKÇİ PERFORMANS\Sertifikalar`
- **Sözleşmeler**: `X:\01.Public\TEDARİKÇİ PERFORMANS\Sözleşme`

---

## 2. Uyumluluk Analizi

### Özellikler

#### A. Sertifika Takibi (Trafik Işığı Sistemi)
- **🟢 Yeşil**: Sertifika süresi 90+ gün
- **🟡 Sarı**: Sertifika süresi 30-90 gün arası
- **🟠 Turuncu**: Sertifika süresi 0-30 gün arası
- **🔴 Kırmızı**: Sertifika süresi dolmuş
- **⚫ Siyah**: Sertifika yok

Takip edilen sertifikalar:
- ISO 9001
- OEKO-TEX
- BSCI

#### B. Detaylı Tedarikçi Bilgileri
Yeni Excel formatı ile entegre tablo:
- **Tedarikçi Adı**: Firma adı
- **Yetkili Kişi**: Yetkili kişi adı soyadı
- **Telefon**: İletişim telefonu
- **Adres**: Firma adresi
- **Tür**: Tedarikçi türü (Tekstil, Kimya, vb.)
- **Alım Türü**: İthalat veya Yerli
- **Mail**: Email adresi
- **Başlangıç Tarihi**: İş birliği başlangıç tarihi
- **Bitiş Tarihi**: Sözleşme bitiş tarihi
- **Kayıt Tarihi**: Sisteme kayıt tarihi
- **Eksik Evraklar**: Eksik belge listesi

#### C. Uyumluluk Skorkarti (0-100)

Toplam 100 puan üzerinden hesaplanır:

**Sertifika Puanı (40 puan)**
- Her sertifika için: 40/3 = 13.33 puan
- ISO 9001, OEKO-TEX, BSCI

**Termin Uyumu (30 puan)**
- Teslimat zamanında yapılma oranı
- %100 termin uyumu = 30 puan

**Kalite Uyumu (30 puan)**
- Reklamasyon oranına göre hesaplanır:
  - ≤%5 reklamasyon: 30 puan
  - ≤%10 reklamasyon: 20 puan
  - ≤%15 reklamasyon: 10 puan
  - >%15 reklamasyon: 0 puan

#### D. Risk Seviyeleri

Uyumluluk skoruna göre risk kategorileri:
- **🟢 Düşük Risk**: Skor ≥ 80
- **🟠 Orta Risk**: Skor 60-79
- **🔴 Yüksek Risk**: Skor < 60

#### E. Risk Matrisi (Kabarcık Grafiği)

Görsel risk analizi:
- **X Ekseni**: Termin Uyum %
- **Y Ekseni**: Reklamasyon Oranı %
- **Kabarcık Boyutu**: Toplam sipariş hacmi
- **Renk Gradyanı**: Uyumluluk skoru (kırmızı-sarı-yeşil)

#### F. Denetim Takvimi

90 gün içindeki yaklaşan denetimler:
- **🔴**: ≤30 gün
- **🟠**: 31-60 gün
- **🟢**: 61-90 gün

#### G. AI Destekli Analiz

Gemini API kullanarak:
- Tedarikçi performans değerlendirmesi
- Risk analizi
- İş birliği önerileri
- Detaylı rapor oluşturma

---

## Excel Veri Yapısı

### 1. Ana Veri Dosyası (Cari Bilgiler)

**Dosya Yolu:** `X:\01.Public\TEDARİKÇİ PERFORMANS\Cari Bilgiler\Cari Bilgiler.xlsx`

#### Gerekli Kolonlar

##### Temel Bilgiler
```
Tedarikçi Adı          | string  | Zorunlu
Yetkili Kişi           | string  | Opsiyonel
Telefon                | string  | Opsiyonel
Adres                  | string  | Opsiyonel
Alım Türü              | string  | "İthalat" veya "Yerli"
Tür                    | string  | Tedarikçi kategorisi
Mail                   | string  | Email adresi
Başlangıç Tarihi       | date    | YYYY-MM-DD formatında
Bitiş Tarihi           | date    | YYYY-MM-DD formatında
Kayıt Tarihi           | date    | YYYY-MM-DD formatında
Eksik Evraklar         | string  | Eksik belge listesi (virgülle ayrılmış)
```

##### Sertifika Bilgileri
```
ISO 9001               | string  | "Var" veya "Yok"
ISO 9001 Bitiş         | date    | YYYY-MM-DD formatında
OEKO-TEX               | string  | "Var" veya "Yok"
OEKO-TEX Bitiş         | date    | YYYY-MM-DD formatında
BSCI                   | string  | "Var" veya "Yok"
BSCI Bitiş             | date    | YYYY-MM-DD formatında
```

##### Performans Metrikleri
```
Termin Uyum %          | number  | 0-100 arası
Reklamasyon Sayısı     | number  | Pozitif tam sayı
Toplam Sipariş         | number  | Pozitif tam sayı
Son Denetim            | date    | YYYY-MM-DD formatında
Sonraki Denetim        | date    | YYYY-MM-DD formatında
```

#### Örnek Veri

```csv
Tedarikçi Adı,Yetkili Kişi,Telefon,Adres,Alım Türü,Tür,Mail,Başlangıç Tarihi,Bitiş Tarihi,Kayıt Tarihi,Eksik Evraklar,ISO 9001,ISO 9001 Bitiş,OEKO-TEX,OEKO-TEX Bitiş,BSCI,BSCI Bitiş,Termin Uyum %,Reklamasyon Sayısı,Toplam Sipariş,Son Denetim,Sonraki Denetim
ABC Tekstil,Ahmet Yılmaz,0532 111 2233,İstanbul,İthalat,Tekstil,info@abctekstil.com,2024-01-15,2026-01-15,2024-01-10,,Var,2026-06-15,Var,2026-09-20,Var,2026-04-10,95,2,150,2025-09-10,2026-03-10
```

---

### 2. Sertifika Durumu ve Trafik Işığı Dosyası

**Dosya Yolu:** `X:\01.Public\TEDARİKÇİ PERFORMANS\Cari Bilgiler\Sertifika Durumu Trafik ışığı.xlsx`

**⚠️ ÖNEMLİ:** Bu dosya ayrı bir Excel dosyasıdır ve yalnızca sertifika bilgilerini içerir.

#### Gerekli Kolonlar (11 Kolon)

```
1.  Tedarikçi Adı      | string  | Zorunlu - Ana dosya ile eşleşmeli
2.  ISO 9001           | string  | "Var" veya "Yok"
3.  ISO 9001 Bitiş     | date    | YYYY-MM-DD formatında
4.  OEKO-TEX           | string  | "Var" veya "Yok"
5.  OEKO-TEX Bitiş     | date    | YYYY-MM-DD formatında
6.  BSCI               | string  | "Var" veya "Yok"
7.  BSCI Bitiş         | date    | YYYY-MM-DD formatında
8.  ISO 14001          | string  | "Var" veya "Yok"
9.  ISO 14001 Bitiş    | date    | YYYY-MM-DD formatında
10. SEDEX              | string  | "Var" veya "Yok"
11. SEDEX Bitiş        | date    | YYYY-MM-DD formatında
```

#### UI'da Görünen 8 Sütun

Uygulamada sertifika tablosunda şu 8 sütun görüntülenir:
1. **Tedarikçi** - Tedarikçi adı
2. **ISO 9001** - Sertifika durumu + trafik ışığı
3. **OEKO-TEX** - Sertifika durumu + trafik ışığı
4. **BSCI** - Sertifika durumu + trafik ışığı
5. **ISO 14001** - Sertifika durumu + trafik ışığı
6. **SEDEX** - Sertifika durumu + trafik ışığı
7. **Uyumluluk Skoru** - Ana dosyadan hesaplanır
8. **Risk** - Skordan türetilir

**Not:** "Uyumluluk Skoru" ve "Risk" Excel'de bulunmaz, otomatik hesaplanır.

#### Örnek Veri

```csv
Tedarikçi Adı,ISO 9001,ISO 9001 Bitiş,OEKO-TEX,OEKO-TEX Bitiş,BSCI,BSCI Bitiş,ISO 14001,ISO 14001 Bitiş,SEDEX,SEDEX Bitiş
ABC Tekstil,Var,2026-06-15,Var,2026-09-20,Var,2026-04-10,Var,2026-03-20,Var,2026-12-30
XYZ Kimya,Var,2025-12-30,Yok,,Var,2026-07-25,Yok,,Var,2026-01-15
```

**📄 Detaylı Format Açıklaması:** Bkz. `Sertifika_Excel_Format.md`

**📋 Şablon Dosya:** Bkz. `Sertifika_Template.csv`

---

## Teknik Detaylar

### Global Değişkenler

```python
df_uyumluluk_global = None  # Uyumluluk verileri DataFrame (Ana veri)
df_sertifika_trafik_global = None  # Sertifika durumu ve trafik ışığı verisi
cari_bilgiler_path = r"X:\01.Public\TEDARİKÇİ PERFORMANS\Cari Bilgiler\Cari Bilgiler.xlsx"
sertifika_trafik_path = r"X:\01.Public\TEDARİKÇİ PERFORMANS\Cari Bilgiler\Sertifika Durumu Trafik ışığı.xlsx"
```

### Sabitler

```python
COMPLIANCE_DATE_FORMAT = '%Y-%m-%d'
CERT_SCORE_PER_CERTIFICATE = 40 / 3
TOTAL_CERT_POINTS = 40
TOTAL_DELIVERY_POINTS = 30
TOTAL_QUALITY_POINTS = 30
```

### Ana Fonksiyonlar

#### 1. Belge Yönetimi
```python
def init_compliance_docs_tab():
    """Sertifika & Sözleşme Görüntüleyici"""
    # Fire satış kontrolü
    # Belge görüntüleme
    # AI sözleşme oluşturma
    # Email gönderme
```

#### 2. Uyumluluk Analizi
```python
def init_compliance_analysis_tab():
    """Uyumluluk Analizi ve Skorlama"""
    # Veri yükleme
    # Detaylı bilgi tablosu
    # Sertifika takibi
    # Risk analizi
    # Denetim takvimi
    # AI analizi
```

### Yardımcı Fonksiyonlar

```python
def load_cari_bilgiler():
    """Excel'den veri yükle veya örnek veri kullan"""
    
def check_certificate_expiry(row, cert_name, cert_date_col):
    """Sertifika süresini kontrol et"""
    
def calculate_compliance_score(row):
    """Uyumluluk skorunu hesapla (0-100)"""
    
def get_risk_level(score):
    """Risk seviyesini belirle"""
    
def calculate_complaint_rate(reklamasyon_count, total_orders):
    """Reklamasyon oranını hesapla"""
    
def ai_compliance_analysis(tedarikci_adi, row):
    """AI destekli uyumluluk analizi"""
    
def check_waste_disposal_compliance(supplier_name):
    """Fire satış çevre uyumluluk kontrolü"""
```

---

## Kullanım

### 1. Sertifika & Sözleşme Yönetimi

1. Sol menüden "🏛️ Sertifika & Sözleşme" seçin
2. Fire Satış Kontrolü için:
   - Tedarikçi adını girin
   - "🔍 Uyumluluk Kontrolü Yap" tıklayın
   - Sonuçları inceleyin
3. AI Sözleşme Oluşturma:
   - İsteğe bağlı şablon seçin
   - "🤖 AI ile Sözleşme Oluştur" tıklayın
4. Email Gönderme:
   - Email adresini girin
   - "📧 Tedarikçiye Email Gönder" tıklayın

### 2. Uyumluluk Analizi

1. Sol menüden "📊 Uyumluluk Analizi" seçin
2. Otomatik olarak Excel verileri yüklenir
3. Detaylı bilgileri ve sertifika durumlarını inceleyin
4. Risk matrisini ve grafikleri görüntüleyin
5. AI Analizi için:
   - Tedarikçi seçin
   - Gemini API Key girin
   - "🤖 AI Analizi Yap" tıklayın

---

## Güvenlik

### API Anahtarı
- Otomatik olarak doldurulmaz
- Her oturumda yeniden girilmelidir
- Maskelenerek gösterilir (`show="*"`)

### Veri Güvenliği
- Tüm işlemler activity logger ile kaydedilir
- Hata durumlarında detaylı log tutulur
- Hassas veriler ekrana yazılmaz

---

## Sorun Giderme

### "Veri yüklenemedi" Hatası
- Excel dosya yolunu kontrol edin: `X:\01.Public\TEDARİKÇİ PERFORMANS\Cari Bilgiler\Cari Bilgiler.xlsx`
- Dosya izinlerini kontrol edin
- Örnek veri kullanılıyor olabilir (normal durum)

### "Klasör bulunamadı" Hatası
- Sertifika klasörü: `X:\01.Public\TEDARİKÇİ PERFORMANS\Sertifikalar`
- Sözleşme klasörü: `X:\01.Public\TEDARİKÇİ PERFORMANS\Sözleşme`
- Klasör yollarının doğru olduğunu kontrol edin

### Tarih Hataları
- Tarihlerin YYYY-MM-DD formatında olduğundan emin olun
- Excel hücrelerinin "Text" veya "Date" formatında olduğunu kontrol edin

---

## Versiyon Geçmişi

### v2.0.0 (2026-02-10)
- **Yeni**: Sistem 2 ayrı sayfaya bölündü
- Sertifika & Sözleşme Yönetimi (eski yapı korundu)
- Uyumluluk Analizi (yeni Excel kolonları eklendi)
- Detaylı tedarikçi bilgileri tablosu
- 11 yeni kolon desteği

### v1.0.0 (2026-02-10)
- İlk versiyon
- Tek sayfa yapısı
- Temel sertifika takibi

---

## İletişim ve Destek

Sorularınız için:
- Email: ozan.sabudak@defacto.com
- Sistem: Son Tedarik v2.8
- **🟢 Yeşil**: Sertifika süresi 90+ gün
- **🟡 Sarı**: Sertifika süresi 30-90 gün arası
- **🟠 Turuncu**: Sertifika süresi 0-30 gün arası
- **🔴 Kırmızı**: Sertifika süresi dolmuş
- **⚫ Siyah**: Sertifika yok

Takip edilen sertifikalar:
- ISO 9001
- OEKO-TEX
- BSCI

### 2. Uyumluluk Skorkarti (0-100)

Toplam 100 puan üzerinden hesaplanır:

#### Sertifika Puanı (40 puan)
- Her sertifika için: 40/3 = 13.33 puan
- ISO 9001, OEKO-TEX, BSCI

#### Termin Uyumu (30 puan)
- Teslimat zamanında yapılma oranı
- %100 termin uyumu = 30 puan

#### Kalite Uyumu (30 puan)
- Reklamasyon oranına göre hesaplanır:
  - ≤%5 reklamasyon: 30 puan
  - ≤%10 reklamasyon: 20 puan
  - ≤%15 reklamasyon: 10 puan
  - >%15 reklamasyon: 0 puan

### 3. Risk Seviyeleri

Uyumluluk skoruna göre risk kategorileri:
- **🟢 Düşük Risk**: Skor ≥ 80
- **🟠 Orta Risk**: Skor 60-79
- **🔴 Yüksek Risk**: Skor < 60

### 4. Risk Matrisi (Kabarcık Grafiği)

Görsel risk analizi:
- **X Ekseni**: Termin Uyum %
- **Y Ekseni**: Reklamasyon Oranı %
- **Kabarcık Boyutu**: Toplam sipariş hacmi
- **Renk Gradyanı**: Uyumluluk skoru (kırmızı-sarı-yeşil)

### 5. Denetim Takvimi

90 gün içindeki yaklaşan denetimler:
- **🔴**: ≤30 gün
- **🟠**: 31-60 gün
- **🟢**: 61-90 gün

### 6. AI Destekli Analiz

Gemini API kullanarak:
- Tedarikçi performans değerlendirmesi
- Risk analizi
- İş birliği önerileri
- Detaylı rapor oluşturma

## Veri Yapısı

### Excel Dosyası Formatı

**Dosya Yolu**: `X:\01.Public\TEDARİKÇİ PERFORMANS\Cari Bilgiler\Cari Bilgiler.xlsx`

**Gerekli Kolonlar**:
```
Tedarikçi Adı          | string
ISO 9001               | "Var" veya "Yok"
ISO 9001 Bitiş         | YYYY-MM-DD formatında tarih
OEKO-TEX               | "Var" veya "Yok"
OEKO-TEX Bitiş         | YYYY-MM-DD formatında tarih
BSCI                   | "Var" veya "Yok"
BSCI Bitiş             | YYYY-MM-DD formatında tarih
Termin Uyum %          | 0-100 arası sayı
Reklamasyon Sayısı     | Pozitif tam sayı
Toplam Sipariş         | Pozitif tam sayı
Son Denetim            | YYYY-MM-DD formatında tarih
Sonraki Denetim        | YYYY-MM-DD formatında tarih
```

### Örnek Veri

```python
{
    'Tedarikçi Adı': 'ABC Tekstil',
    'ISO 9001': 'Var',
    'ISO 9001 Bitiş': '2026-06-15',
    'OEKO-TEX': 'Var',
    'OEKO-TEX Bitiş': '2026-09-20',
    'BSCI': 'Var',
    'BSCI Bitiş': '2026-04-10',
    'Termin Uyum %': 95,
    'Reklamasyon Sayısı': 2,
    'Toplam Sipariş': 150,
    'Son Denetim': '2025-09-10',
    'Sonraki Denetim': '2026-03-10'
}
```

## Teknik Detaylar

### Global Değişkenler

```python
df_uyumluluk_global = None  # Uyumluluk verileri DataFrame
cari_bilgiler_path = r"X:\01.Public\TEDARİKÇİ PERFORMANS\Cari Bilgiler\Cari Bilgiler.xlsx"
```

### Sabitler

```python
COMPLIANCE_DATE_FORMAT = '%Y-%m-%d'
CERT_SCORE_PER_CERTIFICATE = 40 / 3
TOTAL_CERT_POINTS = 40
TOTAL_DELIVERY_POINTS = 30
TOTAL_QUALITY_POINTS = 30
```

### Yardımcı Fonksiyonlar

#### `load_cari_bilgiler()`
Excel dosyasından veri yükler veya örnek veri kullanır.

**Döndürür**: `True` (başarılı), `False` (örnek veri kullanıldı)

#### `check_certificate_expiry(row, cert_name, cert_date_col)`
Sertifika süresini kontrol eder.

**Parametreler**:
- `row`: DataFrame satırı
- `cert_name`: Sertifika adı kolonu
- `cert_date_col`: Bitiş tarihi kolonu

**Döndürür**: `(durum_metni, renk, öncelik)`

#### `calculate_compliance_score(row)`
Uyumluluk skorunu hesaplar (0-100).

**Parametreler**:
- `row`: DataFrame satırı

**Döndürür**: `float` (0-100 arası)

#### `get_risk_level(score)`
Risk seviyesini belirler.

**Parametreler**:
- `score`: Uyumluluk skoru

**Döndürür**: `(seviye_metni, renk)`

#### `calculate_complaint_rate(reklamasyon_count, total_orders)`
Reklamasyon oranını hesaplar.

**Parametreler**:
- `reklamasyon_count`: Reklamasyon sayısı
- `total_orders`: Toplam sipariş sayısı

**Döndürür**: `float` (yüzde olarak)

#### `ai_compliance_analysis(tedarikci_adi, row)`
AI destekli uyumluluk analizi yapar.

**Parametreler**:
- `tedarikci_adi`: Tedarikçi adı
- `row`: DataFrame satırı

**Döndürür**: `string` (AI analiz sonucu)

### Ana Fonksiyon

#### `init_compliance_center_tab()`
Tedarikçi Uyumluluk Merkezi sekmesini başlatır.

**İşlevler**:
1. Verileri yükler
2. UI bileşenlerini oluşturur
3. KPI kartlarını gösterir
4. Sertifika tablosunu doldurur
5. Grafikleri çizer
6. Denetim takvimini hazırlar
7. AI analiz bölümünü ekler

## Kullanım

### Modüle Erişim

1. Uygulamayı başlatın
2. Sol menüden "🏛️ Tedarikçi Uyumluluk Merkezi" seçeneğine tıklayın
3. Modül otomatik olarak veri yükler

### Excel Dosyası Yoksa

Excel dosyası bulunamazsa, sistem otomatik olarak örnek veri kullanır ve kullanıcıya bildirir.

### AI Analizi Kullanımı

1. Tedarikçi seçin (açılır menüden)
2. Gemini API Key'inizi girin
3. "🤖 AI Analizi Yap" butonuna tıklayın
4. Sonuçları okuyun

**Not**: API anahtarı güvenlik nedeniyle her oturumda yeniden girilmelidir.

## Güvenlik

### API Anahtarı
- Otomatik olarak doldurulmaz
- Her oturumda yeniden girilmelidir
- Maskelenerek gösterilir (`show="*"`)

### Veri Güvenliği
- Tüm işlemler activity logger ile kaydedilir
- Hata durumlarında detaylı log tutulur
- Hassas veriler ekrana yazılmaz

### Hata Yönetimi
- Tüm hata durumları yakalanır
- Kullanıcıya açıklayıcı mesajlar gösterilir
- Sistem çökmeden çalışmaya devam eder

## Activity Logger Entegrasyonu

Modül, aşağıdaki olayları loglar:

- **Veri Yükleme**: `log_data_load("Cari Bilgiler", count, "Tedarikçi Uyumluluk")`
- **Sayfa Ziyareti**: `log_page_visit("Tedarikçi Uyumluluk")`
- **AI Analizi**: `log_event("AI_ANALYSIS", supplier_name, "Tedarikçi Uyumluluk")`
- **Hatalar**: `log_error(error_message, "Tedarikçi Uyumluluk")`

## Performans

### Optimizasyonlar
- DataFrame işlemleri optimize edilmiştir
- Matplotlib grafikleri önbelleklenir
- Lazy loading kullanılır (sayfa açıldığında yüklenir)

### Bellek Kullanımı
- Veritabanı global değişkende tutulur
- Grafikler ihtiyaç duyulduğunda oluşturulur
- Eski widget'lar temizlenir

## Bakım ve Geliştirme

### Sertifika Ekleme

Yeni sertifika türü eklemek için:

1. Excel'e yeni kolonlar ekleyin: `[Sertifika Adı]`, `[Sertifika Adı] Bitiş`
2. `calculate_compliance_score()` fonksiyonunda `certs` listesine ekleyin
3. UI tablosunda yeni kolon ekleyin

### Skor Ağırlıklarını Değiştirme

```python
TOTAL_CERT_POINTS = 40      # Sertifika ağırlığı
TOTAL_DELIVERY_POINTS = 30  # Termin uyumu ağırlığı
TOTAL_QUALITY_POINTS = 30   # Kalite ağırlığı
```

### Tarih Formatı Değiştirme

```python
COMPLIANCE_DATE_FORMAT = '%Y-%m-%d'  # YYYY-MM-DD formatı
```

## Sorun Giderme

### "Veri yüklenemedi" Hatası
- Excel dosya yolunu kontrol edin
- Dosya izinlerini kontrol edin
- Örnek veri kullanılıyor olabilir (normal durum)

### "Gemini API Key girilmedi" Hatası
- API Key alanını doldurun
- Geçerli bir API key kullandığınızdan emin olun
- Internet bağlantınızı kontrol edin

### Grafikler Görünmüyor
- Matplotlib kütüphanesinin yüklü olduğundan emin olun
- Veri dosyasının doğru formatda olduğunu kontrol edin
- Konsol çıktılarını kontrol edin

### Tarih Hataları
- Tarihlerin YYYY-MM-DD formatında olduğundan emin olun
- Excel hücrelerinin "Text" veya "Date" formatında olduğunu kontrol edin

## Lisans ve Telif Hakkı

Bu modül "Son Tedarik" uygulamasının bir parçasıdır.
© 2024-2026 DeFacto

## Versiyon Geçmişi

### v1.0.0 (2026-02-10)
- İlk versiyon
- Tüm temel özellikler eklendi
- Sertifika takibi
- Uyumluluk skorlaması
- Risk matrisi
- Denetim takvimi
- AI analizi

## İletişim ve Destek

Sorularınız için:
- Email: ozan.sabudak@defacto.com
- Sistem: Son Tedarik v2.8
