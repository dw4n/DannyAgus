---
title: "Pertimbangan Teknis dan Biaya Pembentukan Blog Ini"
date: 2025-05-02T14:00:00+08:00
tags: ["hugo", "azure", "blog", "deployment", "biaya"]
categories: ["Blog"]
draft: false
---

---
> #### Outline Artikel
> 1. [Arsitektur Awal](#arsitektur-awal)
> 2. [Evaluasi Kebutuhan dan Potensi Masalah](#evaluasi)
> 3. [Strategi yang Saya Ambil](#strategi)
> 4. [Simulasi Biaya](#biaya)
> 5. [Perbandingan Alternatif Hosting](#alternatif-hosting)
> 6. [Kapan Sebaiknya Upgrade?](#upgrade)
> 7. [Kesimpulan](#kesimpulan)
---

<span id="arsitektur-awal"></span>

### Arsitektur Awal

Untuk versi awal, saya memilih arsitektur yang sederhana namun cukup scalable.

**Komponen utama:**

- Konten ditulis dalam Markdown (`.md`) dan dikonversi ke HTML menggunakan Hugo.
- Menggunakan tema PaperMod.
- Dihosting di Azure Static Web Apps (paket gratis).
- Gambar disimpan langsung di dalam folder Hugo (`static/images/`).

**Estimasi awal kebutuhan:**

- 1 post = ±1 MB (Markdown + gambar)
- Jika ada 100 post = sekitar 100 MB total konten

Untuk tahap awal, semua ini terasa cukup efisien dan praktis.

<span id="evaluasi"></span>

### Evaluasi Kebutuhan dan Potensi Masalah

Namun, seiring pertumbuhan konten, beberapa batasan mulai muncul:

- **GitHub Repository** punya batasan ukuran file (100 MB per file) dan total repo.
- **Azure Static Web Apps Free Tier** hanya mendukung maksimal **250 MB** konten.
- **Gambar** bisa membengkak ukurannya, terutama jika ada post bergaya galeri.
- **GitHub Actions** hanya gratis hingga 2000 menit/bulan — cukup, tapi bisa jadi hambatan saat build makin besar.

Dengan mempertimbangkan risiko-risiko ini, saya mulai memikirkan strategi yang lebih efisien untuk jangka panjang.

<span id="strategi"></span>

### Strategi yang Saya Ambil

Solusinya: pisahkan konten dan aset berat.

1. **Markdown dan hasil build HTML tetap di Azure Static Web Apps**  
   File ini kecil (5–10 KB per post), build-nya cepat, dan gratis dideploy melalui GitHub Actions.

2. **Gambar dipindahkan ke Azure Blob Storage**  
   Dengan cara ini, ukuran repo GitHub tetap kecil dan storage bisa tumbuh secara terpisah. Contoh link gambar:  
   `https://namastorage.blob.core.windows.net/images/post-a/foto1.jpg`

Strategi ini memberikan fleksibilitas lebih besar dan memisahkan antara “konten ringan” dan “aset berat”.

<span id="biaya"></span>

### Simulasi Biaya

| Kasus                      | Jumlah Post | Size per Post | Total Size | GitHub | Azure Storage             | Static App         |
|---------------------------|-------------|----------------|-------------|--------|----------------------------|---------------------|
| Awal (Markdown + Gambar)  | 10          | 1 MB           | 10 MB       | Gratis | Gratis                    | Gratis              |
| Optimal (Markdown saja)   | 100         | 10 KB          | 1 MB        | Gratis | ±1 GB gambar = Rp1.000–2.000/bln | Gratis              |
| Skala Besar (>250MB HTML) | 500         | 10 KB          | 5 MB        | Perlu LFS | ±5 GB gambar = Rp5.000–10.000/bln | Upgrade ke tier berbayar |

<span id="alternatif-hosting"></span>

### Perbandingan Alternatif Hosting

Berikut beberapa opsi hosting yang saya pertimbangkan:

| Opsi                        | Keunggulan                            | Kekurangan                            | Estimasi Biaya           |
|-----------------------------|----------------------------------------|----------------------------------------|---------------------------|
| **Azure Static Web (Free)** | Auto SSL, CI/CD, cepat dan ringan     | Batas 250MB total konten               | Gratis                    |
| **Azure Blob Static Site**  | Bisa simpan ratusan GB aset            | Deploy manual atau setup CI sendiri    | ±Rp1.000–2.000/GB/bulan   |
| **Azure Static Web (Standard)** | SLA, staging slot, custom domain banyak | Bayar tetap meski trafik kecil         | ±Rp140.000/bulan          |
| **GitHub Pages**            | Sangat cocok untuk markdown blog       | Tidak ideal untuk gambar besar         | Gratis                    |
| **VPS / WordPress**         | Bebas kontrol, plugin, database        | Perlu urus patching, keamanan, dsb     | ±Rp50.000–200.000/bulan   |

<span id="upgrade"></span>

### Kapan Sebaiknya Upgrade?

Berikut beberapa tanda bahwa sudah waktunya mempertimbangkan opsi lebih serius:

- Konten HTML melebihi 250 MB → pertimbangkan upgrade ke Azure Standard atau pindah ke Blob.
- Gambar makin besar dan trafik tinggi → aktifkan Azure CDN atau cari hosting gambar khusus.
- Mulai butuh backend seperti form, login, atau database → Hugo tidak cukup. Pertimbangkan headless CMS atau gabungan SSG + API.
- Repo GitHub makin berat → pisahkan aset ke Blob atau aktifkan Git LFS.

<span id="kesimpulan"></span>

### Kesimpulan

Untuk saat ini, setup yang saya gunakan sangat efisien:

- **Markdown** tetap ringan dan cepat di-deploy di Static Web App  
- **Gambar** ditangani terpisah via Blob Storage  
- **Biaya** bisa mendekati nol dan tumbuh secara bertahap tanpa migrasi platform besar-besaran

Jika kamu sedang membangun blog pribadi atau proyek kecil, strategi ini layak dipertimbangkan sebelum langsung loncat ke solusi besar yang mahal.

> Hemat bukan berarti murahan. Justru, solusi yang efisien dan tepat sasaran bisa jadi lebih profesional.
