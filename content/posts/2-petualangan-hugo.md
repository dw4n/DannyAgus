---
title: "Dari WordPress ke Hugo: Petualangan Membangun Blog yang Aman, Murah, dan Tanpa Drama"
date: 2025-05-01T11:00:00+08:00
tags: ["hugo", "azure", "static site", "papermod"]
categories: ["Blog"]
draft: false
---

---
> #### Outline Artikel
> 1. [Pendahuluan: Masalah dengan WordPress](#pendahuluan)
> 2. [Tahap 1: Menentukan Arah dan Stack Teknologi](#tahap-1)
> 3. [Tahap 2: Setup Hugo Lokal Tanpa Install](#tahap-2)
> 4. [Tahap 3: Menambahkan Tema PaperMod](#tahap-3)
> 5. [Tahap 4: Menulis dan Menampilkan Postingan Pertama](#tahap-4)
> 6. [Tahap 5: Otomatisasi Deploy dengan GitHub Actions](#tahap-5)
> 7. [Tahap 6: Menambah Fitur Search, Tag, dan Kategori](#tahap-6)
> 8. [Tahap 7: Menyisipkan Gambar dengan Caption](#tahap-7)
> 9. [Penutup: Apa yang Saya Pelajari](#penutup)
---

<span id="pendahuluan"></span>

Bikin blog itu harusnya sederhana. Tapi pengalaman saya dengan WordPress justru sebaliknya: plugin harus rutin di-update, PHP di Azure berubah versi tanpa peringatan (PHP 7 ke 8 pernah bikin WordPress saya rusak), dan hampir setiap 3 bulan sekali, situs saya kena hack. Padahal sudah pasang plugin security (yang versi gratis tentu saja). Biaya server pun tidak murah. Saya jadi berpikir: *apa nggak ada solusi yang lebih simpel, murah, dan aman?*

Dari situlah petualangan ini dimulai. Saya menemukan Hugo — static site generator yang ringan dan cepat — dan memadukannya dengan Azure Static Web Apps. Yang tadinya hanya ingin blog sederhana, ternyata berkembang jadi mini-proyek dengan CI/CD, fitur pencarian, tagging, dan tampilan kece berkat PaperMod.

<span id="tahap-1"></span>

### Tahap 1: Menentukan Arah dan Stack Teknologi

Sebelum menyentuh kode, saya perlu memetakan kebutuhan. Tujuannya jelas:

- Blog sesimpel mungkin
- Bisa ditulis pakai `.md`
- Mendukung gambar sesuai posting (struktur `/images/nama-post/`)
- Bisa dideploy ke Azure
- Masih bisa manual lewat FTP kalau terpaksa

Setelah eksplorasi dan ngobrol dengan beberapa tools (termasuk ChatGPT), saya putuskan kombinasi berikut:

- Hugo untuk generatornya
- PaperMod untuk tema
- Azure Static Web Apps sebagai hosting
- GitHub Actions sebagai jalur deploy otomatis

<span id="tahap-2"></span>

### Tahap 2: Setup Hugo Lokal Tanpa Install

Langkah pertama adalah membuat fondasi blog-nya. Enaknya Hugo, kita bisa pakai executable tanpa harus instalasi penuh.

Unduh Hugo Extended dari:  
[https://github.com/gohugoio/hugo/releases](https://github.com/gohugoio/hugo/releases)

Setelah diekstrak, cukup jalankan:

```bash
.\hugo.exe new site blog-saya
cd blog-saya
```

Dengan ini, kita sudah punya struktur dasar proyek Hugo.

<span id="tahap-3"></span>

### Tahap 3: Menambahkan Tema PaperMod

Setelah struktur dasar siap, kita butuh tampilan yang enak dilihat. Pilihan saya jatuh ke PaperMod — tema clean, cepat, dan aktif dikembangkan.

Download dari:  
[https://github.com/adityatelange/hugo-PaperMod](https://github.com/adityatelange/hugo-PaperMod)

Taruh hasilnya di:

```text
blog-saya/themes/PaperMod/
```

Edit `config.toml`:

```toml
baseURL = "https://namablog.azurestaticapps.net/"
languageCode = "id"
title = "Blog Saya"
theme = "PaperMod"

[params]
  homeInfoParams = { Title = "Selamat datang", Content = "Blog ini ditulis dengan Markdown dan dibangun dengan Hugo + Azure." }
  ShowSearch = true
  ShowReadingTime = true
  ShowShareButtons = true

[outputs]
  home = ["HTML", "JSON"]
```

<span id="tahap-4"></span>

### Tahap 4: Menulis dan Menampilkan Postingan Pertama

Setelah konfigurasi dasar beres, saatnya mencoba menulis posting pertama.

```bash
.\hugo.exe new posts/halo-dunia.md
```

Isi Markdown-nya seperti ini:

```markdown
---
title: "Halo Dunia"
date: 2025-05-01
draft: false
categories: ["Umum"]
---

Ini adalah postingan pertama saya. Gambar bisa ditaruh di `/static/images/halo-dunia/`.

![Gambar ilustrasi](/images/halo-dunia/foto1.jpg)

figure: contoh penyisipan gambar di Markdown
```

Jangan lupa menaruh gambar kamu di:

```text
static/images/halo-dunia/foto1.jpg
```

Untuk melihat hasilnya:

```bash
.\hugo.exe server
```

Buka `http://localhost:1313`.

<span id="tahap-5"></span>

### Tahap 5: Otomatisasi Deploy dengan GitHub Actions

Blog sudah berjalan lokal. Sekarang kita ingin setiap perubahan otomatis di-deploy ke Azure.

1. Buat repository GitHub
2. Push isi blog
3. Tambahkan file workflow berikut ke `.github/workflows/azure.yml`:

```yaml
name: Azure Static Web Apps CI/CD

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
      - name: Build
        run: hugo --minify
      - name: Upload to Azure
        uses: Azure/static-web-apps-deploy@v1
        with:
          azure_static_web_apps_api_token: ${{ secrets.AZURE_TOKEN }}
          repo_token: ${{ secrets.GITHUB_TOKEN }}
          action: "upload"
          app_location: "public"
```

<span id="tahap-6"></span>

### Tahap 6: Menambah Fitur Search, Tag, dan Kategori

Agar blog lebih nyaman dijelajahi, saya aktifkan fitur pencarian dan kategorisasi.

- Hugo secara otomatis membuat `index.json` untuk search
- PaperMod menggunakan Fuse.js untuk pencarian berbasis JavaScript
- Tag dan kategori diambil dari front matter setiap postingan

<span id="tahap-7"></span>

### Tahap 7: Menyisipkan Gambar dengan Caption

Agar dokumentasi lebih rapi, saya buat shortcode `img` sendiri.

Buat file:

```
layouts/shortcodes/img.html
```

Isi:

```html
<figure style="display: flex; flex-direction: column; align-items: center; text-align: center; margin: 1.5em 0;">
  <img src="{{ .Get "src" }}" alt="{{ .Get "alt" }}" width="{{ .Get "width" }}" style="max-width: 100%; height: auto;" />
  {{ with .Get "caption" }}
    <figcaption style="font-size: 0.9em; color: #666; margin-top: 0.5rem;">
      {{ . }}
    </figcaption>
  {{ end }}
</figure>
```

Contoh penggunaan di Markdown:

```md
{{< img src="https://stdwanprod.blob.core.windows.net/dannyaguspublic/img/202505/20250502092521-petualanganHugo-tampilanSearchTagsCategories.png" alt="Tampilan search, tags, categories" width="500" caption="Gambar 2: Tampilan navigasi search, tags, categories pada Hugo PaperMod." >}}
```

<span id="penutup"></span>

### Penutup: Apa yang Saya Pelajari

Awalnya saya cuma ingin blog yang ringan dan nggak ribet. Tapi perjalanan ini justru membuka banyak hal:

- Belajar Hugo
- CI/CD dengan GitHub Actions
- Hosting gambar di Azure Blob
- Menyusun sistem dokumentasi yang modular

Semua dimulai dari file `.md`. Kini saya punya blog yang aman, ringan, dan sepenuhnya dalam kendali saya.

Selanjutnya? Saya ingin coba fitur multilingual, halaman khusus proyek, dan mungkin analisis biaya untuk jangka panjang.