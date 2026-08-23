# platina-ota

OTA güncelleme sunucusu — **Xiaomi Mi 8 Lite (`platina`)** için kaynaktan derlenen
[Project Infinity-X](https://projectinfinity-x.com/) (Android 16) yapıları.

Cihazdaki Updater uygulaması `16/gapps/platina.json` dosyasını okur, ROM paketleri
Releases bölümünde tutulur.

## Yapı

```
16/gapps/platina.json    # Updater'ın sorguladığı dosya
```

## JSON şeması

```json
{
  "response": [
    {
      "timestamp": 1787474517,          // unix saniye — cihazdaki ro.build.date.utc'den BÜYÜK olmalı
      "filename":  "...zip",
      "md5":       "...",               // Updater indirmeyi bununla doğrular
      "size":      1865376717,          // bayt
      "download":  "https://.../...zip",
      "version":   "3.12"
    }
  ]
}
```

`timestamp` cihazdaki yapıdan büyük değilse Updater "güncelleme yok" der.

## Yeni sürüm yayınlama

```bash
# 1) JSON üret
ota/json-uret.sh <zip> https://github.com/666subaru/platina-ota/releases/download/<etiket> <sürüm>

# 2) Zip'i Releases'e yükle
gh release create <etiket> <zip> --title "..." --notes "..."

# 3) JSON'u güncelle ve gönder
git add 16/gapps/platina.json && git commit -m "..." && git push
```

## Notlar

- Depo **public** olmak zorunda: cihaz `raw.githubusercontent.com`'a kimlik doğrulamadan erişir.
- GitHub Releases dosya başına **2 GB** sınırı koyar. Mevcut paket 1,86 GB — sınıra yakın.
  Aşılırsa `download` adresi başka bir barındırıcıya taşınabilir; ROM'da gömülü olan
  yalnızca JSON'un adresidir, indirme adresi serbestçe değiştirilebilir.
