# Sertifika Durumu Trafik Işığı - Excel Dosya Formatı

## Dosya Bilgileri

**Dosya Adı:** `Sertifika Durumu Trafik ışığı.xlsx`

**Dosya Yolu:** `X:\01.Public\TEDARİKÇİ PERFORMANS\Cari Bilgiler\Sertifika Durumu Trafik ışığı.xlsx`

---

## Excel Kolon Başlıkları (11 Kolon)

Excel dosyanızda **tam olarak** aşağıdaki başlıklar olmalıdır:

| # | Kolon Adı | Açıklama | Örnek Değer | Veri Tipi |
|---|-----------|----------|-------------|-----------|
| 1 | **Tedarikçi Adı** | Tedarikçi firma adı | ABC Tekstil | Metin |
| 2 | **ISO 9001** | ISO 9001 sertifikası durumu | Var / Yok | Metin |
| 3 | **ISO 9001 Bitiş** | ISO 9001 sertifika bitiş tarihi | 2026-06-15 | Tarih (YYYY-MM-DD) |
| 4 | **OEKO-TEX** | OEKO-TEX sertifikası durumu | Var / Yok | Metin |
| 5 | **OEKO-TEX Bitiş** | OEKO-TEX sertifika bitiş tarihi | 2026-09-20 | Tarih (YYYY-MM-DD) |
| 6 | **BSCI** | BSCI sertifikası durumu | Var / Yok | Metin |
| 7 | **BSCI Bitiş** | BSCI sertifika bitiş tarihi | 2026-04-10 | Tarih (YYYY-MM-DD) |
| 8 | **ISO 14001** | ISO 14001 sertifikası durumu | Var / Yok | Metin |
| 9 | **ISO 14001 Bitiş** | ISO 14001 sertifika bitiş tarihi | 2026-03-20 | Tarih (YYYY-MM-DD) |
| 10 | **SEDEX** | SEDEX sertifikası durumu | Var / Yok | Metin |
| 11 | **SEDEX Bitiş** | SEDEX sertifika bitiş tarihi | 2026-12-30 | Tarih (YYYY-MM-DD) |

---

## Örnek Excel Satırı

```
Tedarikçi Adı | ISO 9001 | ISO 9001 Bitiş | OEKO-TEX | OEKO-TEX Bitiş | BSCI | BSCI Bitiş | ISO 14001 | ISO 14001 Bitiş | SEDEX | SEDEX Bitiş
ABC Tekstil   | Var      | 2026-06-15     | Var      | 2026-09-20     | Var  | 2026-04-10 | Var       | 2026-03-20      | Var   | 2026-12-30
XYZ Kimya     | Var      | 2025-12-30     | Yok      |                | Var  | 2026-07-25 | Yok       |                 | Var   | 2026-01-15
```

---

## Önemli Notlar

### 📋 Sertifika Durumu Değerleri
- **"Var"** = Sertifika mevcut
- **"Yok"** = Sertifika yok
- Büyük/küçük harf duyarlı değil ama standart kullanım önerilir

### 📅 Tarih Formatı
- **YYYY-MM-DD** formatında olmalı (örn: 2026-06-15)
- Excel'de tarih olarak veya metin olarak saklanabilir
- Sertifika yoksa bu alan boş bırakılabilir

### ⚠️ Boş Değerler
- Sertifika "Yok" ise, bitiş tarihi sütunu boş bırakılabilir
- Boş hücreler otomatik olarak işlenir

### 🔄 UI'da Görünen Sütunlar
Uygulamada **8 sütun** görüntülenir:
1. Tedarikçi
2. ISO 9001 (durum + trafik ışığı)
3. OEKO-TEX (durum + trafik ışığı)
4. BSCI (durum + trafik ışığı)
5. ISO 14001 (durum + trafik ışığı)
6. SEDEX (durum + trafik ışığı)
7. Uyumluluk Skoru (ana dosyadan)
8. Risk (ana dosyadan)

**Not:** "Uyumluluk Skoru" ve "Risk" sütunları, ana "Cari Bilgiler.xlsx" dosyasından hesaplanır ve bu dosyada olması gerekmez.

---

## Trafik Işığı Göstergeleri

Sistem, sertifika bitiş tarihlerine göre otomatik trafik ışığı gösterir:

- 🔴 **Kırmızı**: Süresi dolmuş
- 🟠 **Turuncu**: 30 gün veya daha az kaldı
- 🟡 **Sarı**: 90 gün veya daha az kaldı
- 🟢 **Yeşil**: 90 günden fazla kaldı
- ⚫ **Siyah**: Sertifika yok

---

## Excel Dosyası Hazırlama Adımları

1. **Excel'i açın** ve yeni bir çalışma sayfası oluşturun
2. **İlk satıra** yukarıdaki 11 kolon başlığını yazın
3. **İkinci satırdan itibaren** tedarikçi bilgilerini ekleyin
4. **Dosyayı kaydedin** belirtilen yola:
   ```
   X:\01.Public\TEDARİKÇİ PERFORMANS\Cari Bilgiler\Sertifika Durumu Trafik ışığı.xlsx
   ```
5. **Uygulamayı yeniden başlatın** veya "Uyumluluk Analizi" sekmesini yenileyin

---

## Hata Durumları

### Dosya Bulunamazsa
Eğer Excel dosyası belirtilen konumda yoksa:
- Uygulama otomatik olarak örnek veri kullanır
- Activity log'a "Sertifika trafik örnek veri kullanıldı" mesajı yazılır
- Kullanıcıya bilgi verilmez (sessizce çalışır)

### Kolon Adları Yanlışsa
- Pandas read_excel, kolon adlarına göre veri okur
- Yanlış adlandırılmış kolonlar okunmaz
- Boş veya hatalı veriler "Yok" olarak işlenir

---

## Sık Sorulan Sorular

### S: Tüm sertifikalar zorunlu mu?
**C:** Hayır. İstediğiniz sertifikaları "Yok" olarak işaretleyebilirsiniz.

### S: Yeni sertifika türü eklenebilir mi?
**C:** Evet, ancak kod değişikliği gerektirir. Şu anda 5 sertifika tipi desteklenmektedir.

### S: Tarih formatı değiştirilebilir mi?
**C:** Hayır, kod YYYY-MM-DD formatını beklemektedir.

### S: Excel dosyasını nasıl güncellerim?
**C:** Dosyayı düzenleyip kaydettikten sonra, uygulamada "Uyumluluk Analizi" sekmesini yeniden yükleyin.

---

## Destek

Sorularınız için: ozan.sabudak@defacto.com
