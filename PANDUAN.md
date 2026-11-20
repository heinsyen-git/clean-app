# Panduan Memasang Aplikasi Kebersihan Kantor di Android

Aplikasi ini berbentuk **PWA (Progressive Web App)**. Setelah dipasang, aplikasi muncul sebagai ikon di layar utama HP Android, terbuka layar penuh seperti aplikasi biasa, dan **tidak perlu akun claude.ai**. Petugas cukup login dengan username dan password aplikasi.

Data (akun, area, laporan, dan foto) disimpan di **Google Firebase** milik kantor Anda sendiri. Paket gratisnya sudah cukup untuk pemakaian kantor.

Waktu yang dibutuhkan sekitar 15–20 menit. Anda hanya perlu satu akun Google.

---

## Isi folder

| File | Fungsi |
|---|---|
| `index.html` | Aplikasinya (tampilan manager dan petugas) |
| `config.js` | Tempat menempel konfigurasi Firebase (langkah 2) |
| `manifest.json`, `sw.js`, `icons/` | Membuat aplikasi bisa dipasang di Android |
| `firestore.rules` | Aturan akses database (langkah 1) |

---

## Langkah 1: Buat database di Firebase

1. Buka **https://console.firebase.google.com** dan login dengan akun Google kantor.
2. Klik **Create a project** (Buat project). Beri nama, misalnya `kebersihan-kantor`. Google Analytics boleh dimatikan.
3. Di menu kiri, pilih **Build → Firestore Database**, lalu klik **Create database**.
   - Lokasi: pilih **asia-southeast2 (Jakarta)** agar cepat dari Indonesia.
   - Mode: pilih **Start in production mode**.
4. Setelah database jadi, buka tab **Rules**. Hapus semua isinya, lalu tempelkan isi file `firestore.rules`. Klik **Publish**.

## Langkah 2: Hubungkan aplikasi ke Firebase

1. Di Firebase, klik ikon roda gigi **⚙ → Project settings**.
2. Di bagian **Your apps**, klik ikon **`</>`** (Web). Beri nama aplikasi, misalnya `Kebersihan`. Tidak perlu mencentang Firebase Hosting. Klik **Register app**.
3. Akan muncul kode yang berisi `const firebaseConfig = { apiKey: "...", ... }`. Salin nilai-nilainya.
4. Buka file `config.js` dengan Notepad, lalu ganti setiap `ISI_...` dengan nilai dari Firebase. Hasilnya kurang lebih seperti ini:

```js
window.FIREBASE_CONFIG = {
  apiKey: "AIzaSyB...",
  authDomain: "kebersihan-kantor.firebaseapp.com",
  projectId: "kebersihan-kantor",
  storageBucket: "kebersihan-kantor.appspot.com",
  messagingSenderId: "1234567890",
  appId: "1:1234567890:web:abc123"
};
```

5. Simpan file.

## Langkah 3: Online-kan aplikasinya (gratis)

Cara paling mudah tanpa keahlian pemrograman adalah memakai **Netlify**:

1. Buka **https://app.netlify.com/signup** dan daftar gratis (bisa langsung pakai akun Google).
2. Setelah masuk, buka **https://app.netlify.com/drop**.
3. Seret (drag) **seluruh folder aplikasi** ini ke kotak yang tersedia.
4. Dalam beberapa detik Anda mendapat link seperti `https://nama-acak-123.netlify.app`.
5. Agar mudah diingat, buka **Site configuration → Change site name**, lalu ganti misalnya menjadi `kebersihan-woori`. Link berubah menjadi `https://kebersihan-woori.netlify.app`.

> Link harus diawali `https://` agar kamera HP dan fitur pasang aplikasi bisa berjalan. Netlify sudah otomatis memakai https.

## Langkah 4: Buka pertama kali

1. Buka link aplikasi di browser.
2. Muncul halaman **Siapkan aplikasi**. Tekan **Pakai data dari versi sebelumnya**.
   Data dari versi claude.ai akan dimasukkan: akun **itadmin**, **Windi (OG)**, **Heri (OB)**, dan 19 area kerja beserta checklist-nya. Password semua akun tetap sama.
3. Login sebagai manager dengan username `itadmin`.

> Laporan dan foto yang pernah dikirim di versi claude.ai tidak ikut pindah. Versi ini dimulai dengan laporan yang masih kosong.

## Langkah 5: Pasang di HP Android

Lakukan di setiap HP petugas:

1. Buka link aplikasi di **Google Chrome** (bukan dari dalam WhatsApp; salin link lalu tempel di Chrome).
2. Tekan tombol kuning **⬇ Pasang aplikasi di HP** di halaman login.
   Jika tombolnya tidak muncul, tekan menu **⋮** di pojok kanan atas Chrome, lalu pilih **Instal aplikasi** atau **Tambahkan ke layar utama**.
3. Ikon **Kebersihan** akan muncul di layar utama HP. Buka dari ikon itu.
4. Login dengan akun petugas, misalnya `heri`.
5. Saat pertama kali menekan tombol kamera, pilih **Izinkan** untuk akses kamera.

## Langkah 6 (opsional): Membuat file APK

Jika kantor ingin file **.apk** atau ingin menerbitkan aplikasi ke Google Play Store:

1. Buka **https://www.pwabuilder.com**.
2. Masukkan link aplikasi Anda (misalnya `https://kebersihan-woori.netlify.app`), lalu klik **Start**.
3. Klik **Package For Stores → Android → Generate Package**, lalu unduh hasilnya.
4. Di dalam file zip hasil unduhan ada:
   - **file .apk**: bisa dikirim ke HP petugas dan dipasang langsung. HP akan meminta izin "Instal dari sumber tidak dikenal"; izinkan untuk Chrome atau aplikasi file manager.
   - **file .aab**: untuk diunggah ke Google Play Console jika ingin terbit di Play Store (biaya pendaftaran developer Google sekali bayar sebesar USD 25).

Untuk pemakaian internal kantor, langkah 5 (pasang lewat Chrome) sudah cukup dan paling mudah diperbarui.

---

## Setelah aplikasi berjalan

**Ganti password sementara.** Minta Heri dan Windi mengganti password lewat menu **Akun → Ganti password**, terutama password sementara seperti `heri123`.

**Memperbarui aplikasi.** Jika suatu saat file `index.html` diubah, buka situs Anda di Netlify, pilih tab **Deploys**, lalu seret ulang foldernya. HP petugas otomatis memakai versi terbaru saat aplikasi dibuka dengan koneksi internet.

**Batas paket gratis Firebase (Spark).**
- Penyimpanan 1 GB. Setiap laporan dengan 2 foto berukuran sekitar 250–350 KB, jadi cukup untuk kira-kira **3.000 laporan**. Dengan 19 area per hari, ini setara sekitar 5 bulan. Setelah itu, laporan lama bisa dihapus dari Firebase Console (setelah dicetak atau diunduh), atau upgrade ke paket **Blaze** yang biayanya hanya beberapa ribu rupiah per GB per bulan.
- Kuota baca dan tulis harian (50.000 baca, 20.000 tulis) jauh di atas kebutuhan 2–3 petugas.

**Tentang keamanan.** Login username dan password di aplikasi ini memisahkan peran manager dan petugas, tetapi database Firestore-nya dapat diakses siapa saja yang mengetahui konfigurasi di `config.js`. Ini wajar untuk laporan kebersihan, tetapi **jangan menyimpan data rahasia** di aplikasi ini, dan jangan sebarkan link aplikasi di luar kantor.
