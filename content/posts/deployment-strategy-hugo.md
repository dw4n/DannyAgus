
---
title: "Rencana Deployment Blog: Pertimbangan Teknis dan Biaya"
date: 2025-05-02
tags: ["hugo", "azure", "blog", "deployment", "biaya"]
categories: ["Teknologi"]
draft: false
---

Dalam proses membangun blog ini, kami tidak hanya membahas tampilan dan fitur, tapi juga melakukan perencanaan matang soal **deployment** dan **biaya operasional** jangka panjang. Blog ini dibangun dengan [Hugo](https://gohugo.io) dan menggunakan tema [PaperMod](https://github.com/adityatelange/hugo-PaperMod). Berikut ini adalah rangkuman pertimbangan kami.

---

## 🧱 Arsitektur Awal

### Komponen:
- **Konten:** Markdown (`.md`) yang dikonversi ke HTML dengan Hugo
- **Tema:** PaperMod
- **Hosting:** Azure Static Web Apps (Free Tier)
- **Gambar:** disimpan di dalam folder Hugo (`static/images/`)

### Perkiraan Awal:
- 1 post = 1 Markdown + gambar = **±1 MB total**
- Misalnya ada 100 post = **~100 MB** total konten

---

## 💡 Evaluasi Kebutuhan dan Risiko

### Masalah yang Muncul:
- **GitHub Repo Limit:** GitHub membatasi size repo, dan ada limit per file (100MB)
- **Static Web Apps Limit:** Free tier hanya mendukung **250 MB** total size
- **Images Berat:** Post galeri foto bisa >10MB per post
- **CI/CD GitHub Actions:** meski gratis 2000 menit/bulan, tetap terbatas kalau repo makin besar

---

## ✅ Strategi Final yang Diambil

### 1. **Konten Markdown dan Hugo HTML tetap di Static Web App**
- Kecil (5–10 KB per post)
- Cepat dibuild
- Gratis deploy dengan GitHub Actions

### 2. **Gambar dipisah ke Azure Blob Storage**
- Contoh link: `https://namastorage.blob.core.windows.net/images/post-a/foto1.jpg`
- Gambar tidak memakan space GitHub maupun Static App

---

## 🔢 Simulasi Perkiraan Biaya

| Kasus                      | Jumlah Post | Size per Post | Total Size | Biaya GitHub | Biaya Azure Storage | Biaya Static App |
|---------------------------|-------------|----------------|-------------|----------------|---------------------|------------------|
| Awal (Markdown + Image)   | 10          | 1 MB           | 10 MB       | 0              | 0                   | 0                |
| Optimal (Markdown only)   | 100         | 10 KB          | 1 MB        | 0              | ±1 GB Image = Rp1.000–2.000/bln | 0                |
| Skala Besar (>250 MB HTML)| 500         | 10 KB          | 5 MB        | Mungkin perlu LFS | ±5 GB Image = Rp5.000–10.000/bln | Butuh Tier Berbayar |

---

## 📦 Perbandingan Hosting Hugo

| Opsi                  | Keunggulan                             | Kekurangan                            | Biaya                 |
|-----------------------|-----------------------------------------|----------------------------------------|------------------------|
| **Azure Static Web (Free)** | Auto SSL, CI/CD, ringan              | Limit 250MB total                      | Gratis                 |
| **Azure Blob Static Website** | Bisa simpan ratusan GB             | Manual deploy/CI perlu setup           | ±Rp1.000–2.000/GB/bulan|
| **Azure Static Web (Standard)** | SLA, staging, domain lebih banyak | Biaya tetap meski trafik kecil         | ±Rp140.000/bln         |
| **GitHub Pages**      | Gratis, gampang untuk markdown blog     | Gambar besar tetap bermasalah          | Gratis                 |
| **VPS / WordPress**   | Bebas kontrol, plugin, database         | Harus urus security, patching, dll     | ±Rp50.000–200.000/bln  |

---

## 🧭 Kapan Harus Upgrade?

- **Sudah >250MB konten HTML**: Pertimbangkan Standard Tier atau pindah ke Blob
- **Sudah >1GB gambar dan traffic tinggi**: Pasang Azure CDN atau pindah ke hosting khusus
- **Butuh login, form, backend**: Hugo tidak cukup — bisa pindah ke headless CMS atau SSG + API
- **Repo GitHub makin besar**: Pisahkan gambar ke Blob, pakai LFS kalau perlu

---

## ✍️ Penutup

Untuk saat ini, setup ini sangat efisien dan scalable:
- Markdown kecil → Static Web App
- Gambar → Blob Storage
- Biaya hampir **nol**, dan bisa **naik bertahap tanpa ganti platform**

Kalau kamu memulai blog atau galeri pribadi, strategi ini layak dipertimbangkan sebelum pakai solusi besar yang mahal dari awal.

> Hemat bukan berarti murahan — kadang justru efisien itu yang profesional.
