# 🧮 Kira Gaji — Malaysian Salary Calculator

> Kalkulator gaji Malaysia yang mengira KWSP, SOCSO, EIS, PCB/MTD dan Zakat Pendapatan secara tepat dan pantas. Boleh dipasang sebagai Progressive Web App (PWA).

🔗 **[Buka Aplikasi — muhamaddarulhadi.github.io/Malaysia-Salary-And-Zakat-Calculator](https://muhamaddarulhadi.github.io/Malaysia-Salary-And-Zakat-Calculator/)**

---

## 📋 Kandungan

- [Tentang](#tentang)
- [Ciri-ciri](#ciri-ciri)
- [Pengiraan](#pengiraan)
- [Struktur Fail](#struktur-fail)
- [Cara Guna](#cara-guna)
- [Cara Deploy](#cara-deploy)
- [PWA — Pasang sebagai App](#pwa--pasang-sebagai-app)
- [Teknologi](#teknologi)
- [Penafian](#penafian)

---

## Tentang

**Kira Gaji** adalah alat pengiraan gaji Malaysia yang berjalan sepenuhnya di pelayar web tanpa memerlukan pelayan atau pangkalan data. Semua pengiraan dilakukan secara tempatan menggunakan JavaScript tulen.

Dicipta oleh **M.D.Hadi** — [github.com/muhamaddarulhadi](https://github.com/muhamaddarulhadi)

---

## Ciri-ciri

### 💰 Pengiraan Gaji
- Input gaji pokok dan elaun tambahan yang boleh dikonfigurasi
- Paparan jumlah pendapatan kasar secara masa nyata
- Sokongan potongan lain-lain (PTPTN, pinjaman, dll.)

### 🏦 KWSP (Kumpulan Wang Simpanan Pekerja)
- Pengiraan berdasarkan Jadual Ketiga Akta KWSP 452
- Sokongan pelbagai profil pekerja:
  - Warganegara Malaysia
  - Pemastautin Tetap (PR)
  - Pekerja Asing
- Sokongan pelbagai kadar:
  - Standard (11% pekerja)
  - Dikurangkan (9% pekerja)
  - Dikecualikan (0% pekerja)
- Kadar berbeza untuk pekerja bawah 60 dan 60 ke atas
- Paparan caruman pekerja dan majikan berasingan

### 🛡️ PERKESO (SOCSO + EIS)
- **SOCSO** — berdasarkan jadual kadar caruman tepat Akta 4 (Skim Bencana Pekerjaan + Skim Keilatan)
- **EIS/SIP** — berdasarkan Akta 800, had gaji RM6,000
- Toggle untuk aktif/nyahaktif syer pekerja secara berasingan
- Paparan syer pekerja dan majikan berasingan

### 📊 PCB / MTD (Potongan Cukai Bulanan)
- Formula pengiraan berdasarkan spesifikasi LHDN 2026 (kaedah berkomputer)
- Menggunakan formula rasmi: `MTD = [(P − M) × R + B] ÷ (n+1)`
- Relief KWSP dikira menggunakan formula K1/K2 yang betul per spesifikasi LHDN
- Rebat individu RM400 (Cat 1 & 3) dan RM800 (Cat 2) diambil kira secara automatik
- Sokongan tiga kategori pekerja:
  - **Kategori 1** — Bujang / Janda / Duda
  - **Kategori 2** — Berkahwin, pasangan tidak bekerja
  - **Kategori 3** — Berkahwin, pasangan bekerja / bercerai / balu/duda dengan anak angkat
- Sokongan anak di bawah 18 tahun dan anak OKU
- Paparan pendapatan bercukai tahunan, cukai tahunan dan kadar efektif
- **Penunjuk had pendapatan tidak bercukai** — paparan jadual ambang PCB = RM0 mengikut kategori
- **Penunjuk PCB < RM10** — amaran LHDN bahawa majikan tidak wajib potong jika MTD bulanan kurang RM10, dengan toggle untuk majikan yang ingin tetap memotong

### 🌙 Zakat Pendapatan
- Sokongan 16 negeri dengan pihak berkuasa zakat masing-masing:
  - Selangor (LZS), W.P. Kuala Lumpur (MAIWP), W.P. Putrajaya (MAIWP)
  - W.P. Labuan (MZWPL), Johor (MZAJ), Kedah (LZNK), Kelantan (MAIK)
  - Melaka, Negeri Sembilan (MAINS), Pahang (PAIP)
  - Pulau Pinang (MAINPP), Perak (MAIAMP), Perlis (MAIPs)
  - Sabah (MUIS), Sarawak (Baitulmal), Terengganu (MAIDAM)
- Nisab boleh dikonfigurasi (default RM8,200/tahun)
- Kadar zakat boleh dikonfigurasi (default 2.5%)
- Toggle untuk tolak KWSP dari asas zakat
- Toggle untuk tolak PERKESO (SOCSO + EIS) dari asas zakat
- Paparan status wajib/tidak wajib zakat berdasarkan nisab
- Semua tetapan zakat boleh disembunyikan jika tidak diperlukan

### 📈 Carta & Ringkasan
- Carta donut interaktif pecahan gaji (Chart.js)
- Ringkasan potongan wajib bulanan
- Paparan gaji bersih (take-home pay) yang menonjol

### 🎨 Antara Muka
- Mod terang dan mod gelap dengan peralihan lancar
- **Tema disimpan dalam `localStorage`** — pilihan disimpan secara kekal
- Reka bentuk responsif untuk desktop dan mudah alih
- Dropdown Select2 jQuery dengan carian untuk pemilihan negeri
- Animasi fade-in yang halus

---

## Pengiraan

### Formula KWSP
Caruman pekerja dikira sebagai peratusan gaji kasar, dibundarkan ke atas ke ringgit terdekat (`Math.ceil`). Kadar bergantung kepada kewarganegaraan dan umur pekerja.

| Profil | Pekerja | Majikan |
|--------|---------|---------|
| WN / PR < 60 (standard) | 11% | 13% (≤RM5,000) / 12% (>RM5,000) |
| WN / PR < 60 (dikurangkan) | 9% | 13% / 12% |
| WN 60+ | 0% | 4% |
| PR 60+ (standard) | 5.5% | 6.5% (≤RM5,000) / 6% (>RM5,000) |
| Asing | 11% | RM5 (flat) |

### Formula PCB/MTD
Berdasarkan **Spesifikasi Pengiraan PCB Berkomputer LHDN 2026**:

```
P = Gaji Tahunan − [Relief Peribadi + Relief Pasangan + Relief Anak + Relief KWSP]

Relief KWSP:
  K1  = Caruman KWSP bulan semasa
  K2  = min( (4000 − K1) ÷ 11, K1 )  ← ditolak kepada 2 titik perpuluhan
  Relief KWSP = K1 + (K2 × 11), had RM4,000

MTD = [(P − M) × R + B] ÷ 12
```

Nilai `B` dalam Table 1 sudah mengandungi **rebat individu** (RM400 untuk Cat 1 & 3, RM800 untuk Cat 2) bagi P ≤ RM35,000.

### SOCSO & EIS
Dikira berdasarkan jadual kadar tepat dari Akta 4 dan Akta 800. Had gaji RM6,000 untuk kedua-duanya.

---

## Struktur Fail

Semua fail perlu berada dalam **folder yang sama**:

```
📁 kira-gaji/
├── 📄 malaysia-salary-calculator.html   # Aplikasi utama
├── 📄 manifest.json                     # PWA Web App Manifest
├── ⚙️  sw.js                             # Service Worker (cache & offline)
├── 🖼️  favicon.ico                       # Ikon tab pelayar
├── 🖼️  icon-192.png                      # Ikon PWA (Android/Chrome)
├── 🖼️  icon-512.png                      # Ikon PWA (splash screen)
└── 📄 README.md                         # Dokumentasi ini
```

> ⚠️ **Penting**: Semua 6 fail mesti berada dalam folder yang sama. PWA tidak akan berfungsi jika fail berpisah atau laluan tidak betul.

---

## Cara Guna

### 1. Masukkan Maklumat Gaji
- Masukkan **Gaji Pokok** dalam kotak input utama
- Tambah elaun jika ada (klik **＋ Tambah Elaun**)
- Jumlah Pendapatan Kasar dikira secara automatik

### 2. Tetapkan Profil Pekerja & KWSP
- Pilih **kewarganegaraan**: Warganegara / PR / Asing
- Pilih **umur**: Di bawah 60 / 60 ke atas
- Pilih **kadar KWSP**: Standard / Dikurangkan / Dikecualikan

### 3. Tetapkan PERKESO
- Toggle SOCSO dan EIS mengikut keperluan
- Pekerja asing dan pekerja 60+ dikecualikan secara automatik

### 4. Tetapkan PCB / MTD
- Pilih **status perkahwinan** dari dropdown
- Masukkan bilangan **anak di bawah 18** dan **anak OKU**

### 5. Tetapkan Zakat (Muslim sahaja)
- Aktifkan toggle **Kira Zakat Pendapatan?**
- Pilih **negeri** dari dropdown (carian tersedia)
- Ubah nilai nisab jika berbeza dari default negeri anda
- Toggle untuk tolak KWSP / PERKESO dari asas pengiraan zakat

### 6. Semak Keputusan
- **Gaji Bersih** dipaparkan secara tenonjol di bahagian kanan
- Semak pecahan potongan dalam kad KWSP, PERKESO, PCB dan Zakat
- Ringkasan keseluruhan potongan bulanan di bawah carta

---

## Cara Deploy

### GitHub Pages
1. Muat naik semua 6 fail ke repositori GitHub anda
2. Pergi ke **Settings → Pages**
3. Tetapkan sumber kepada branch `main` atau `master`
4. Akses melalui `https://username.github.io/repo-name/malaysia-salary-calculator.html`

### Web Server Biasa (Apache / Nginx)
1. Salin semua 6 fail ke folder `public_html` atau direktori web anda
2. Pastikan pelayan menghidangkan fail dengan jenis MIME yang betul
3. Untuk PWA berfungsi sepenuhnya, laman web **mesti dihidangkan melalui HTTPS**

### Penggunaan Tempatan
Buka terus `malaysia-salary-calculator.html` dalam pelayar — semua ciri pengiraan berfungsi secara offline. Namun Service Worker dan PWA memerlukan pelayan HTTP (bukan `file://`).

---

## PWA — Pasang sebagai App

Kira Gaji boleh dipasang sebagai aplikasi penuh pada peranti anda:

### Android (Chrome)
1. Buka laman web dalam Chrome
2. Klik butang **⬇ Install App** di bar header, ATAU
3. Klik menu ⋮ → **Tambah ke Skrin Utama**

### iOS (Safari)
1. Buka laman web dalam Safari
2. Klik ikon **Share** (kotak dengan anak panah ke atas)
3. Pilih **Add to Home Screen**

### Desktop (Chrome / Edge)
1. Klik ikon ⊕ di bar alamat, ATAU
2. Klik butang **⬇ Install App** di header

**Ciri PWA:**
- ✅ Boleh digunakan sepenuhnya **tanpa internet** selepas pemasangan pertama
- ✅ Dipaparkan seperti aplikasi native (tiada bar pelayar)
- ✅ Ikon pada skrin utama / desktop
- ✅ Tema warna diselaraskan dengan sistem

---

## Teknologi

| Teknologi | Versi | Kegunaan |
|-----------|-------|---------|
| HTML5 / CSS3 / JavaScript | — | Asas aplikasi |
| [Chart.js](https://www.chartjs.org/) | 4.4.0 | Carta donut pecahan gaji |
| [jQuery](https://jquery.com/) | 3.7.1 | Asas Select2 |
| [Select2](https://select2.org/) | 4.1.0-rc.0 | Dropdown negeri & status perkahwinan |
| [Plus Jakarta Sans](https://fonts.google.com/specimen/Plus+Jakarta+Sans) | — | Fon utama |
| [DM Mono](https://fonts.google.com/specimen/DM+Mono) | — | Fon nombor |
| Service Worker API | — | Cache & sokongan offline |
| Web App Manifest | — | PWA metadata |
| localStorage | — | Simpan pilihan tema |

**Tiada framework, tiada backend, tiada pangkalan data.** Semua kod berjalan sepenuhnya di pelayar.

---

## Rujukan Undang-undang & Peraturan

| Komponen | Rujukan |
|----------|---------|
| KWSP | Akta KWSP 452, Jadual Ketiga |
| SOCSO | Akta Keselamatan Sosial Pekerja 1969 (Akta 4) |
| EIS / SIP | Akta Sistem Insurans Pekerjaan 2017 (Akta 800) |
| PCB / MTD | Spesifikasi Pengiraan PCB Berkomputer LHDN 2026 |
| Rebat Cukai | Kaedah-Kaedah Cukai Pendapatan (Potongan Daripada Saraan) 1994 |
| Zakat | Pihak Berkuasa Agama Islam negeri masing-masing |

---

## Penafian

> **Kira Gaji** adalah alat anggaran sahaja dan **bukan nasihat kewangan atau cukai profesional**.
>
> - Pengiraan KWSP berdasarkan Jadual Ketiga (Akta 452)
> - Pengiraan SOCSO berdasarkan jadual kadar tepat Akta 4
> - Pengiraan PCB berdasarkan spesifikasi LHDN 2026 dengan relief standard
> - Nilai nisab zakat berbeza mengikut negeri dan masa — semak dengan pihak berkuasa zakat negeri anda
>
> Untuk pengiraan cukai yang muktamad, sila rujuk **Lembaga Hasil Dalam Negeri Malaysia (LHDN)** di [hasil.gov.my](https://www.hasil.gov.my) atau berunding dengan pengamal cukai berlesen.

---

## Lesen

©  M.D.Hadi. Hak cipta terpelihara.

---

*Dibina dengan ❤️ untuk komuniti pekerja Malaysia*
