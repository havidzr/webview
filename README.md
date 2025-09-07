# Shafwah WebView (AAB via GitHub Actions)

Repo starter Android (WebView) + GitHub Actions untuk menghasilkan **.aab** siap unggah ke Play Console.

## Cara Pakai (Singkat)
1. **Ganti URL** di `app/src/main/java/com/shafwah/webview/MainActivity.kt` (variabel `START_URL`).
2. **Push** seluruh folder ini ke GitHub.
3. **Tambahkan GitHub Secrets** pada repo:
   - `ANDROID_KEYSTORE_BASE64` — isi base64 dari file keystore (upload key).
   - `SIGNING_KEY_ALIAS` — alias key (mis. `upload`).
   - `SIGNING_KEY_PASSWORD` — password key.
   - `SIGNING_STORE_PASSWORD` — password keystore.
4. Buka tab **Actions** → jalankan workflow **Android AAB Build** (atau push ke `main`).  
   Hasil `.aab` ada di **Artifacts**.

> Untuk membuat *upload key* (Linux/macOS):
>
> ```bash
> keytool -genkeypair -v -keystore upload.keystore -alias upload \
>   -keyalg RSA -keysize 2048 -validity 10000
> base64 upload.keystore > upload.keystore.b64
> # salin isi file .b64 ke Secret ANDROID_KEYSTORE_BASE64
> ```
>
> Windows (PowerShell):
> ```powershell
> keytool -genkeypair -v -keystore upload.keystore -alias upload -keyalg RSA -keysize 2048 -validity 10000
> certutil -encode upload.keystore upload.keystore.b64
> # buka .b64, salin isinya ke Secret ANDROID_KEYSTORE_BASE64 (tanpa header/footer)
> ```

## Rangkaian Build di CI
- Java 17 (Temurin)
- Android SDK (platform 34, build-tools 34.0.0)
- Gradle 8.7 (via `gradle/gradle-build-action`)
- Task: `clean bundleRelease`
- Artifact: `app/build/outputs/bundle/release/*.aab`

## Catatan
- Target SDK 34, min SDK 21.
- Signing **release** menggunakan keystore yang didekode dari Secret ke `app/upload.keystore`.
- Tidak menyertakan Gradle Wrapper; CI menjalankan Gradle 8.7 langsung.
- Jika butuh features khusus (kamera, file upload, WA intent), tambahkan izin & handler sesuai kebutuhan.

---

Lisensi: MIT