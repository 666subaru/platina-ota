# Patches / Yamalar

🇬🇧 Local changes applied on top of the upstream sources for the current release (**3.12-r5**).
Each file is a `git diff` against the pinned commit in [`../manifest/r5-pinned.xml`](../manifest/r5-pinned.xml),
applied from the root of the corresponding project.

🇹🇷 Güncel sürüm (**3.12-r5**) için upstream kaynakların üzerine uygulanan yerel değişiklikler.
Her dosya, [`../manifest/r5-pinned.xml`](../manifest/r5-pinned.xml) içindeki sabit commit'e karşı alınmış bir
`git diff`'tir ve ilgili projenin kök dizininden uygulanır.

```bash
cd kernel/xiaomi/sdm660 && git apply /path/to/patches/kernel_xiaomi_sdm660.patch
```

| Patch | Project / Proje | License / Lisans |
|---|---|---|
| `kernel_xiaomi_sdm660.patch` | [sabarop/kernel_xiaomi_sdm660](https://github.com/sabarop/kernel_xiaomi_sdm660) | GPL-2.0 |
| `device_xiaomi_platina.patch` | [sabarop/android_device_xiaomi_platina](https://github.com/sabarop/android_device_xiaomi_platina) | Apache-2.0 |
| `device_xiaomi_sdm660-common.patch` | [sabarop/android_device_xiaomi_sdm660-common](https://github.com/sabarop/android_device_xiaomi_sdm660-common) | Apache-2.0 |
| `vendor_xiaomi_sdm660-common.patch` | [sabarop/android_vendor_xiaomi_sdm660-common](https://github.com/sabarop/android_vendor_xiaomi_sdm660-common) | build files only / yalnız derleme dosyaları |
| `packages_apps_Updater.patch` | [ProjectInfinity-X/packages_apps_Updater](https://github.com/ProjectInfinity-X/packages_apps_Updater) | Apache-2.0 |

Each patch is distributed under the license of the project it modifies.
Her yama, değiştirdiği projenin lisansı altında dağıtılır.

---

## 🇬🇧 What each patch does

### `kernel_xiaomi_sdm660.patch`
- **Disable CFI** — adds `arch/arm64/configs/vendor/xiaomi/platina-fix.config`, applied as the last
  config fragment. With `CONFIG_CFI_CLANG=y`, a function-pointer type mismatch in the Venus video
  encoder's HFI interface caused a fatal panic when starting a video recording
  (`kernel_panic,cfi_failure_hard,hfi_*_session_get_seq_hdr`).
- **OpenSSL 4 host build fix** — `scripts/extract-cert.c` used the ENGINE API, which was removed in
  OpenSSL 4. The PKCS#11 path is now compiled out when `OPENSSL_NO_ENGINE` is defined, the same way
  `scripts/sign-file.c` already handles it. Host tool only; no effect on the device kernel.

### `device_xiaomi_platina.patch`
- `TARGET_KERNEL_CONFIG` switched to the config-fragment layout used by the 4.19 kernel branch
  (there is no monolithic `platina_defconfig`), with `platina-fix.config` appended last.

### `device_xiaomi_sdm660-common.patch`
- **H.264 encoder limited to 1080p** (`media_codecs.xml`) — the encoder declared 3840x2160, but the
  device's own `media_codecs_performance.xml` tops out at 1920x1080 and the driver rejects larger
  sessions (`msm_venc_s_fmt: session not supported`). HEVC left unchanged.
- **4K/QHD camcorder profiles removed** (`media_profiles_vendor.xml`) — 2160p, qhd, 2k, 4kdci and
  their timelapse variants were all declared as H.264, so camera apps offered sizes that could never
  be recorded.
- **Dolby HAL in the framework compatibility matrix** — the vendor branch ships
  `vendor.dolby.hardware.dms@2.0`, which the device branch's matrix did not list, failing the VINTF
  check. Added as `optional="true"`.

### `vendor_xiaomi_sdm660-common.patch`
- Removes the duplicate `libprotobuf-cpp-full-21.7` prebuilt, which is already provided by
  `hardware/lineage/compat` (`vendor_available: true`) and broke the build with
  "already defined".

### `packages_apps_Updater.patch`
- Points the OTA and changelog URLs to this repository, since `platina` is not an official
  Infinity-X device.

## Non-patch changes

- [`../manifest/local_manifest.xml`](../manifest/local_manifest.xml) — device, vendor and kernel
  repositories, plus `LineageOS/android_packages_apps_CellBroadcastReceiver` pinned to `6ea8f374`:
  Wear OS backports merged on 2026-11-10 need `CellBroadcastMetrics` constants that are not in the
  AOSP 25Q4 `CellBroadcastService`.
- Build with `LC_ALL=C`. Under a Turkish locale, Java's `toLowerCase()` turns `INT` into `ınt`
  (dotless i) and AutoValue generates uncompilable code.
- Lunch target: `infinity_platina-userdebug`.

---

## 🇹🇷 Her yama ne yapıyor

### `kernel_xiaomi_sdm660.patch`
- **CFI kapatıldı** — en son uygulanan config fragment'i olarak
  `arch/arm64/configs/vendor/xiaomi/platina-fix.config` eklendi. `CONFIG_CFI_CLANG=y` iken Venus video
  kodlayıcısının HFI arayüzündeki fonksiyon işaretçisi tip uyuşmazlığı, video kaydı başlatılınca
  ölümcül paniğe yol açıyordu (`kernel_panic,cfi_failure_hard,hfi_*_session_get_seq_hdr`).
- **OpenSSL 4 derleme düzeltmesi** — `scripts/extract-cert.c` OpenSSL 4'te kaldırılan ENGINE API'sini
  kullanıyordu. `OPENSSL_NO_ENGINE` tanımlıyken PKCS#11 yolu derlenmiyor; `scripts/sign-file.c` zaten
  böyle yapıyordu. Yalnızca derleme makinesindeki aracı etkiler, cihaz kernel'ine etkisi yok.

### `device_xiaomi_platina.patch`
- `TARGET_KERNEL_CONFIG`, 4.19 kernel dalının kullandığı fragment düzenine geçirildi (tek parça
  `platina_defconfig` yok); `platina-fix.config` en sona eklendi.

### `device_xiaomi_sdm660-common.patch`
- **H.264 kodlayıcı 1080p ile sınırlandı** (`media_codecs.xml`) — kodlayıcı 3840x2160 ilan ediyordu,
  oysa cihazın kendi `media_codecs_performance.xml` ölçümleri 1920x1080'de bitiyor ve sürücü daha büyük
  oturumları reddediyor (`msm_venc_s_fmt: session not supported`). HEVC'ye dokunulmadı.
- **4K/QHD kamera profilleri kaldırıldı** (`media_profiles_vendor.xml`) — 2160p, qhd, 2k, 4kdci ve
  timelapse varyantları H.264 olarak tanımlıydı; kamera uygulamaları hiç kaydedilemeyecek boyutlar
  sunuyordu.
- **Dolby HAL uyumluluk matrisine eklendi** — vendor dalı `vendor.dolby.hardware.dms@2.0` içeriyor,
  device dalının matrisinde karşılığı olmadığı için VINTF kontrolü başarısız oluyordu.
  `optional="true"` olarak eklendi.

### `vendor_xiaomi_sdm660-common.patch`
- `hardware/lineage/compat` tarafından zaten sağlanan (`vendor_available: true`) çift
  `libprotobuf-cpp-full-21.7` prebuilt'i kaldırıldı; "already defined" hatasıyla derlemeyi bozuyordu.

### `packages_apps_Updater.patch`
- `platina` resmi bir Infinity-X cihazı olmadığı için OTA ve changelog adresleri bu depoya yönlendirildi.

## Yama dışı değişiklikler

- [`../manifest/local_manifest.xml`](../manifest/local_manifest.xml) — device, vendor ve kernel
  depoları; ayrıca `LineageOS/android_packages_apps_CellBroadcastReceiver` `6ea8f374`'e sabitlendi:
  2026-11-10'da gelen Wear OS backport'ları, AOSP 25Q4 `CellBroadcastService`'te olmayan
  `CellBroadcastMetrics` sabitlerini istiyor.
- `LC_ALL=C` ile derleyin. Türkçe yerel ayarda Java'nın `toLowerCase()`'i `INT`'i `ınt` (noktasız ı)
  yapıyor ve AutoValue derlenemeyen kod üretiyor.
- Lunch hedefi: `infinity_platina-userdebug`.
