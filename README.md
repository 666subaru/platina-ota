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
manifest/                   # pinned source manifests
patches/                    # local changes on top of upstream
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

### Source code

This ROM is built from public sources; nothing is closed except the prebuilt vendor
blobs and GApps that ship with every build of this kind.

| Component | Repository | Branch |
|---|---|---|
| ROM | [ProjectInfinity-X/manifest](https://github.com/ProjectInfinity-X/manifest) | `16` |
| Device | [sabarop/android_device_xiaomi_platina](https://github.com/sabarop/android_device_xiaomi_platina) | `16.2` |
| Common device | [sabarop/android_device_xiaomi_sdm660-common](https://github.com/sabarop/android_device_xiaomi_sdm660-common) | `16.2-dynamic-pixel` |
| Kernel (4.19) | [sabarop/kernel_xiaomi_sdm660](https://github.com/sabarop/kernel_xiaomi_sdm660) | `lineage-23.2-ksu` |
| Vendor | [sabarop/android_vendor_xiaomi_platina](https://github.com/sabarop/android_vendor_xiaomi_platina), [sdm660-common](https://github.com/sabarop/android_vendor_xiaomi_sdm660-common) | `16`, `16-Inf` |

- [`manifest/r5-pinned.xml`](manifest/r5-pinned.xml) — exact commit of every project used for the current release
- [`manifest/local_manifest.xml`](manifest/local_manifest.xml) — device-specific repositories
- [`patches/`](patches/) — our local changes, with an explanation of each

The Linux kernel is licensed under GPL-2.0; its full source for each release is the pinned
kernel commit plus [`patches/kernel_xiaomi_sdm660.patch`](patches/kernel_xiaomi_sdm660.patch).

### Credits

[Project Infinity-X](https://github.com/ProjectInfinity-X), [LineageOS](https://github.com/LineageOS),
and [sabarop](https://github.com/sabarop) for the `platina` device, vendor and kernel trees.

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
manifest/                   # sabitlenmiş kaynak manifestleri
patches/                    # upstream üzerine yerel değişiklikler
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

### Kaynak kod

Bu ROM public kaynaklardan derlenir; bu tür her yapıda olduğu gibi yalnızca hazır vendor
dosyaları ve GApps kapalı kaynaktır.

| Bileşen | Depo | Dal |
|---|---|---|
| ROM | [ProjectInfinity-X/manifest](https://github.com/ProjectInfinity-X/manifest) | `16` |
| Cihaz | [sabarop/android_device_xiaomi_platina](https://github.com/sabarop/android_device_xiaomi_platina) | `16.2` |
| Ortak cihaz | [sabarop/android_device_xiaomi_sdm660-common](https://github.com/sabarop/android_device_xiaomi_sdm660-common) | `16.2-dynamic-pixel` |
| Kernel (4.19) | [sabarop/kernel_xiaomi_sdm660](https://github.com/sabarop/kernel_xiaomi_sdm660) | `lineage-23.2-ksu` |
| Vendor | [sabarop/android_vendor_xiaomi_platina](https://github.com/sabarop/android_vendor_xiaomi_platina), [sdm660-common](https://github.com/sabarop/android_vendor_xiaomi_sdm660-common) | `16`, `16-Inf` |

- [`manifest/r5-pinned.xml`](manifest/r5-pinned.xml) — güncel sürümde kullanılan her projenin tam commit'i
- [`manifest/local_manifest.xml`](manifest/local_manifest.xml) — cihaza özel depolar
- [`patches/`](patches/) — yerel değişikliklerimiz ve her birinin açıklaması

Linux kernel GPL-2.0 lisanslıdır; her sürümün tam kaynağı, sabitlenen kernel commit'i ve
[`patches/kernel_xiaomi_sdm660.patch`](patches/kernel_xiaomi_sdm660.patch) dosyasıdır.

### Teşekkürler

[Project Infinity-X](https://github.com/ProjectInfinity-X), [LineageOS](https://github.com/LineageOS)
ve `platina` cihaz, vendor ve kernel ağaçları için [sabarop](https://github.com/sabarop).

### Notlar

- Depo **public** olmak zorunda: cihaz `raw.githubusercontent.com`'a kimlik doğrulamadan erişir.
- GitHub Releases dosya başına **2 GB** sınırı koyar. Mevcut paket 1,86 GB — sınıra yakın.
  Aşılırsa `download` adresi başka bir barındırıcıya taşınabilir; ROM'da gömülü olan
  yalnızca JSON'un adresidir.

---

## License / Lisans

The repository's own files (README, JSON, notes) are under the [MIT License](LICENSE).
Patches follow the license of the project they modify; see [`patches/README.md`](patches/README.md).
ROM packages contain third-party software under their respective licenses.

Deponun kendi dosyaları (README, JSON, notlar) [MIT Lisansı](LICENSE) altındadır.
Yamalar değiştirdikleri projenin lisansına tabidir; bkz. [`patches/README.md`](patches/README.md).
ROM paketleri, kendi lisanslarına tabi üçüncü taraf yazılımlar içerir.
