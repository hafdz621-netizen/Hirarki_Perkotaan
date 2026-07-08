<div align="center">

# UrbaStrata
### Analisis Hirarki Perkotaan — Skalogram Guttman &middot; Sentralitas Marshall &middot; Peta Interaktif

*Aplikasi web satu-berkas (single-file), 100% client-side, tanpa server dan tanpa proses build.*

[![DOI](https://img.shields.io/badge/DOI-10.5281%2Fzenodo.21252193-blue)](https://doi.org/10.5281/zenodo.21252193)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Live](https://img.shields.io/badge/demo-urbastrata.dpdns.org-brightgreen)](https://urbastrata.dpdns.org)

</div>

---

## Daftar Isi

- [Ringkasan](#ringkasan)
- [Fitur Utama](#fitur-utama)
- [Metode Analisis](#metode-analisis)
  - [1. Skalogram Guttman](#1-skalogram-guttman)
  - [2. Hirarki Langsung (Ordo)](#2-hirarki-langsung-ordo)
  - [3. Indeks Sentralitas Marshall](#3-indeks-sentralitas-marshall)
  - [4. Klasifikasi Kelas (Sturges)](#4-klasifikasi-kelas-sturges)
  - [5. Referensi Hierarki Sarana (Tiebreaker)](#5-referensi-hierarki-sarana-tiebreaker)
- [Format Data Masukan](#format-data-masukan)
- [Cara Menggunakan](#cara-menggunakan)
- [Peta & Skala Administrasi](#peta--skala-administrasi)
- [Ekspor Hasil](#ekspor-hasil)
- [Simpan & Muat Proyek](#simpan--muat-proyek)
- [Instalasi & Deployment](#instalasi--deployment)
- [Struktur Berkas & Arsitektur](#struktur-berkas--arsitektur)
- [Pustaka Eksternal](#pustaka-eksternal)
- [Privasi & Keamanan Data](#privasi--keamanan-data)
- [Batasan & Catatan Penting](#batasan--catatan-penting)
- [Pertanyaan Umum (FAQ)](#pertanyaan-umum-faq)
- [Rencana Pengembangan](#rencana-pengembangan)
- [Kontribusi](#kontribusi)
- [Lisensi](#lisensi)
- [Cara Mengutip](#cara-mengutip)
- [Kontributor](#kontributor)

---

## Ringkasan

**UrbaStrata** adalah alat bantu analisis **hirarki perkotaan** (*urban hierarchy analysis*) yang biasa digunakan dalam perencanaan wilayah dan kota (*urban and regional planning*) untuk menilai tingkat perkembangan/orde suatu wilayah berdasarkan kelengkapan sarana dan fasilitas publik yang dimilikinya (pendidikan, kesehatan, perdagangan, dll.).

Berbeda dari kebanyakan alat sejenis yang berupa spreadsheet manual atau skrip statistik terpisah, UrbaStrata menggabungkan **tiga metode analisis hirarki sekaligus** (Skalogram Guttman, Ordo Langsung, dan Sentralitas Marshall), lengkap dengan **visualisasi peta interaktif** dan **ekspor Shapefile**, dalam satu berkas HTML yang bisa langsung dibuka di peramban — tanpa instalasi, tanpa server, tanpa akun.

Cocok digunakan oleh mahasiswa, peneliti, konsultan, maupun instansi perencanaan (Bappeda, Dinas PUPR, dsb.) untuk studi RTRW, RDTR, atau kajian pusat pertumbuhan wilayah.

---

## Fitur Utama

| # | Fitur | Keterangan |
|---|---|---|
| 1 | **Impor Data** | Unggah `.xlsx`/`.xls`, pilih sheet, baris header, kolom identitas wilayah, kolom administrasi, dan atribut tambahan. |
| 2 | **Ringkasan Koefisien** | CR, CS, Total Error, dan ukuran matriks (item × identitas) ditampilkan otomatis setelah analisis dijalankan. |
| 3 | **Skalogram Guttman** | Peringkat item, peringkat kelengkapan wilayah, klasifikasi kelas hirarki, matriks observasi, dan matriks ideal — masing-masing dalam tab terpisah. |
| 4 | **Hirarki Langsung** | Peringkat wilayah berdasarkan jumlah total fasilitas. |
| 5 | **Sentralitas Marshall** | Pembobotan tiap sarana berdasarkan kelangkaannya dan skor sentralitas tiap wilayah. |
| 6 | **Peta Hirarki Perkotaan** | Visualisasi titik wilayah pada peta Leaflet, diwarnai menurut kelas hirarki hasil analisis. Titik hirarki selalu di lapisan paling atas (*always on top*), tidak tertutup layer lain. |
| 7 | **Multi-Basemap** | Empat pilihan basemap: OpenStreetMap, Esri Satellite, Esri Topo Map, CartoDB Light — dipilih dari panel kontrol layer di peta. |
| 8 | **Overlay Tutupan Lahan (LULC)** | Layer *Indonesia Hybrid Landcover 2020* (CSIRO, resolusi 10 m, gabungan Sentinel-1 SAR + Sentinel-2 optik + ALOS-2 PALSAR-2), 14 kelas tutupan lahan, lengkap legenda otomatis. |
| 9 | **Batas Administrasi Resmi** | Layer batas Kabupaten/Kota, Kecamatan, dan Desa/Kelurahan dimuat otomatis dari server BIG (Kebijakan Satu Peta) via query GeoJSON — hanya wilayah yang teranalisis, bisa diklik untuk info nama wilayah. |
| 10 | **Referensi Hierarki Sarana** | Pengaturan tingkat hierarki tiap jenis sarana (otomatis atau manual) sebagai *tiebreaker* skor identik. |
| 11 | **Fitur Atribut** | Kolom administrasi (Kabupaten/Kota, Kecamatan) atau kolom lain apa pun bisa "dijadikan atribut" — ikut tampil sebagai keterangan tambahan di tabel/peringkat tanpa dihitung sebagai data sarana. |
| 12 | **Ekspor Excel** | Unduh seluruh hasil analisis (8 sheet) sebagai satu berkas `.xlsx`. |
| 13 | **Ekspor Shapefile** | Unduh titik-titik peta sebagai Shapefile (`.shp`/`.shx`/`.dbf`/`.prj`) dalam `.zip`, dibangun langsung di browser tanpa pustaka GIS eksternal. |
| 14 | **Simpan/Muat Proyek** | Seluruh kondisi kerja disimpan sebagai berkas `.urbhir` (JSON) dan bisa dimuat kembali kapan saja. |
| 15 | **Data Contoh** | Tersedia dataset contoh (data kecamatan di Kabupaten Kendal) untuk mencoba aplikasi tanpa perlu menyiapkan data sendiri. |

---

## Metode Analisis

### 1. Skalogram Guttman

Metode ini menyusun data biner (ada/tidaknya suatu sarana di suatu wilayah) menjadi pola tangga (*Guttman scale*) untuk menguji apakah hirarki fasilitas bersifat kumulatif — artinya, wilayah yang punya sarana tingkat tinggi seharusnya juga punya sarana tingkat di bawahnya.

**Coefficient of Reproducibility (CR)**

```
CR = 1 − (Total Error / Total Sel)
Total Sel = jumlah item (jenis sarana) × jumlah identitas (wilayah)
```

**Coefficient of Scalability (CS)**

```
CS = (CR − MMR) / (1 − MMR)
```

di mana **MMR** (*Minimal Marginal Reproducibility*) dihitung dari proporsi respons modal (jawaban mayoritas: ada/tidak) tiap item, dijumlahkan ke seluruh sel matriks.

**Kriteria kelayakan skala** (standar umum dalam literatur Guttman scaling):

| Koefisien | Ambang Minimum | Interpretasi |
|---|---|---|
| CR | ≥ 0.90 | Skala cukup akurat merepresentasikan pola respons |
| CS | ≥ 0.60 | Skala benar-benar bersifat unidimensional (satu dimensi hirarki) |

Aplikasi menampilkan matriks **observasi** (data riil, dengan sel error ditandai warna merah) berdampingan dengan matriks **ideal** (pola tangga sempurna: pada baris berskor *k*, item peringkat 1..*k* = 1, sisanya 0), beserta legenda warna.

### 2. Hirarki Langsung (Ordo)

Pendekatan paling sederhana: wilayah diperingkat berdasarkan **total unit sarana** yang dimiliki, tanpa pembobotan apa pun. Berguna sebagai pembanding cepat terhadap hasil Skalogram maupun Marshall.

### 3. Indeks Sentralitas Marshall

Mengacu pada pendekatan Marshall (1969) untuk mengukur derajat "kepusatan" (*centrality*) suatu wilayah:

1. Setiap jenis sarana diberi **bobot** yang berbanding terbalik dengan jumlah wilayah yang memilikinya — sarana yang langka (hanya dimiliki sedikit wilayah, mis. rumah sakit) mendapat bobot lebih tinggi daripada sarana yang umum (mis. sekolah dasar).
2. Nilai tiap sel (jumlah unit sarana) dikalikan bobot sarana tersebut.
3. **Indeks Sentralitas** wilayah = jumlah seluruh nilai terbobot pada baris wilayah tersebut.

Wilayah dengan indeks sentralitas tertinggi dianggap sebagai pusat pelayanan (*service center*) paling dominan dalam sistem wilayah yang dianalisis.

### 4. Klasifikasi Kelas (Sturges)

Baik hasil Skalogram maupun Marshall diklasifikasikan ke dalam kelas-kelas hirarki (mis. Orde I, II, III) menggunakan aturan Sturges:

```
k = 1 + 3.322 × log(n)         (dibulatkan ke atas → jumlah kelas)
Rentang Data = Skor Maks − Skor Min
Interval Kelas = Rentang Data / k   (dibulatkan ke atas)
```

di mana *n* = jumlah wilayah. Aplikasi menampilkan seluruh komponen perhitungan ini (n, k, rentang, interval) secara transparan, beserta daftar wilayah anggota tiap kelas.

### 5. Referensi Hierarki Sarana (Tiebreaker)

Karena skor dua item atau dua wilayah bisa saja identik, aplikasi menyediakan panel **Referensi Hierarki Sarana** untuk menetapkan tingkat hierarki tiap jenis sarana (Tingkat 1 = paling eksklusif/tinggi, mis. Rumah Sakit atau Universitas; tingkat lebih besar = lebih dasar/umum).

- **Diisi otomatis** berdasarkan jumlah wilayah pemilik tiap sarana (makin sedikit wilayah pemilik → makin tinggi tingkatnya), dengan rata-rata unit per wilayah sebagai tiebreaker sekunder.
- **Bisa diedit manual** sesuai penilaian pengguna, lalu diterapkan ulang ke seluruh perhitungan.
- **Logika tiebreaker:** untuk item, jika total kepemilikan sama → sarana bertingkat lebih besar (lebih dasar) ditempatkan lebih kiri pada matriks. Untuk wilayah, jika skor sama → wilayah dengan sarana bertingkat lebih kecil (lebih eksklusif) ditempatkan lebih atas.

---

## Format Data Masukan

Susun berkas Excel dengan struktur berikut:

- **Setiap baris = satu wilayah** (desa, kecamatan, atau kabupaten/kota).
- **Setiap kolom = satu jenis sarana/fasilitas**, berisi **jumlah unit** (angka, boleh 0).
- **Baris header** (nama kolom) bisa berada di baris mana pun — tentukan nomor barisnya saat impor, atau isi `0` jika tidak ada header.
- Berkas boleh berisi **lebih dari satu sheet** — sheet mana yang dipakai bisa dipilih setelah file dimuat.
- Kolom identitas wilayah (nama wilayah) dipilih terpisah dari kolom data sarana.
- Kolom administrasi (**Kabupaten/Kota**, **Kecamatan**) bersifat opsional, digunakan untuk memperjelas pencarian lokasi di peta (geocoding) dan bisa "dijadikan atribut" agar ikut tampil sebagai keterangan tambahan.
- Kolom lain di luar itu juga bisa dipilih sebagai **atribut tambahan** (boleh lebih dari satu) — akan disertakan sebagai informasi pelengkap, tidak dihitung sebagai data sarana.

Contoh struktur tabel minimal:

| Kabupaten | Kecamatan | TK Negeri | SD Negeri | SMP Negeri | Rumah Sakit | Pasar |
|---|---|---|---|---|---|---|
| Kendal | Boja | 2 | 39 | 4 | 2 | 4 |
| Kendal | Kaliwungu | 0 | 23 | 1 | 1 | 3 |
| … | … | … | … | … | … | … |

> Tersedia data contoh siap pakai (22 kecamatan di Kabupaten Kendal, 20+ jenis sarana) melalui tautan **"gunakan data contoh untuk mencoba"** pada bagian Impor Data, tanpa perlu mengunggah berkas apa pun.

---

## Cara Menggunakan

1. **Buka `index.html`** langsung di peramban modern (Chrome, Firefox, Edge, Safari terbaru) — cukup klik dua kali, tidak perlu server maupun instalasi.
2. **Impor Data** — unggah berkas Excel, atau gunakan data contoh. Tentukan sheet, baris header, kolom identitas wilayah, serta (opsional) kolom administrasi dan atribut tambahan.
3. Klik **"Jalankan Analisis Skalogram"** — ketiga metode (Skalogram, Ordo, Marshall) dihitung sekaligus.
4. Telusuri hasil:
   - **Ringkasan Koefisien** — CR, CS, Total Error.
   - **Skalogram Guttman** — peringkat item, kelengkapan wilayah, hirarki kelas, matriks observasi & ideal.
   - **Hirarki Langsung** — peringkat berdasarkan jumlah fasilitas.
   - **Sentralitas Marshall** — bobot sarana & skor sentralitas wilayah.
5. (Opsional) Buka panel **Referensi Hierarki** untuk menyesuaikan tingkat hierarki tiap sarana secara manual, lalu terapkan ulang.
6. **Peta Hirarki Perkotaan** — pilih skala administrasi (Desa/Kelurahan atau Kecamatan/Kabupaten-Kota) dan metode hirarki yang ingin divisualisasikan, lalu klik **"Cari Lokasi & Tampilkan Peta"**.
7. **Unduh hasil** — berkas Excel lengkap (`.xlsx`) dan/atau titik peta sebagai Shapefile (`.zip`).
8. (Opsional) **Simpan proyek** (`.urbhir`) agar bisa melanjutkan pekerjaan di sesi lain tanpa mengulang dari awal.

---

## Peta & Skala Administrasi

UrbaStrata mendukung dua skala visualisasi peta, masing-masing dengan sumber koordinat berbeda:

| Skala | Sumber Koordinat | Catatan |
|---|---|---|
| **Desa/Kelurahan** | Berkas **Shapefile titik administrasi desa** yang diunggah pengguna sendiri (`.zip` berisi `.shp` + `.shx` + `.dbf`), atau dimuat langsung dari URL publik (mis. GitHub raw). | Lebih akurat, tidak bergantung pencarian nama via internet. Pengguna memetakan field nama Desa/Kecamatan/Kabupaten dari Shapefile ke data mereka. Desa yang tak ditemukan di Shapefile akan otomatis dicoba lewat geocoding sebagai cadangan. |
| **Kecamatan/Kabupaten-Kota** | Layanan geocoding gratis **OpenStreetMap Nominatim**. | Berjalan di server donasi berkapasitas terbatas, maksimal **1 permintaan/detik** — lihat [kebijakan penggunaan Nominatim](https://operations.osmfoundation.org/policies/nominatim/). |

Batas wilayah (poligon desa/kecamatan) yang ditampilkan di peta merupakan **hasil simplifikasi** untuk keperluan visualisasi web — lihat [Batasan & Catatan Penting](#batasan--catatan-penting).

Titik-titik pada peta diberi warna sesuai kelas hirarki hasil analisis, lengkap dengan legenda.

### Basemap & Overlay

Panel kontrol layer (ikon tumpukan di pojok kanan atas peta) menyediakan satu basemap aktif dan overlay yang bisa dinyalakan bersamaan:

| Jenis | Nama | Sumber |
|---|---|---|
| Basemap | OpenStreetMap | Tile jalan/kawasan komunitas OSM (default) |
| Basemap | Esri Satellite | Citra satelit Esri World Imagery |
| Basemap | Esri Topo Map | Peta topografi Esri (relief, kontur) |
| Basemap | CartoDB Light | Basemap minimalis latar terang |
| Overlay | LULC Indonesia 10m | *Indonesia Hybrid Landcover 2020* (CSIRO) — hasil gabungan Sentinel-1 SAR, Sentinel-2 optik, dan ALOS-2 PALSAR-2; 14 kelas tutupan lahan, legenda otomatis muncul di pojok kiri bawah peta saat diaktifkan. Diakses via tile cache publik ArcGIS. Sumber dataset: [data.csiro.au/collection/csiro:55614](https://data.csiro.au/collection/csiro:55614). |

### Batas Administrasi Resmi (BIG)

Selain poligon desa hasil simplifikasi (lihat [Batasan & Catatan Penting](#batasan--catatan-penting)), peta juga memuat batas administrasi **Kabupaten/Kota**, **Kecamatan**, dan **Desa/Kelurahan** langsung dari server **Badan Informasi Geospasial (BIG)** melalui **Kebijakan Satu Peta** — bukan tile gambar, melainkan hasil query GeoJSON yang dirender Leaflet, sehingga bisa diklik untuk menampilkan nama wilayah.

- Hanya wilayah yang teranalisis yang dimuat (difilter berdasarkan Kabupaten/Kota dan Kecamatan pada data), bukan seluruh Indonesia.
- Batas Kab/Kota dan Kecamatan: fill hijau toska transparan, garis merah tua. Batas Desa/Kel: garis tipis abu-abu tanpa fill.
- Filter membutuhkan kolom Kabupaten/Kota dan Kecamatan terisi saat impor data; jika tidak, muncul peringatan bahwa filter tidak dapat diterapkan.
- Jika server BIG tidak dapat diakses, titik hirarki tetap ditampilkan tanpa layer batas administrasi.

---

## Ekspor Hasil

### Berkas Excel (`.xlsx`)

Ekspor satu berkas dengan **8 sheet**:

| # | Nama Sheet | Isi |
|---|---|---|
| 1 | Data Mentah | Tabel data asal (wilayah × sarana) beserta atribut, jika diaktifkan |
| 2 | Matriks Ideal | Pola tangga Guttman ideal |
| 3 | Matriks Observasi & CR | Matriks biner riil, Total Error, CR, MMR, CS |
| 4 | Hirarki Skalogram | Klasifikasi kelas hirarki hasil metode Skalogram |
| 5 | Data Marshall Terbobot | Nilai tiap sel × bobot sarana, beserta atribut |
| 6 | Hirarki Marshall | Klasifikasi kelas hirarki hasil metode Marshall |
| 7 | Hirarki Jumlah Fasilitas | Klasifikasi kelas hirarki hasil metode Ordo |
| 8 | Referensi Hierarki Sarana | Tingkat hierarki tiap jenis sarana (otomatis/manual) |

> Sheet yang berisi identitas wilayah (1, 3, 5) menyertakan kolom **Atribut** bila fitur atribut diaktifkan. Sheet hirarki (4, 6, 7) tidak menyertakan kolom Atribut.

### Shapefile (`.zip`)

Titik-titik wilayah pada peta (beserta nama, atribut, tingkat hirarki, dan skor) dapat diunduh sebagai Shapefile standar (`wilayah.shp`, `.shx`, `.dbf`, `.prj` — proyeksi WGS84), dibungkus dalam satu `.zip`, siap dibuka di QGIS/ArcGIS. Berkas ini dibangun langsung di browser (format biner Shapefile ditulis manual byte-per-byte via `DataView`), **tanpa** bergantung pada pustaka GIS pihak ketiga.

---

## Simpan & Muat Proyek

Tombol **Save**/**Open** pada dok bawah layar memungkinkan seluruh kondisi kerja disimpan sebagai satu berkas `.urbhir` (format JSON), berisi:

- Nama berkas asal dan nama sheet yang dipakai
- Pengaturan baris header
- Kolom identitas wilayah, kolom administrasi, dan atribut yang dipilih
- Seluruh data workbook (baris/kolom mentah)
- Hasil perhitungan Skalogram dan Marshall
- Titik-titik peta terakhir (jika sudah pernah ditampilkan)

Berkas ini bisa dibuka kembali lewat tombol **Open** untuk melanjutkan analisis tanpa perlu mengunggah ulang berkas Excel maupun mengulang pengaturan.

---

## Instalasi & Deployment

Tidak ada proses *build*, *bundler*, atau dependensi npm — UrbaStrata adalah satu berkas `index.html` mandiri.

**Menjalankan secara lokal:**
```bash
git clone https://github.com/<username>/<repo>.git
cd <repo>
# buka langsung di peramban
open index.html      # macOS
xdg-open index.html  # Linux
start index.html     # Windows
```

**Hosting statis** (opsional, agar bisa diakses via URL publik) — unggah `index.html` ke salah satu layanan berikut, tanpa konfigurasi tambahan:

- GitHub Pages
- Netlify / Vercel (static deploy)
- Cloudflare Pages
- Server statis apa pun (Nginx, Apache, dsb.)

---

## Struktur Berkas & Arsitektur

```
.
├── index.html      # markup (HTML), styling (CSS), dan seluruh logika (JavaScript)
└── README.md
```

Seluruh kode — markup, styling, dan logika — sengaja digabung dalam satu berkas agar aplikasi bisa didistribusikan dan dijalankan semudah mungkin (cukup satu berkas untuk dibagikan atau diarsipkan). Logika utama JavaScript disusun dalam beberapa IIFE (*Immediately Invoked Function Expression*) yang menangani, antara lain:

- Pembacaan & parsing Excel (SheetJS)
- Perhitungan Skalogram, Ordo, dan Marshall
- Manajemen state UI (navigasi antar-bagian, tab, atribut)
- Interaksi peta (Leaflet) & geocoding
- Pembacaan/penulisan Shapefile biner secara manual
- Ekspor Excel multi-sheet
- Simpan/muat proyek (`.urbhir`)

---

## Pustaka Eksternal

Seluruhnya dimuat via CDN (`cdnjs.cloudflare.com`), tanpa perlu instalasi lokal:

| Pustaka | Versi | Kegunaan |
|---|---|---|
| [SheetJS (`xlsx`)](https://sheetjs.com/) | 0.18.5 | Membaca & menulis berkas Excel |
| [Leaflet](https://leafletjs.com/) | 1.9.4 | Peta interaktif |
| [JSZip](https://stuk.github.io/jszip/) | 3.10.1 | Membaca/menulis arsip `.zip` (Shapefile & proyek) |
| [TopoJSON](https://github.com/topojson/topojson) | 3.0.2 | Pemrosesan data batas administrasi (poligon desa/kecamatan) |

Font: *Source Serif 4*, *JetBrains Mono*, *Inter* (Google Fonts).

---

## Privasi & Keamanan Data

- **Tidak ada backend.** Seluruh pembacaan Excel, perhitungan statistik, dan pembuatan Shapefile berjalan sepenuhnya di peramban pengguna (client-side). Data yang diunggah **tidak pernah dikirim** ke server mana pun oleh aplikasi ini.
- **Permintaan jaringan** hanya terjadi untuk: (a) memuat pustaka via CDN saat halaman dibuka, (b) permintaan opsional ke OpenStreetMap Nominatim saat pengguna menampilkan peta skala kecamatan/kabupaten, dan (c) pengunduhan Shapefile dari URL yang pengguna masukkan sendiri (skala desa).
- Cocok digunakan untuk data yang bersifat sensitif/internal, selama koneksi internet hanya diperlukan untuk fitur peta.

---

## Batasan & Catatan Penting

- **Bukan batas administrasi resmi.** Poligon desa/kecamatan yang ditampilkan di peta adalah hasil simplifikasi untuk keperluan visualisasi web. Untuk keperluan resmi/legal, gunakan data dari Badan Informasi Geospasial (BIG) — [kspservices.big.go.id](https://kspservices.big.go.id).
- **Keterbatasan Nominatim.** Geocoding skala kecamatan/kabupaten memakai layanan gratis dengan batas laju 1 permintaan/detik — pencarian untuk dataset besar bisa memakan waktu.
- **Akurasi nama wilayah.** Pencocokan nama wilayah (baik ke Shapefile desa maupun ke hasil geocoding) bergantung pada kesamaan penulisan nama; variasi ejaan dapat menyebabkan wilayah tidak ditemukan.
- **Performa** bergantung pada kapasitas peramban dan perangkat pengguna, karena seluruh komputasi (termasuk penyusunan Shapefile biner) berjalan di sisi klien.
- Format proyek `.urbhir` adalah format internal aplikasi (JSON) dan tidak dijamin kompatibel lintas versi aplikasi yang berbeda signifikan.

---

## Pertanyaan Umum (FAQ)

**Apakah data saya diunggah ke server?**
Tidak. Semua pemrosesan file Excel dan perhitungan terjadi di peramban Anda sendiri.

**Apakah saya perlu koneksi internet?**
Untuk memuat aplikasi pertama kali (pustaka via CDN) dan untuk fitur peta (geocoding/pemuatan Shapefile dari URL), ya. Untuk impor data dan seluruh perhitungan analisis, tidak.

**Metode mana yang paling tepat digunakan?**
Bergantung pada tujuan kajian. Skalogram Guttman cocok untuk menguji konsistensi hirarki secara statistik (dengan uji CR/CS). Ordo Langsung cocok untuk gambaran cepat tanpa asumsi statistik. Sentralitas Marshall cocok saat ingin mempertimbangkan kelangkaan/eksklusivitas tiap jenis sarana.

**Bagaimana jika dua wilayah punya skor identik?**
Gunakan panel Referensi Hierarki Sarana untuk mengatur tiebreaker berdasarkan tingkat hierarki jenis sarana.

**Apakah bisa dipakai untuk skala provinsi/nasional?**
Secara teknis bisa (kolom identitas wilayah = provinsi, dsb.), namun fitur peta skala Desa/Kecamatan yang tersedia saat ini dirancang untuk skala sub-kabupaten.

---

## Rencana Pengembangan

Beberapa arah pengembangan yang mungkin dilakukan ke depan (silakan ajukan lewat *Issues* jika ada prioritas tertentu):

- [ ] Dukungan format data masukan tambahan (CSV, Google Sheets)
- [ ] Ekspor peta sebagai GeoJSON
- [ ] Mode perbandingan antar-periode (analisis time-series hirarki)
- [ ] Uji statistik tambahan (indeks lain di luar Guttman/Marshall)
- [ ] Lokalisasi bahasa (Inggris)

---

## Kontribusi

Kontribusi berupa laporan bug, saran fitur, maupun *pull request* sangat terbuka.

1. *Fork* repositori ini
2. Buat *branch* baru (`git checkout -b fitur/nama-fitur`)
3. Lakukan perubahan pada `index.html`
4. Uji secara manual di peramban (tidak ada proses build/test otomatis)
5. Ajukan *pull request* dengan deskripsi perubahan yang jelas

Karena seluruh kode berada dalam satu berkas, mohon jaga agar perubahan tetap terstruktur mengikuti pola penamaan dan pengelompokan (CSS di `<style>`, markup per bagian, logika JS per IIFE) yang sudah ada.

---

## Lisensi

Proyek ini dilisensikan di bawah **[MIT License](./LICENSE)** — bebas digunakan, disalin, dimodifikasi, dan didistribusikan ulang (termasuk untuk keperluan komersial), selama pemberitahuan hak cipta dan lisensi asli disertakan. Lihat berkas [`LICENSE`](./LICENSE) untuk teks lengkap.

---

## Cara Mengutip

Proyek ini memiliki DOI arsip di Zenodo: **[10.5281/zenodo.21252193](https://doi.org/10.5281/zenodo.21252193)**. Metadata sitasi lengkap tersedia di berkas [`CITATION.cff`](./CITATION.cff) (dapat digunakan langsung lewat tombol "Cite this repository" di GitHub).

**BibTeX:**

```bibtex
@software{urbastrata2026,
  author       = {Anshori, Ahmad Hafidz},
  title        = {UrbaStrata: Analisis Hirarki Perkotaan},
  year         = {2026},
  publisher    = {Zenodo},
  doi          = {10.5281/zenodo.21252193},
  url          = {https://github.com/hafdz621-netizen/Hirarki_Perkotaan}
}
```

---

## Kontributor

Dibangun oleh **Ahmad Hafidz Anshori** ([@ans91.hafdz](https://www.instagram.com/ans91.hafdz/)) dengan bantuan Claude (Anthropic).

Kontribusi dari pihak lain akan dicantumkan di sini — lihat bagian [Kontribusi](#kontribusi).
