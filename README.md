# Canva Sınıfı Rezervasyon Sistemi

**Bursa İl Millî Eğitim Müdürlüğü YEĞİTEK Birimi | BTSO Eğitim Vadisi**

Canva Sınıfı için ortak rezervasyon ve doluluk takvimi. Web sayfası **GitHub Pages** üzerinde yayınlanır; telefon, tablet ve bilgisayarda tarayıcıdan açılır. Kayıtlar arka planda bir **Google E-Tablo**'da tutulur. Böylece farklı cihazlardan girenler aynı saati seçemez.

Adres: `https://ugurcanolmez.github.io/CanvaBursa/`

## Dosya yapısı

```
CanvaBursa/
├── index.html                 ← Web sayfası (GitHub Pages bunu açar)
├── manifest.webmanifest       ← "Ana ekrana ekle" bilgisi (telefon/tablet)
├── icons/
│   ├── icon.svg               ← Sekme simgesi
│   ├── icon-192.png           ← Android ana ekran simgesi
│   ├── icon-512.png           ← Android açılış ekranı simgesi
│   └── apple-touch-icon.png   ← iPhone / iPad ana ekran simgesi
├── google-apps-script/        ← Sayfanın parçası değil, Google'a yapıştırılacak kod
│   └── Kod.gs                 ← Veri katmanı (kayıt, çakışma kilidi, iptal, CSV)
├── .nojekyll                  ← GitHub Pages'in dosyaları olduğu gibi yayınlaması için
└── README.md
```

GitHub'a yüklerken bu yapıyı koruyun: `index.html` deponun **ana klasöründe** olmalı.

## Kurulum

### 1. Google tarafı (bir kez, yaklaşık 10 dakika)

Bilgisayardan yapılması önerilir. Ayrıntılı anlatım için aşağıdaki **"Adım adım kurulum"** bölümüne bakın.

1. [script.new](https://script.new) adresinde `google-apps-script/Kod.gs` içeriğini yapıştırıp kaydedin.
2. **Proje Ayarları → Komut dosyası özellikleri** bölümüne `YONETICI_PIN` adıyla kendi PIN'inizi ekleyin.
3. `kurulum` fonksiyonunu çalıştırıp izinleri verin. Drive'ınızda "Canva Sınıfı Rezervasyonları" tablosu otomatik oluşur.
4. **Dağıt → Yeni dağıtım → Web uygulaması** seçin ("Ben" olarak yürüt, erişim "Herkes") ve `.../exec` linkini kopyalayın.

### 2. Sayfayı Google'a bağlama

`index.html` içinde şu satırı bulun:

```js
var API_URL = '';
```

Google'dan aldığınız linki tırnakların arasına yapıştırın. Bu satır boş kaldığı sürece sayfa **demo modunda** çalışır: kayıtlar yalnızca o cihazda kalır.

### 3. GitHub Pages'i açma

1. Dosyaları deponun ana klasörüne yükleyin.
2. **Settings → Pages → Build and deployment → Source: Deploy from a branch** seçin.
3. Dalı seçin, klasörü **/ (root)** bırakıp **Save**'e basın.
4. 1–2 dakika sonra sayfa açılır.

## Telefon ve tablet

- **Telefon:** Günler üstte sekme olarak görünür, seçilen günün saatleri alt alta listelenir.
- **Tablet ve bilgisayar:** Haftanın 5 günü yan yana görünür.
- **Ana ekrana ekleme:** Sayfa ana ekrana eklenebilir. iPhone/iPad'de Safari'de **Paylaş → Ana Ekrana Ekle**, Android'de Chrome menüsünde **Ana ekrana ekle**. Simgesiyle uygulama gibi tam ekran açılır.
- iPhone'daki çentik ve alt çubuk alanları hesaba katılmıştır.

## Özellikler

- Hafta içi (Pazartesi–Cuma), 09:00–12:00 ve 13:00–17:00 arası 1 saatlik bloklar (12:00–13:00 öğle arası).
- Birden fazla **ardışık** saat tek seferde seçilebilir.
- **Çakışma kilidi:** `LockService` aynı anda gelen istekleri sıraya alır. İlk gelen kaydedilir, diğerine "Bu saat aralığı az önce doldu" uyarısı çıkar.
- Dolu saatler kırmızı ve tıklanamaz. Üzerinde yalnızca okul/kurum ve eğitim konusu yazar; ad ve telefon gizlidir.
- Geçmiş saatler otomatik kilitlenir. Ekran her 30 saniyede bir ve sekmeye geri dönüldüğünde güncellenir.
- Kapasite kontrolü yapılır (varsayılan 30 kişi).
- **Yönetici İşlemi:** PIN ile giriş yapılır. Dolu saatlerde ad ve telefon görünür, "İptal Et / Boşalt" ve "Excel (CSV) İndir" butonları açılır. İptal edilen kayıt silinmez, tabloda "İptal" olarak işaretlenir.
- 10 dakikada 5 hatalı PIN denemesinden sonra giriş geçici olarak kapanır.

## Ayarlar (`google-apps-script/Kod.gs` başındaki `AYARLAR`)

| Ayar | Açıklama |
|---|---|
| `TABLO_ID` | Kayıtların tutulduğu Google E-Tablo |
| `YONETICI_PIN` | Yedek PIN. Asıl PIN'i Komut Dosyası Özellikleri'ne yazın |
| `KAPASITE` | En fazla katılımcı sayısı |
| `ILERI_GUN_SINIRI` | Kaç gün sonrasına kadar rezervasyon alınacağı |
| `SAATLER` | Saat blokları |

Kodda değişiklik yaparsanız: **Dağıt → Dağıtımları yönet → Düzenle (kalem) → Sürüm: Yeni sürüm → Dağıt.** Böylece link değişmez.

> ⚠️ Depo herkese açıksa gerçek PIN'i `Kod.gs` içine yazıp GitHub'a göndermeyin. Komut Dosyası Özellikleri'ni kullanın.

## Adım adım kurulum (yeni başlayanlar için)

> Bilgisayardan yapın. Google hesabınıza giriş yapmış olun. Kayıtlar bu hesabın Drive'ında tutulacak.

### A. Kodu kopyalayın
1. Tarayıcıda `github.com/ugurcanolmez/CanvaBursa` adresini açın.
2. `google-apps-script` klasörüne, ardından `Kod.gs` dosyasına tıklayın.
3. Kodun sağ üst köşesindeki **iki üst üste kare** simgesine ("Copy raw file") tıklayın. Kodun tamamı kopyalanır.

### B. Google'a yapıştırın
1. Yeni sekmede **script.new** adresini açın. Google Apps Script editörü açılır.
2. Ekranda `function myFunction() { }` yazan kod var. Önce **Ctrl+A**, ardından **Delete** ile silin.
3. **Ctrl+V** ile kopyaladığınız kodu yapıştırın.
4. Sol üstteki **"Adsız proje"** yazısına tıklayıp adını `Canva Sınıfı Rezervasyon` yapın.
5. **Ctrl+S** ile ya da üstteki disket simgesiyle kaydedin.

### C. Yönetici PIN'ini belirleyin
1. Soldaki dar menüde en alttaki **dişli çark (Proje Ayarları)** simgesine tıklayın.
2. Sayfanın en altına inin, **"Komut dosyası özelliği ekle"** butonuna basın.
3. **Özellik** kutusuna `YONETICI_PIN`, **Değer** kutusuna kendi PIN'inizi yazın (örn. `4826`).
4. **"Komut dosyası özelliklerini kaydet"** butonuna basın.

### D. Kurulumu çalıştırın ve izin verin
1. Soldaki menüden **`< >` (Düzenleyici)** simgesine tıklayıp koda dönün.
2. Üst çubukta **"Çalıştır"** butonunun yanındaki açılır listeden `kurulum` seçin.
3. **Çalıştır**'a basın. "Yetkilendirme gerekli" penceresi çıkar → **İzinleri incele**.
4. Google hesabınızı seçin.
5. **"Google bu uygulamayı doğrulamadı"** uyarısı çıkar. Bu normaldir, uygulamayı siz yazdığınız için çıkar. Sol alttaki **Gelişmiş** → **"Canva Sınıfı Rezervasyon (güvenli değil) sayfasına git"** → **İzin ver**.
6. Alttaki "Yürütme günlüğü" kısmında **"Kurulum tamam. Kayıtlar bu tabloda tutulacak: https://docs.google.com/..."** yazısını görürsünüz. Drive'ınızda "Canva Sınıfı Rezervasyonları" tablosu oluşmuştur.

### E. Yayına alın ve linki alın
1. Sağ üstteki mavi **Dağıt** butonu → **Yeni dağıtım**.
2. "Tür seçin" yazısının yanındaki **dişli çark** → **Web uygulaması**.
3. Ayarlar:
   - **Açıklama:** `Canva Sınıfı` (isteğe bağlı)
   - **Şu kullanıcı olarak yürüt:** **Ben**
   - **Erişimi olanlar:** **Herkes**. "Google hesabı olan herkes" seçeneğini değil, düz **Herkes**'i seçin.
4. **Dağıt**'a basın. Yeniden izin isterse D adımındaki gibi izin verin.
5. **Web uygulaması URL'si** altında `https://script.google.com/macros/s/...../exec` linki çıkar. **Kopyala**'ya basın.
6. Kontrol için linki yeni sekmede açın. `"durum":"çalışıyor"` yazısı görünüyorsa her şey yolunda.

### F. Linki sayfaya ekleyin
1. GitHub'da deponun ana sayfasında `index.html` dosyasına tıklayın.
2. Sağ üstteki **kalem (Edit)** simgesine tıklayın.
3. **Ctrl+F** ile `API_URL` arayın. `var API_URL = '';` satırını bulun.
4. İki tek tırnağın **arasına** linki yapıştırın. Satır şöyle görünmeli:
   `var API_URL = 'https://script.google.com/macros/s/...../exec';`
5. Sağ üstteki yeşil **Commit changes...** → tekrar **Commit changes**.
6. 1–2 dakika sonra sayfayı yenileyin. Sarı "Demo modu" yazısı kaybolduysa sistem bağlanmıştır.

### Sorun çıkarsa
- **Sayfada "Veriler alınamadı" yazıyor:** E3'te erişim büyük ihtimalle "Herkes" değil. Dağıt → Dağıtımları yönet → kalem → Erişim: Herkes → Dağıt.
- **Kodu değiştirdim ama etkisi yok:** Dağıt → Dağıtımları yönet → kalem → Sürüm: **Yeni sürüm** → Dağıt. Böylece link aynı kalır.
- **Kayıtları görmek:** Drive'daki "Canva Sınıfı Rezervasyonları" tablosunu açın.
