# Tedarikçi Uyumluluk Merkezi - Dokümantasyon

## Genel Bakış

"Son Tedarik" uygulamasına eklenen **Tedarikçi Uyumluluk Merkezi** modülü, tedarikçi sertifikalarının takibi, uyumluluk skorlaması, risk analizi, denetim takvimi ve AI destekli uyumluluk değerlendirmesi sağlar.

## Özellikler

### 1. Sertifika Takibi (Trafik Işığı Sistemi)
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
