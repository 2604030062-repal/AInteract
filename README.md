# AInteract (Android)

Aplikasi Android untuk membaca, bertanya, berlatih, dan mengobrol, dibuat dari desain Figma **Prototype of Paper**.
Dibangun dengan Kotlin + Jetpack Compose. Paket: `id.ainteract.app`, minSdk 26 (Android 8.0).

## Cara mendapatkan file .apk

**A. Android Studio (paling mudah)**
1. Buka folder ini di Android Studio (Ladybug atau lebih baru), tunggu Gradle sync selesai.
2. Menu **Build > Build App Bundle(s) / APK(s) > Build APK(s)**.
3. File ada di `app/build/outputs/apk/debug/app-debug.apk`. Kirim ke HP lalu install.

**B. Lewat command line** (butuh JDK 17 dan Android SDK)
```
./gradlew assembleDebug
```

**C. Lewat GitHub (tanpa install apa pun)**
1. Upload folder ini ke repositori GitHub baru (branch `main`).
2. Buka tab **Actions > Build AInteract APK > Run workflow**.
3. Setelah hijau, unduh artifact **AInteract-debug-apk**, di dalamnya ada `app-debug.apk`.

APK debug sudah ditandatangani dengan kunci debug, jadi langsung bisa di-install
(aktifkan "Install from unknown sources" di HP).

## Layar dari desain

| Layar di Figma | Di aplikasi |
|---|---|
| Loading Screen Apps - 1 | `LoadingScreen` (logo AInteract) |
| Main Screen UI -2 / (if scrolled) -3 | Tab **Home**: header tetap, rak *Fiction Books* dan *Nonfiction Books*, tombol *Read Now* |
| Reading Screen - 4 / (FT) | `ReadingScreen`: teks bacaan + logo AInteract mengambang |
| Reading Screen (if button is pressed) - 5/6 | Logo ditekan: pil *Make Summary*, *Make Quiz*, *Make Infografis*, *Ask AInteract* |
| Reading Screen (UI AInteract) - 6/7 | Panel biru *AInteract* dengan kolom "Type here to ask" |
| Create an Summary / quiz / Infographic | Panel biru *Make Summary*, *Make Quiz* (pilihan A sampai E), *Make Infografis* (share + download) |

Bottom bar: **Home** (main screen), **Chat** (chat sosial antar akun AInteract), **Add** (logo tengah, tambah buku), **Settings**, **Profile** (akun).

## Fitur AI (versi offline)
Sesuai catatan di desain ("use AI Offline Version"), semua fitur AI berjalan di HP tanpa internet:
ringkasan (peringkat kalimat berdasarkan frekuensi kata), kuis isian dengan 5 pilihan A sampai E,
infografis (digambar dengan Canvas), dan tanya-jawab (pencarian kalimat paling relevan di teks buku).
Kodenya ada di `app/src/main/java/id/ainteract/app/ai/`.

## Chat sosial dan akun
Chat butuh akun AInteract (tab **Profile** > Create account). Saat ini akun dan pesan disimpan **di HP itu sendiri**
(SQLite, password di-hash PBKDF2), jadi orang yang punya akun di HP yang sama bisa saling chat.
Ada akun demo resmi `ainteract` yang membalas otomatis.

Untuk chat antar HP yang berbeda dibutuhkan server. Semua kebutuhan chat sudah dibungkus di interface
`SocialBackend` (`data/SocialBackend.kt`). Buat implementasi baru (Firebase, Supabase, atau API sendiri)
lalu ganti satu baris di `AppViewModel.kt`:
```kotlin
val social: SocialBackend = LocalSocialBackend(app)   // ganti dengan implementasi server
```

## Buku
Tiga buku dari desain sudah ada. Teks bacaannya adalah ringkasan orisinal untuk prototipe ini
(`data/SeedBooks.kt`); deskripsi kartu diambil dari desain. Tambahkan buku sendiri lewat tab **Add**
(tempel teks atau impor file `.txt`).
