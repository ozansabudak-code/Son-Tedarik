# Uyumluluk Analizi Sorun Giderme Kılavuzu

## Sorun: Butonlar ve Formlar Görünmüyor

### Debug Modu Aktif!

Programı çalıştırdığınızda artık konsol penceresinde (command prompt / terminal) debug mesajları göreceksiniz:

```
🔧 DEBUG: Sayfa gösteriliyor - Uyumluluk Analizi
🔧 DEBUG: init_compliance_analysis_tab() başlatıldı
🔧 DEBUG: Veri yükleniyor...
🔧 DEBUG: Veri yüklendi - Ana: 5 kayıt
🔧 DEBUG: Bilgi paneli oluşturuluyor...
🔧 DEBUG: Bilgi paneli oluşturuldu ✓
🔧 DEBUG: AI butonu oluşturuluyor...
🔧 DEBUG: AI butonu oluşturuldu ✓
🔧 DEBUG: Kaydetme butonu oluşturuluyor...
🔧 DEBUG: Kaydetme butonu oluşturuldu ✓
🔧 DEBUG: Tedarikçi ekleme formu oluşturuluyor...
🔧 DEBUG: Tedarikçi ekleme formu oluşturuldu ✓
✅ BAŞARILI: init_compliance_analysis_tab() tamamlandı - Tüm UI elemanları oluşturuldu!
```

### Adım Adım Kontrol

#### 1. Programı Konsol ile Başlatın
```bash
# Windows'ta:
python "Son Tedarik"

# veya
python3 "Son Tedarik"
```

Programı çift tıklayarak DEĞIL, komut satırından başlatın. Böylece debug mesajlarını görebilirsiniz.

#### 2. Debug Mesajlarını Kontrol Edin

**EĞER Debug Mesajları Görünüyorsa:**
- ✅ Fonksiyon çalışıyor
- ✅ UI elemanları oluşturuluyor
- ❓ Görünmeme sorunu layout/rendering ile ilgili olabilir

**EĞER Hata Mesajı Görüyorsanız:**
- ❌ Bir hata var ve fonksiyon tamamlanamıyor
- 📋 Hata mesajını kopyalayın ve paylaşın

**EĞER Hiç Mesaj Görünmüyorsa:**
- ❌ Sayfa hiç açılmıyor
- ❌ Menü tıklaması çalışmıyor
- 🔍 Doğru sekmeye tıkladığınızdan emin olun

#### 3. Doğru Sekmeyi Kontrol Edin

Soldaki menüde arayın:
- **📊 Uyumluluk Analizi** ← BU DOĞRU SEKME
- (NOT: "🏛️ Sertifika & Sözleşme" FARKLI bir sekmedir)

#### 4. Sayfayı Yenileyin

Programı kapatıp tekrar açın VEYA:
- "📊 Uyumluluk Analizi" sekmesine gidin
- Sayfanın en üstünde MAVİ bir panel olmalı
- O panelde "🔄 Verileri Yenile" butonu olmalı
- Bu butona tıklayın

#### 5. Python Cache'i Temizleyin

```bash
# __pycache__ klasörlerini silin
find . -type d -name "__pycache__" -exec rm -rf {} +

# .pyc dosyalarını silin
find . -name "*.pyc" -delete
```

### Beklenen UI Elemanları

Eğer her şey çalışıyorsa görecekleriniz:

1. **Başlık Bölümü** (Turkuaz arka plan)
   - 📁 Ana Veri durumu
   - 📜 Sertifika Verisi durumu

2. **Mavi Bilgi Paneli** (Sayfanın üst kısmında)
   - ⭐ ÖNEMLİ BİLGİ başlığı
   - Butonların yeri hakkında bilgi
   - 🔄 Verileri Yenile butonu

3. **Tablolar ve Grafikler** (Kaydırarak aşağı inin)
   - KPI kartları
   - Detaylı bilgi tablosu
   - Sertifika durumu tablosu
   - Grafikler
   - Denetim takvimi

4. **AI Analizi Bölümü** (Mor çerçeveli)
   - 🤖 AI İLE ANALİZ YAP butonu (BÜYÜK MOR BUTON)

5. **Kaydetme Bölümü** (Yeşil çerçeveli)
   - 💾 Excel'e Kaydet butonu (BÜYÜK YEŞİL BUTON)

6. **Tedarikçi Ekleme Formu** (Mavi çerçeveli)
   - Tedarikçi Adı alanı
   - Yetkili Kişi alanı
   - Telefon alanı
   - Mail alanı
   - ➕ Tedarikçi Ekle butonu

### Yaygın Sorunlar ve Çözümler

#### Sorun: "Hiçbir şey görünmüyor"
**Çözüm:**
1. Doğru sekmeye tıkladığınızdan emin olun
2. Sayfayı AŞAĞI kaydırın (scroll yapın)
3. Pencereyi büyütün (tam ekran yapın)

#### Sorun: "Sadece başlık görünüyor"
**Çözüm:**
1. Debug mesajlarına bakın
2. Bir hata var mı kontrol edin
3. Veri yüklenmiş mi kontrol edin

#### Sorun: "Tablolar var ama butonlar yok"
**Çözüm:**
1. AŞAĞI kaydırmaya devam edin
2. Sayfanın sonuna kadar inin
3. Kaydırma çubuğu çalışıyor mu kontrol edin

### Test Scripti

Eğer hala sorun varsa, bu test scriptini çalıştırın:

```python
#!/usr/bin/env python3
# test_ui_elements.py

with open('Son Tedarik', 'r', encoding='utf-8') as f:
    content = f.read()

elements = [
        ('init_compliance_analysis_tab', 'Ana fonksiyon'),
        ('AI İLE ANALİZ YAP', 'AI butonu'),
        ('Excel\'e Kaydet', 'Kaydet butonu'),
        ('Yeni Tedarikçi Ekle', 'Ekleme formu'),
        ('ÖNEMLİ BİLGİ', 'Bilgi paneli'),
        ('Verileri Yenile', 'Yenile butonu')
    ]

print("UI Eleman Kontrolü:")
print("-" * 50)
for text, desc in elements:
    if text in content:
        print(f"✅ {desc}: BULUNDU")
    else:
        print(f"❌ {desc}: BULUNAMADI")
```

### Destek İçin

Eğer sorun devam ediyorsa:

1. **Debug çıktısını** kopyalayın
2. **Hata mesajlarını** (varsa) kopyalayın
3. **Hangi sekmeye** baktığınızı belirtin
4. **Ekran görüntüsü** alın

Bu bilgilerle sorunu daha iyi teşhis edebiliriz.

---

## Son Değişiklikler

**Eklenen Debug Özellikleri:**
- 🔧 Fonksiyon başlangıcında debug mesajı
- 🔧 Her UI elemanı oluşturulduğunda onay mesajı
- 🔧 Fonksiyon sonunda başarı mesajı
- ❌ Hata durumunda detaylı hata mesajı
- 📋 Try-except blokları ile hata yakalama

**Son Commit:**
- Tüm UI elemanları hala yerinde
- Debug logging eklendi
- Hata yakalama mekanizması eklendi
