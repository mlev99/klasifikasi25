# Klasifikasi Baku Lapangan Usaha Indonesia
**Aplikasi Referensi KBLI & KBJI — oleh Saka Omni Webapps**

![Versi](https://img.shields.io/badge/versi-1.1.0-2563EB?style=flat-square)
![Tipe](https://img.shields.io/badge/tipe-Single%20File%20HTML-0EA5E9?style=flat-square)
![Lisensi Data](https://img.shields.io/badge/data-BPS%20RI-22C55E?style=flat-square)
![Offline](https://img.shields.io/badge/offline-ready-6366f1?style=flat-square)

---

## Daftar Isi

1. [Tentang Aplikasi](#tentang-aplikasi)
2. [Fitur Utama](#fitur-utama)
3. [Cara Menjalankan](#cara-menjalankan)
4. [Tutorial Penggunaan](#tutorial-penggunaan)
   - [Memuat Data (Manual)](#1-memuat-data-manual)
   - [Sinkronisasi Otomatis dari GitHub](#2-sinkronisasi-otomatis-dari-github)
   - [Tab KBLI — Jelajah Hierarki](#3-tab-kbli--jelajah-hierarki)
   - [Tab Cari Usaha](#4-tab-cari-usaha)
   - [Tab KBJI — Jabatan Industri](#5-tab-kbji--jabatan-industri)
   - [Pengaturan & Kelola Data](#6-pengaturan--kelola-data)
   - [Backup & Restore Pembelajaran](#7-backup--restore-pembelajaran)
5. [Struktur File JSON Data](#struktur-file-json-data)
6. [Struktur Kode Aplikasi](#struktur-kode-aplikasi)
7. [Cara Deploy ke GitHub Pages](#cara-deploy-ke-github-pages)
8. [Format Repositori Data (klasifikasi25)](#format-repositori-data-klasifikasi25)
9. [Pertanyaan Umum (FAQ)](#pertanyaan-umum-faq)
10. [Versi & Changelog](#versi--changelog)
11. [Lisensi & Kredit](#lisensi--kredit)

---

## Tentang Aplikasi

Aplikasi ini adalah **alat referensi berbasis web satu file** (`index.html`) untuk mencari dan menjelajahi:

- **KBLI** — Klasifikasi Baku Lapangan Usaha Indonesia (edisi 2025)
- **KBJI** — Klasifikasi Baku Jenis Pekerjaan Indonesia (edisi 2014)

Aplikasi berjalan sepenuhnya di sisi klien (client-side), tanpa server backend milik pengembang. Data tersimpan di `localStorage` browser sehingga tersedia secara **offline** setelah pertama kali dimuat.

> **Catatan:** Aplikasi ini bukan produk resmi Badan Pusat Statistik (BPS). Untuk keperluan resmi, selalu verifikasi dengan dokumen resmi BPS.

---

## Fitur Utama

| Fitur | Keterangan |
|---|---|
| 🌐 **Auto-Sinkronisasi GitHub** | Deteksi & unduh otomatis file JSON terbaru dari repo GitHub saat online |
| 🔍 **Pencarian Hierarki** | Jelajah KBLI/KBJI dalam struktur pohon (Kategori → Golongan Pokok → Golongan → Subgolongan → Kelompok) |
| 💡 **Cari Usaha (Bahasa Sehari-hari)** | Ketik nama usaha dengan kata biasa, sistem mencocokkan ke kode KBLI dengan skor relevansi |
| 🧠 **Scoring Engine dengan Pembelajaran** | Hasil pencarian semakin akurat berkat feedback 👍/👎 yang disimpan lokal |
| 🌙 **Dark/Light Mode** | Tampilan gelap dan terang yang dapat diubah kapan saja |
| 📱 **Responsif Mobile** | Optimal di perangkat mobile dan desktop |
| 🔒 **Privasi Penuh** | Tidak ada tracking, tidak ada cookie iklan, tidak ada backend |
| 💾 **Offline Ready** | Data tersimpan di localStorage, tetap bisa digunakan tanpa internet |
| 📥 **Drag & Drop Upload** | Seret file JSON langsung ke zona unggah |

---

## Cara Menjalankan

Aplikasi ini adalah **satu file HTML tunggal** — tidak perlu instalasi, tidak perlu Node.js, tidak perlu build tool.

### Opsi A — Buka Langsung di Browser

1. Unduh file `index.html` dari repositori ini.
2. Klik dua kali file tersebut, atau buka via browser:
   - **Chrome/Edge**: `Ctrl+O` → pilih `index.html`
   - **Firefox**: `Ctrl+O` → pilih `index.html`
3. Aplikasi langsung berjalan.

> ⚠️ Beberapa fitur (seperti auto-sync GitHub) memerlukan koneksi internet saat pertama kali digunakan.

### Opsi B — Deploy ke GitHub Pages (Direkomendasikan)

Lihat panduan lengkap di bagian [Cara Deploy ke GitHub Pages](#cara-deploy-ke-github-pages).

### Opsi C — Jalankan via Web Server Lokal

Jika Anda memiliki Python atau Node.js:

```bash
# Python 3
python -m http.server 8080

# Node.js (npx)
npx serve .

# Kemudian buka browser di:
# http://localhost:8080
```

---

## Tutorial Penggunaan

### 1. Memuat Data (Manual)

Saat pertama kali membuka aplikasi, belum ada data tersimpan. Anda bisa memuat data dengan dua cara: **upload manual** atau **auto-sinkronisasi** (lihat bagian selanjutnya).

#### Upload Manual File KBLI

1. Buka tab **Pengaturan** (ikon ⚙️ di navigation bar).
2. Gulir ke kartu **"Unggah File KBLI"**.
3. Klik zona unggah atau **seret file** `KBLI_2025_lengkap.json` ke dalamnya.
4. Tunggu beberapa detik hingga muncul notifikasi: *"Data KBLI berhasil dimuat ✓"*
5. Tab **KBLI** dan **Cari Usaha** sekarang siap digunakan.

#### Upload Manual File KBJI

1. Masih di tab **Pengaturan**, gulir ke kartu **"Unggah File KBJI"**.
2. Klik zona unggah atau seret file `KBJI_2014_hierarki.json`.
3. Tunggu notifikasi: *"Data KBJI 2014 berhasil dimuat ✓"*

> **Format yang diterima:**
> - KBLI: file JSON dengan kunci root `"kategori"` (array hierarki)
> - KBJI: file JSON dengan kunci root `"golongan_pokok"` (array hierarki)

---

### 2. Sinkronisasi Otomatis dari GitHub

Mulai **versi 1.1.0**, aplikasi dapat mengunduh data secara otomatis dari repositori GitHub [`mlev99/klasifikasi25`](https://github.com/mlev99/klasifikasi25) tanpa perlu upload manual.

#### Cara Kerja

```
Saat Online
    │
    ▼
Cek SHA file di api.github.com (setiap 6 jam)
    │
    ├── SHA sama → Data sudah terbaru ✓ (tidak ada aksi)
    │
    └── SHA berbeda → Tampilkan notifikasi pembaruan
            │
            └── Pengguna klik "Unduh Sekarang"
                    │
                    └── Unduh via jsDelivr CDN → Proses & simpan ke localStorage
```

#### Indikator Status (Topbar)

| Ikon | Warna | Arti |
|---|---|---|
| ☁️✓ | Abu-abu | Data sudah terbaru |
| ☁️⬇ berkedip | Hijau teal | **Pembaruan tersedia** — klik untuk unduh |
| ☁️⬇ | Biru | Sedang mengunduh |
| (tersembunyi) | — | Offline / tidak terdeteksi |

#### Langkah Sinkronisasi Manual

1. Buka tab **Pengaturan**.
2. Gulir ke kartu **"Sinkronisasi Data dari GitHub"**.
3. Klik tombol **"Cek Sekarang"**.
4. Jika ada pembaruan, tombol **"Unduh Semua"** akan muncul.
5. Klik **"Unduh Semua"** — progress bar akan tampil selama proses berlangsung.
6. Setelah selesai, data langsung tersedia di seluruh tab.

#### Banner Notifikasi

Saat pembaruan terdeteksi, sebuah **banner notifikasi** muncul di bagian atas layar:

```
┌─────────────────────────────────────────────────────────────┐
│ ☁  Pembaruan Data Tersedia — KBLI 2025 versi baru ditemukan │
│     di GitHub.                    [Unduh Sekarang]  [×]     │
└─────────────────────────────────────────────────────────────┘
```

- Klik **Unduh Sekarang** → langsung unduh
- Klik **×** → tutup banner (dapat diunduh nanti dari Pengaturan)

> **Catatan teknis:** Auto-sync menggunakan `api.github.com` untuk cek SHA dan `cdn.jsdelivr.net` untuk unduh file. Tidak ada data pribadi Anda yang dikirimkan.

---

### 3. Tab KBLI — Jelajah Hierarki

Tab **KBLI** (Lapangan Usaha) menampilkan seluruh hierarki KBLI dalam bentuk **accordion tree** yang dapat dibuka-tutup.

#### Struktur Hierarki KBLI

```
Kategori (1 huruf)          contoh: A — Pertanian, Kehutanan dan Perikanan
  └── Golongan Pokok (2 digit)   contoh: 01 — Pertanian Tanaman, Peternakan
        └── Golongan (3 digit)        contoh: 011 — Pertanian Tanaman Semusim
              └── Subgolongan (4 digit)    contoh: 0111 — Pertanian Tanaman Serealia
                    └── Kelompok (5 digit)      contoh: 01111 — Pertanian Tanaman Padi
```

#### Cara Menjelajah

1. **Klik baris mana saja** untuk membuka/menutup cabang.
2. Ikon **›** menunjukkan cabang bisa dibuka; akan berputar 90° saat terbuka.
3. Di dalam setiap Kelompok (5 digit), tersedia **uraian lengkap** — mencakup, tidak mencakup, dan referensi silang.
4. **Referensi silang** (contoh: "lihat 01120") bisa diklik langsung untuk melompat ke kode tersebut.

#### Pencarian di Tab KBLI

1. Ketik di kolom **"Cari kode atau judul KBLI…"**
2. Hasil langsung difilter secara real-time.
3. Kata yang cocok disorot dengan **background kuning**.
4. Jumlah hasil ditampilkan di bawah kolom pencarian.

#### Filter Kategori (Chips)

Di bawah kolom pencarian terdapat **chip filter** per huruf kategori (A, B, C, …). Klik chip untuk menampilkan satu kategori saja.

---

### 4. Tab Cari Usaha

Tab **Cari Usaha** adalah fitur utama untuk pengguna yang tidak hafal kode KBLI. Cukup ketik nama usaha dengan **bahasa sehari-hari**.

#### Cara Menggunakan

1. Pindah ke tab **Cari Usaha**.
2. Ketik di kolom pencarian, contoh:
   - `jual bensin`
   - `bengkel motor`
   - `warung makan padang`
   - `budidaya udang vaname`
   - `klinik kecantikan`
3. Hasil muncul otomatis dengan **skor relevansi** (%).

#### Memahami Kartu Hasil

```
┌────────────────────────────────────────────────────┐
│  [95%]   47732                                     │
│  HIGH    Perdagangan Eceran Bahan Bakar Kendaraan  │
│          ● kelompok                                │
│  A › 47 › 477 › 4773 › 47732                      │
│  Mengapa cocok: [bensin] [bahan bakar]             │
│  [Buka di KBLI]  [Salin Kode]  [👍]  [👎]        │
└────────────────────────────────────────────────────┘
```

| Elemen | Keterangan |
|---|---|
| **Skor %** | Relevansi hasil (HIGH ≥ 60%, MED 35–59%, LOW < 35%) |
| **Kode & Judul** | Kode KBLI dan nama resmi |
| **Breadcrumb** | Jalur hierarki lengkap |
| **Mengapa cocok** | Kata kunci yang memicu kecocokan |
| **Buka di KBLI** | Melompat ke posisi kode dalam tab KBLI |
| **Salin Kode** | Menyalin kode ke clipboard |
| **👍 / 👎** | Feedback untuk melatih sistem |

#### Tips Pencarian yang Efektif

| Tujuan | Kata Kunci yang Disarankan |
|---|---|
| Usaha dagang/toko | Awali dengan: `jual`, `toko`, `warung`, `agen`, `sewa` |
| Usaha jasa | Awali dengan: `jasa`, `klinik`, `servis`, `cukur`, `salon` |
| Pertanian/peternakan | Gunakan: `budidaya`, `ternak`, `peternakan`, `pembudidayaan` |
| Industri/pabrik | Gunakan: `produksi`, `manufaktur`, `pengolahan`, `pabrik` |

#### Filter Skala Usaha

- **Semua Skala** — tampilkan semua hasil (default)
- **UMKM** — prioritaskan hasil berskala usaha mikro/kecil/menengah
- **Usaha Besar** — prioritaskan hasil berskala industri besar

Filter ini **mengubah urutan** hasil, bukan menghapus opsi lain.

#### Sistem Pembelajaran (Scoring Engine)

Setiap kali Anda menekan **👍** (relevan) atau **👎** (tidak relevan):
- Skor kode tersebut untuk query serupa naik atau turun.
- Data pembelajaran tersimpan di `localStorage`.
- Semakin banyak feedback, semakin akurat hasil untuk query serupa di masa depan.

---

### 5. Tab KBJI — Jabatan Industri

Tab **KBJI** (Jabatan Industri) menampilkan Klasifikasi Baku Jenis Pekerjaan Indonesia edisi 2014.

#### Struktur Hierarki KBJI

```
Golongan Pokok (1 digit)     contoh: 1 — Manajer
  └── Golongan (2 digit)          contoh: 11 — Kepala Eksekutif, Pejabat Senior
        └── Subgolongan (3 digit)      contoh: 111 — Legislator dan Pejabat Senior
              └── Kelompok (4 digit)        contoh: 1111 — Legislator
```

#### Cara Menjelajah

Sama seperti KBLI — klik baris untuk membuka cabang, gunakan kolom pencarian untuk filter.

---

### 6. Pengaturan & Kelola Data

Tab **Pengaturan** (ikon ⚙️) adalah pusat kontrol aplikasi.

#### Kartu Informasi Aplikasi

Menampilkan: nama aplikasi, pengembang, versi kode (`1.1.0`), versi KBLI yang aktif, versi KBJI yang aktif, serta tautan ke Syarat & Ketentuan dan Kebijakan Privasi.

#### Kartu Status Data

Menampilkan status file yang sedang aktif:
- 🟢 **Dot hijau** = data sudah dimuat
- ⚪ **Dot abu** = belum ada data
- Tombol **Hapus** untuk menghapus data satu per satu dari cache

#### Kartu Sinkronisasi GitHub

Lihat [Tutorial Sinkronisasi Otomatis](#2-sinkronisasi-otomatis-dari-github).

#### Kartu Kelola Data

Tombol **"Hapus Semua Data Cache"** — menghapus seluruh data KBLI, KBJI, dan metadata dari `localStorage`. Data pembelajaran (scoring) **tidak** ikut terhapus.

---

### 7. Backup & Restore Pembelajaran

Data pembelajaran dari feedback 👍/👎 dapat di-backup dan di-restore antar perangkat.

#### Backup (Export)

1. Di tab **Pengaturan**, gulir ke kartu **"Backup & Restore Hasil Pembelajaran Pencarian"**.
2. Lihat **Total Query Terpelajari** — jumlah kode yang sudah mendapat feedback.
3. Klik **"Backup (Export)"**.
4. File `kbli_learn_db.json` akan otomatis terunduh.

#### Restore (Import)

1. Klik **"Restore (Import)"**.
2. Pilih file `kbli_learn_db.json` hasil backup sebelumnya.
3. Data pembelajaran langsung dimuat dan digabungkan.

> Gunakan fitur ini untuk memindahkan data pembelajaran ke browser/perangkat lain, atau untuk mencadangkan sebelum menghapus cache browser.

---

## Struktur File JSON Data

### Format KBLI (`KBLI_2025_lengkap.json`)

```json
{
  "kategori": [
    {
      "kode": "A",
      "judul": "Pertanian, Kehutanan dan Perikanan",
      "uraian": "Kategori ini mencakup...",
      "golongan_pokok": [
        {
          "kode": "01",
          "judul": "Pertanian Tanaman, Peternakan, ...",
          "uraian": "...",
          "golongan": [
            {
              "kode": "011",
              "judul": "Pertanian Tanaman Semusim",
              "uraian": "...",
              "subgolongan": [
                {
                  "kode": "0111",
                  "judul": "Pertanian Tanaman Serealia ...",
                  "uraian": "...",
                  "kelompok": [
                    {
                      "kode": "01111",
                      "judul": "Pertanian Tanaman Padi",
                      "uraian": "Kelompok ini mencakup..."
                    }
                  ]
                }
              ]
            }
          ]
        }
      ]
    }
  ]
}
```

### Format KBJI (`KBJI_2014_hierarki.json`)

```json
{
  "versi": "2014",
  "golongan_pokok": [
    {
      "kode": "1",
      "judul": "Manajer",
      "golongan": [
        {
          "kode": "11",
          "judul": "Kepala Eksekutif, Pejabat Senior ...",
          "subgolongan": [
            {
              "kode": "111",
              "judul": "Legislator dan Pejabat Senior ...",
              "kelompok": [
                {
                  "kode": "1111",
                  "judul": "Legislator"
                }
              ]
            }
          ]
        }
      ]
    }
  ]
}
```

---

## Struktur Kode Aplikasi

Seluruh aplikasi ada dalam **satu file** `index.html`, dengan struktur sebagai berikut:

```
index.html
├── <head>
│   ├── CDN Bootstrap 5.3.3 CSS
│   ├── CDN Bootstrap Icons 1.11.3
│   └── <style> — Semua CSS custom (CSS Variables, komponen, dark mode)
│
├── <body>
│   ├── .topbar              — Navbar sticky (brand, sync btn, theme toggle)
│   ├── .tab-nav             — Tab navigasi (KBLI / Cari / KBJI / Pengaturan)
│   ├── .main-content
│   │   ├── #page-kbli       — Halaman KBLI (search + accordion tree)
│   │   ├── #page-cari       — Halaman Cari Usaha (NLP search + cards)
│   │   ├── #page-kbji       — Halaman KBJI (search + accordion tree)
│   │   └── #page-settings   — Halaman Pengaturan (info, sync, upload, backup)
│   ├── Modals
│   │   ├── #tipsModalOverlay    — Tips pencarian
│   │   ├── #legalModalTos       — Syarat & Ketentuan
│   │   └── #legalModalPrivacy   — Kebijakan Privasi
│   ├── #toastContainer      — Notifikasi toast
│   ├── #syncBanner          — Banner notifikasi pembaruan GitHub
│   └── #syncProgressOverlay — Overlay progress download
│
└── <script>
    ├── App {}               — State global (kbli, kbji, meta, flat index, smartIdx)
    ├── initTheme()          — Dark/light mode
    ├── switchTab()          — Navigasi tab
    ├── loadFile()           — Upload JSON manual
    ├── processKbli()        — Parse & index KBLI
    ├── processKbji()        — Parse & index KBJI
    ├── buildSmartIndex()    — Bangun indeks extended untuk Cari Usaha
    ├── scoreKbli()          — Scoring engine (token matching + learning)
    ├── onCariInput()        — Handler pencarian Cari Usaha
    ├── renderKbli()         — Render accordion tree KBLI
    ├── renderKbji()         — Render accordion tree KBJI
    ├── onKbliSearch()       — Handler pencarian KBLI
    ├── onKbjiSearch()       — Handler pencarian KBJI
    ├── githubSync {}        — ★ Modul auto-sinkronisasi GitHub (v1.1.0)
    │   ├── checkRemote()    — Cek SHA file di GitHub API
    │   ├── downloadFile()   — Unduh file via jsDelivr CDN
    │   ├── downloadAll()    — Unduh semua file yang ada pembaruan
    │   ├── autoCheck()      — Cek otomatis (interval 6 jam)
    │   └── dismissBanner()  — Tutup banner notifikasi
    ├── updateSettings()     — Sinkron UI panel pengaturan
    ├── clearData()          — Hapus data satu type
    ├── clearAllData()       — Hapus semua cache
    ├── saveCache()          — Simpan ke localStorage (chunked untuk data besar)
    ├── loadCache()          — Muat dari localStorage (de-chunk)
    ├── exportLearnDB()      — Backup data pembelajaran
    ├── importLearnDB()      — Restore data pembelajaran
    └── boot()               — Inisialisasi: muat cache + trigger autoCheck
```

### Dependensi Eksternal

| Library | Versi | Sumber | Kegunaan |
|---|---|---|---|
| Bootstrap CSS | 5.3.3 | cdnjs.cloudflare.com | Layout & komponen dasar |
| Bootstrap Icons | 1.11.3 | cdnjs.cloudflare.com | Ikon |
| Bootstrap JS | 5.3.3 | cdnjs.cloudflare.com | Komponen interaktif |
| GitHub API | v3 | api.github.com | Cek metadata file (SHA) |
| jsDelivr CDN | — | cdn.jsdelivr.net | Unduh file JSON dari GitHub |

> Semua dependensi diakses via CDN — tidak ada `node_modules`, tidak ada build step.

---

## Cara Deploy ke GitHub Pages

### Langkah 1 — Fork atau Buat Repositori Baru

```bash
# Buat repositori baru di GitHub (misalnya: username/kbli-app)
# Lalu clone ke lokal:
git clone https://github.com/username/kbli-app.git
cd kbli-app
```

### Langkah 2 — Tambahkan File

```bash
# Copy file aplikasi
cp /path/to/index.html .

# Buat README (file ini)
cp /path/to/README.md .

git add index.html README.md
git commit -m "feat: initial deploy KBLI app v1.1.0"
git push origin main
```

### Langkah 3 — Aktifkan GitHub Pages

1. Buka repositori di GitHub.
2. Klik **Settings** → **Pages** (menu kiri).
3. Di bagian **Source**, pilih:
   - Branch: `main`
   - Folder: `/ (root)`
4. Klik **Save**.
5. Tunggu beberapa menit, lalu akses di:
   `https://username.github.io/kbli-app/`

### Langkah 4 — Setup Repositori Data (Opsional)

Agar fitur auto-sync bekerja, file JSON data harus tersedia di repositori terpisah `mlev99/klasifikasi25` dengan nama file yang tepat:

```
klasifikasi25/
├── KBLI_2025_lengkap.json
└── KBJI_2014_hierarki.json
```

> Jika Anda ingin menggunakan repositori data sendiri, ubah konstanta `REPO` di dalam blok `githubSync` di file `index.html`:
> ```javascript
> const REPO = 'username-anda/nama-repo-data';
> ```

---

## Format Repositori Data (klasifikasi25)

Repositori [`mlev99/klasifikasi25`](https://github.com/mlev99/klasifikasi25) menyimpan file data JSON yang dikonsumsi oleh fitur auto-sync.

### Aturan Penamaan File

| File | Nama yang Diharapkan |
|---|---|
| Data KBLI 2025 | `KBLI_2025_lengkap.json` |
| Data KBJI 2014 | `KBJI_2014_hierarki.json` |

### Cara Update Data di Repositori

```bash
# Clone repositori data
git clone https://github.com/mlev99/klasifikasi25.git
cd klasifikasi25

# Ganti/update file JSON
cp /path/to/KBLI_2025_lengkap_baru.json KBLI_2025_lengkap.json

# Commit dan push
git add KBLI_2025_lengkap.json
git commit -m "update: KBLI data [tanggal]"
git push origin main
```

Setelah push, aplikasi yang sudah terdeploy akan mendeteksi pembaruan (via perubahan SHA commit) pada pengecekan berikutnya (maksimum 6 jam, atau klik "Cek Sekarang" secara manual).

---

## Pertanyaan Umum (FAQ)

**Q: Apakah data saya dikirim ke internet?**
> Tidak. Data KBLI/KBJI yang Anda unggah dan data pembelajaran Anda tersimpan sepenuhnya di `localStorage` browser Anda. Satu-satunya request jaringan yang dibuat aplikasi adalah saat mengecek atau mengunduh data dari GitHub (repositori publik).

**Q: Kenapa tombol "Unduh Semua" tidak muncul?**
> Tombol ini hanya muncul jika terdeteksi perbedaan SHA antara file lokal dan file di GitHub. Coba klik "Cek Sekarang" terlebih dahulu.

**Q: Apakah bisa digunakan tanpa internet?**
> Ya. Setelah data dimuat sekali (baik via upload manual maupun auto-sync), seluruh fitur pencarian dan jelajah tersedia secara offline. Hanya fitur auto-sync yang memerlukan internet.

**Q: Kenapa hasil "Cari Usaha" kurang akurat?**
> Coba tambahkan kata konteks: `jual`, `toko`, `jasa`, `budidaya`, dll. Juga berikan feedback 👍/👎 agar sistem belajar dari preferensi Anda.

**Q: Data pembelajaran hilang setelah hapus cache browser?**
> Gunakan fitur **Backup (Export)** sebelum menghapus cache browser. File backup bisa di-restore kapan saja.

**Q: Apakah bisa digunakan untuk keperluan resmi/perizinan?**
> Sebaiknya gunakan sebagai referensi awal saja. Untuk keperluan resmi (OSS, perizinan usaha, sensus), verifikasi selalu dengan dokumen resmi BPS.

**Q: Bagaimana cara mengubah sumber repo data untuk auto-sync?**
> Edit baris `const REPO = 'mlev99/klasifikasi25';` di dalam blok `githubSync` di file `index.html`, ganti dengan `'username/nama-repo'` milik Anda.

---

## Versi & Changelog

### v1.1.0 — 29 Juni 2026
- ✨ **Fitur baru:** Auto-sinkronisasi data dari GitHub (`mlev99/klasifikasi25`)
- ✨ **Fitur baru:** Banner notifikasi pembaruan data
- ✨ **Fitur baru:** Indikator status sinkronisasi di topbar
- ✨ **Fitur baru:** Kartu "Sinkronisasi Data dari GitHub" di halaman Pengaturan
- ✨ **Fitur baru:** Progress overlay saat mengunduh
- 📝 **Update:** Versi aplikasi → `1.1.0`
- 📝 **Update:** Syarat & Ketentuan — ditambah Pasal 4b (auto-sync & pihak ketiga)
- 📝 **Update:** Kebijakan Privasi — ditambah Pasal 2 (koneksi jaringan GitHub/jsDelivr)

### v1.0.0 — Rilis Awal
- 🚀 Rilis pertama aplikasi referensi KBLI & KBJI
- Jelajah hierarki KBLI 5-level dan KBJI 4-level
- Pencarian bahasa sehari-hari (Cari Usaha) dengan scoring engine
- Sistem pembelajaran 👍/👎
- Dark/Light mode
- Upload manual JSON via file picker & drag-drop
- Backup & restore data pembelajaran
- Syarat & Ketentuan dan Kebijakan Privasi

---

## Lisensi & Kredit

### Aplikasi

**Klasifikasi Baku Lapangan Usaha Indonesia**
Dikembangkan oleh **Saka Omni Webapps**

Kode aplikasi (kerangka kerja, tampilan, logika) adalah hak cipta Saka Omni Webapps. Penggunaan bebas untuk keperluan non-komersial dengan tetap mencantumkan atribusi.

### Data

Data klasifikasi **KBLI** dan **KBJI** adalah hak milik dan hak cipta **Badan Pusat Statistik Republik Indonesia (BPS RI)** sebagai penerbit resmi. Referensi:
- [BPS — Klasifikasi Baku Lapangan Usaha Indonesia 2020](https://www.bps.go.id/id/publication/2020/03/23/1f301a47d7e1bc48a93b8c10/klasifikasi-baku-lapangan-usaha-indonesia-2020.html)
- [BPS — Klasifikasi Baku Jenis Pekerjaan Indonesia 2014](https://www.bps.go.id/id/publication/2015/11/30/5ed37e6ed2b0e0f1d4b37a47/klasifikasi-baku-jenis-pekerjaan-indonesia.html)

### Library Pihak Ketiga

- [Bootstrap](https://getbootstrap.com/) — MIT License
- [Bootstrap Icons](https://icons.getbootstrap.com/) — MIT License
- [jsDelivr](https://www.jsdelivr.com/) — digunakan sebagai CDN untuk distribusi data publik

---

> Dibuat dengan ❤️ oleh Saka Omni Webapps · Versi 1.1.0 · 29 Juni 2026
