# Canva Sınıfı Rezervasyon Sistemi

**Bursa İl Millî Eğitim Müdürlüğü YEĞİTEK Birimi | BTSO Eğitim Vadisi**

Canva Sınıfı için ortak rezervasyon ve doluluk takvimi. Google E-Tablolar + Google Apps Script ile çalışır, sunucu veya ücret gerektirmez.

## Özellikler

- Hafta içi (Pazartesi–Cuma), 09:00–12:00 ve 13:00–17:00 arası 1 saatlik bloklar (12:00–13:00 öğle arası)
- Birden fazla **ardışık** saat tek seferde seçilebilir
- **Çakışma kilidi:** `LockService` ile aynı anda gelen istekler sıraya alınır; ilk gelen kaydedilir, diğerine "Bu saat aralığı az önce doldu" uyarısı çıkar
- Dolu saatler kırmızı ve tıklanamaz; üzerinde okul/kurum ve eğitim konusu yazar (telefon ve ad gizli)
- Geçmiş saatler otomatik kilitlenir; ekran her 30 saniyede bir kendini günceller
- Kapasite kontrolü (varsayılan 30 kişi)
- **Yönetici İşlemi:** PIN ile giriş → dolu saatlerde ad/telefon görünür, "İptal Et / Boşalt" ve "Excel (CSV) İndir" butonları açılır. İptaller silinmez, tabloda "İptal" olarak işaretlenir
- 10 dakikada 5 hatalı PIN denemesinden sonra giriş geçici olarak kapanır
- Telefon ve masaüstü uyumlu

## Dosyalar

| Dosya | Açıklama |
|---|---|
| `Kod.gs` | Sunucu kodu (Apps Script) |
| `index.html` | Arayüz (Tailwind CSS). Apps Script dışında açılırsa demo modunda çalışır (PIN: 1234) |
| `appsscript.json` | Proje ayarları (saat dilimi: İstanbul) |

## Yayına alma (5 adım)

1. **E-Tabloyu açın:** Tablo hazır: [Canva Sınıfı Rezervasyonları](https://docs.google.com/spreadsheets/d/1m8okHB0eAxoZrhW9Q-iNMniK5J25nhYOgDmCYfNh-ZI/edit) (stemscientixbursa@gmail.com Drive'ında; `Kod.gs` içindeki `TABLO_ID` bu tabloya bağlı). Farklı bir tablo kullanacaksanız `TABLO_ID` değerini değiştirin veya boş bırakın.
2. **Kodu yapıştırın:** Menüden **Uzantılar → Apps Script**'i açın. `Kod.gs` içeriğini mevcut kodun yerine yapıştırın. Sol taraftaki **+ → HTML** ile `index` adında bir dosya oluşturup `index.html` içeriğini yapıştırın. Kaydedin (Ctrl+S).
3. **PIN'i değiştirin ve kurun:** `Kod.gs` başındaki `YONETICI_PIN: '1234'` değerini kendi PIN'inizle değiştirin. Üstteki fonksiyon listesinden `kurulum`'u seçip **Çalıştır**'a basın, Google'ın istediği izinleri verin. E-Tablo'da "Rezervasyonlar" sayfası oluşacak.
4. **Web uygulaması olarak dağıtın:** **Dağıt → Yeni dağıtım → Tür: Web uygulaması**. "Şu kullanıcı olarak çalıştır: **Ben**", "Erişimi olanlar: **Herkes**" seçip **Dağıt**'a basın.
5. **Linki paylaşın:** Size verilen `https://script.google.com/macros/s/.../exec` linkini öğretmenlerle paylaşın (WhatsApp, e-posta, QR kod). Tüm kayıtlar E-Tablo'nuza düşer.

> Kodda değişiklik yaparsanız: **Dağıt → Dağıtımları yönet → Düzenle (kalem) → Sürüm: Yeni sürüm → Dağıt.** Böylece link değişmez.

### Ayarlar (`Kod.gs` başındaki `AYARLAR`)

- `YONETICI_PIN` – yönetici PIN'i (daha güvenli yol: Proje Ayarları → Komut Dosyası Özellikleri'ne `YONETICI_PIN` ekleyin)
- `KAPASITE` – en fazla katılımcı sayısı
- `ILERI_GUN_SINIRI` – kaç gün sonrasına kadar rezervasyon alınacağı
- `SAATLER` – saat blokları
