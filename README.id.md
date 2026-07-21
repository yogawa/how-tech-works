# How Tech Works

🌐 [English](README.md) | **Bahasa Indonesia**

**🔗 Demo langsung: [yogawa.github.io/how-tech-works](https://yogawa.github.io/how-tech-works/)**

Kumpulan visualisasi interaktif yang menjelaskan **cara kerja teknologi** — jaringan, sistem, keamanan, dan topik lainnya — dengan bahasa sederhana, langkah demi langkah, supaya pemula bisa benar-benar mengikutinya.

Setiap topik adalah halaman interaktif dengan dukungan **multi-bahasa** bawaan: teks terjemahan disimpan di file JSON terpisah dari kode, sehingga kontributor bisa menambah bahasa baru tanpa menyentuh HTML/JavaScript sama sekali.

## Menjalankannya

Proyek ini memuat teks terjemahan lewat `fetch()`, sehingga **wajib dijalankan lewat local server**, bukan dibuka langsung sebagai file (`file://`) — browser memblokir permintaan `fetch` ke file JSON lokal di bawah protokol `file://` karena pembatasan CORS.

Pilih salah satu:

- **VS Code**: pasang ekstensi [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer), klik kanan `index.html` → "Open with Live Server".
- **Python**: `python -m http.server 8000`, lalu buka `http://localhost:8000`.
- **Node.js**: `npx serve .`, lalu buka URL yang ditampilkan.

Untuk versi yang di-hosting, aktifkan **GitHub Pages** di pengaturan repository (Settings → Pages → Deploy from branch `main`, folder `/`), lalu buka `https://<username>.github.io/<nama-repo>/`. Deployment langsung dari proyek ini sendiri ada di [yogawa.github.io/how-tech-works](https://yogawa.github.io/how-tech-works/).

## Struktur proyek

```
.
├── index.html                    # Halaman utama: sidebar topik + area konten (iframe)
├── README.md
├── README.id.md                  # Terjemahan Bahasa Indonesia dari file ini
├── i18n/                         # Terjemahan untuk chrome UI index.html (sidebar, empty state, dsb.)
│   ├── id.json
│   └── en.json
└── topics/
    ├── tcp-ip-layers.html        # Perjalanan sebuah paket lewat TCP/IP (encapsulation/decapsulation)
    ├── dns.html                  # Bagaimana nama domain diterjemahkan menjadi alamat IP
    ├── asymmetric-encryption.html # Pasangan kunci publik/privat — enkripsi & tanda tangan digital
    ├── blockchain.html           # Blok yang saling terhubung lewat hash dan konsensus jaringan
    ├── multithreading.html       # Thread konkuren, race condition, dan mutex lock
    ├── docker-vs-vm.html         # Container vs. virtual machine tradisional
    ├── git.html                  # Git: 5 skenario — alur commit, branching, konflik, reset vs revert, rebase vs merge
    ├── https-tls.html            # HTTPS/TLS: 4 skenario — handshake, rantai kepercayaan sertifikat, TLS 1.2 vs 1.3, serangan MITM
    ├── event-driven-architecture.html # EDA: 4 skenario — pub/sub, event queue, koreografi vs orkestrasi, ketahanan sistem
    ├── llm.html                  # LLM: tokenisasi, embedding, self-attention, dan generasi token
    ├── aot-vs-jit.html           # AOT vs JIT: alur kompilasi, startup/warm-up, optimisasi adaptif
    └── i18n/                     # Terjemahan konten per topik, satu file per bahasa
        ├── tcp-ip-layers.id.json / .en.json
        ├── dns.id.json / .en.json
        ├── asymmetric-encryption.id.json / .en.json
        ├── blockchain.id.json / .en.json
        ├── multithreading.id.json / .en.json
        ├── docker-vs-vm.id.json / .en.json
        ├── git.id.json / .en.json
        ├── https-tls.id.json / .en.json
        ├── event-driven-architecture.id.json / .en.json
        ├── llm.id.json / .en.json
        └── aot-vs-jit.id.json / .en.json
```

- `index.html` berisi sidebar untuk memilih topik. Klik salah satu akan memuat halaman HTML yang sesuai ke area kanan lewat `<iframe>`, dengan bahasa aktif diteruskan lewat query string (`?lang=en`).
- Setiap file di `topics/` adalah halaman HTML **mandiri (self-contained)** — CSS dan JavaScript-nya ada di dalam satu file itu saja, terpisah dari `index.html`. Masing-masing juga bisa dibuka berdiri sendiri (lewat local server), lengkap dengan toggle bahasanya sendiri.
- Semua teks yang tampil ke pengguna (judul, deskripsi, label tombol, isi tabel, dll.) berasal dari file JSON di `i18n/`, **bukan** ditulis langsung (hardcode) di HTML/JS. Itulah yang membuatnya bisa diterjemahkan tanpa menyentuh kode.

## Cara kerja setup multi-bahasa

- Bahasa yang dipilih disimpan di `localStorage` browser, sehingga tetap tersimpan meski halaman dimuat ulang.
- `index.html` memuat `i18n/<kode-bahasa>.json` untuk teks chrome (sidebar, judul topik di daftar, dll.).
- Setiap halaman topik memuat `topics/i18n/<nama-topik>.<kode-bahasa>.json` untuk seluruh konten interaktifnya.
- Saat bahasa diganti dari sidebar, `index.html` menambahkan `?lang=<kode>` ke URL iframe dan memuat ulang. Saat bahasa diganti **dari dalam** halaman topik (toggle ID/EN di pojok kanan atasnya), halaman itu mengirim pesan (`postMessage`) kembali ke `index.html` supaya sidebar ikut sinkron tanpa memicu iframe dimuat ulang untuk kedua kalinya.

## Daftar topik

- [x] TCP/IP — perjalanan sebuah paket dari satu titik ke titik lain (encapsulation & decapsulation di setiap layer)
- [x] DNS — bagaimana nama domain diterjemahkan menjadi alamat IP
- [x] Enkripsi asimetris — pasangan kunci publik/privat, dipakai untuk mengenkripsi pesan rahasia sekaligus tanda tangan digital
- [x] Blockchain — blok yang saling terhubung lewat hash dan konsensus jaringan
- [x] Multithreading — thread konkuren, race condition, dan bagaimana mutex lock mencegahnya
- [x] Docker/container vs. virtual machine
- [x] Git — lima skenario: alur dasar commit, branching & merging, konflik merge, reset vs. revert, dan rebase vs. merge
- [x] HTTPS/TLS — empat skenario: alur TLS handshake lengkap, rantai kepercayaan sertifikat, TLS 1.2 vs. 1.3, dan simulasi serangan man-in-the-middle
- [x] Event-Driven Architecture — empat skenario: pola publish/subscribe dasar, event queue untuk pemrosesan async, koreografi vs. orkestrasi, dan ketahanan terhadap kegagalan sebagian
- [x] LLM — cara Large Language Model memproses bahasa: tokenisasi, embedding, self-attention, dan generasi token
- [x] AOT vs. JIT compiler — empat skenario: alur kompilasi AOT, alur kompilasi JIT, startup time & warm-up, dan optimisasi adaptif

Semua topik dari daftar ide awal sudah selesai dibuat — ide topik baru dan kontribusi tetap sangat diterima.

## Menambah topik baru

1. Buat file HTML baru di dalam folder `topics/`. Gunakan halaman yang sudah ada (mis. `topics/tcp-ip-layers.html` atau `topics/asymmetric-encryption.html`) sebagai referensi struktur: elemen DOM diberi `id` kosong, lalu diisi lewat JavaScript dari file JSON — bukan ditulis sebagai teks statis langsung di HTML. Setiap topik bebas punya palet warna, font, dan identitas visualnya sendiri (semua topik yang ada sekarang memang sengaja dibuat berbeda) — satu-satunya kontrak bersama adalah query param `?lang=` dan mekanisme sinkronisasi bahasa lewat `postMessage` yang dijelaskan di bawah. Jika sebuah topik mencakup beberapa skenario berbeda alih-alih satu alur linear (lihat `topics/blockchain.html` atau `topics/git.html`), tambahkan tab switcher di atas step viewer, dengan tiap tab memiliki array langkahnya sendiri di JSON i18n (`i18n.steps.<scenarioKey>[...]`).
2. Buat file terjemahan kontennya di `topics/i18n/<nama-topik>.id.json` (dan `.en.json`, dst. untuk setiap bahasa yang sudah didukung).
3. Buka `index.html`, cari array `topics` di dekat bagian bawah tag `<script>`, lalu tambahkan entri baru:

   ```js
   {
     key: "newTopicKey",
     file: "topics/file-name.html"
   }
   ```

   Tambahkan di bawah kategori yang sudah ada (`categoryKey`: saat ini `network`, `security`, `systems`, `tools`, `architecture`, atau `ai`), atau buat objek kategori baru jika topiknya termasuk area yang berbeda.
4. Tambahkan judul & deskripsi topik itu ke **setiap** file di `i18n/` (mis. `i18n/id.json` dan `i18n/en.json`) di bawah `topics.<key>.title` dan `topics.<key>.desc`, dan tambahkan label kategori barunya di bawah `categories.<categoryKey>` kalau belum ada.
5. Jalankan lewat local server (lihat "Menjalankannya") dan pastikan topik baru muncul di sidebar dan terbuka dengan benar di setiap bahasa yang didukung.

## Menambah bahasa baru

1. Duplikat `i18n/id.json` menjadi `i18n/<kode-bahasa>.json` (mis. `i18n/ja.json`), lalu terjemahkan isinya.
2. Untuk setiap topik yang sudah ada, duplikat juga `topics/i18n/<nama-topik>.id.json` menjadi `topics/i18n/<nama-topik>.<kode-bahasa>.json` dan terjemahkan. Bagian seperti `sizeNum`, class CSS, dan struktur array **tidak** perlu diterjemahkan — hanya nilai teksnya saja (`title`, `desc`, `fields`, `sizeUnit`, dsb.).
3. Tambahkan kode bahasa itu ke array `LANGS` di `index.html` (dan di `topics/*.html` kalau toggle-nya juga ingin tersedia saat halaman topik dibuka berdiri sendiri).
4. Tidak perlu menerjemahkan semua topik sekaligus — file JSON yang belum ada untuk bahasa baru bisa ditambahkan bertahap.

## Lisensi

MIT — bebas dipakai, dimodifikasi, dan dibagikan untuk keperluan belajar.
