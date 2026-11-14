# platina-ota

🇬🇧 [English](#-english) · 🇹🇷 [Türkçe](#-türkçe)

---

## 🇬🇧 English

OTA update server for unofficial [Project Infinity-X](https://projectinfinity-x.com/)
(Android 16) builds for the **Xiaomi Mi 8 Lite (`platina`)**, compiled from source.

The Updater app on the device reads `16/gapps/platina.json`; ROM packages are
stored under Releases.

### Layout

```
16/gapps/platina.json       # file the Updater queries
16/changelog/platina.txt    # release notes shown in the Updater (Turkish)
```

### JSON schema

```json
{
  "response": [
    {
      "timestamp": 1794676644,          // unix seconds — must be GREATER than ro.build.date.utc on the device
      "filename":  "...zip",
      "md5":       "...",               // Updater verifies the download with this
      "size":      1861390261,          // bytes
      "download":  "https://.../...zip",
      "version":   "3.12-r5",
      "romtype":   "UNOFFICIAL"
    }
  ]
}
```

If `timestamp` is not greater than the installed build, the Updater reports "no update".

### Publishing a new release

```bash
# 1) Generate JSON
ota/json-uret.sh <zip> https://github.com/666subaru/platina-ota/releases/download/<tag> <version>

# 2) Upload the zip to Releases
gh release create <tag> <zip> --title "..." --notes-file notes.md

# 3) Update and push the JSON + changelog
git add 16/ && git commit -m "..." && git push
```

Afterwards, check that `raw.githubusercontent.com/.../main/...` serves the new
content — the raw CDN has occasionally kept old content for up to an hour.
Pushing one more small commit clears it.

### Notes

- The repository **must be public**: the device fetches `raw.githubusercontent.com` without authentication.
- GitHub Releases has a **2 GB** per-file limit. Current package is 1.86 GB — close to the limit.
  If exceeded, `download` can point to another host; only the JSON URL is baked into the ROM.

---

## 🇹🇷 Türkçe

**Xiaomi Mi 8 Lite (`platina`)** için kaynaktan derlenen, resmi olmayan
[Project Infinity-X](https://projectinfinity-x.com/) (Android 16) yapılarının OTA güncelleme sunucusu.

Cihazdaki Updater uygulaması `16/gapps/platina.json` dosyasını okur, ROM paketleri
Releases bölümünde tutulur.

### Yapı

```
16/gapps/platina.json       # Updater'ın sorguladığı dosya
16/changelog/platina.txt    # Updater'da görünen sürüm notları (Türkçe)
```

### JSON şeması

```json
{
  "response": [
    {
      "timestamp": 1794676644,          // unix saniye — cihazdaki ro.build.date.utc'den BÜYÜK olmalı
      "filename":  "...zip",
      "md5":       "...",               // Updater indirmeyi bununla doğrular
      "size":      1861390261,          // bayt
      "download":  "https://.../...zip",
      "version":   "3.12-r5",
      "romtype":   "UNOFFICIAL"
    }
  ]
}
```

`timestamp` cihazdaki yapıdan büyük değilse Updater "güncelleme yok" der.

### Yeni sürüm yayınlama

```bash
# 1) JSON üret
ota/json-uret.sh <zip> https://github.com/666subaru/platina-ota/releases/download/<etiket> <sürüm>

# 2) Zip'i Releases'e yükle
gh release create <etiket> <zip> --title "..." --notes-file notlar.md

# 3) JSON'u ve changelog'u güncelle, gönder
git add 16/ && git commit -m "..." && git push
```

Sonra `raw.githubusercontent.com/.../main/...` adresinin yeni içeriği verdiğini
kontrol et — raw CDN bazen eski içeriği bir saate kadar tutabiliyor.
Küçük bir commit daha atmak önbelleği temizliyor.

### Notlar

- Depo **public** olmak zorunda: cihaz `raw.githubusercontent.com`'a kimlik doğrulamadan erişir.
- GitHub Releases dosya başına **2 GB** sınırı koyar. Mevcut paket 1,86 GB — sınıra yakın.
  Aşılırsa `download` adresi başka bir barındırıcıya taşınabilir; ROM'da gömülü olan
  yalnızca JSON'un adresidir.
