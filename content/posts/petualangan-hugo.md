---
title: "Petualangan Membuat Blog dengan Hugo + Azure Static Web"
date: 2025-05-01T11:00:00+08:00
tags: ["hugo", "azure", "static site", "papermod"]
categories: ["Teknologi"]
draft: false
---
Semuanya bermula dari keinginan sederhana: bikin blog pribadi yang ringan, bisa ditulis pakai Markdown, dan gampang di-*upload*. Tapi siapa sangka, dari situ berkembang jadi sebuah mini-proyek dengan CI/CD, search engine, tagging, dan tampil kece berkat PaperMod.

Sebagai konteks, sebelumnya saya punya blog yang jarang di update. Menggunakan Wordpress + PHP server dari azure. Saya cuma pakai 3 plugin, jet something itu untuk SEO, security, dan ada satu untuk moderator comment. Template pun template bawaan. Karena saya jarang update, setiap 6 bulan sekali kena virus, dan saya harus restore ke previous backup. Proses restore pun agak menyebalkan, karena harus proses delete dan reupload ftp.

## 1. Ide Awal

Tujuan utama blog ini:

- Sesimple mungkin
- Bisa baca atau nulis pakai `.md`
- Bisa upload gambar sesuai post (misal: `/images/judul-post/`)
- Bisa di-*deploy* ke Azure (karena udah ada akun gratis)
- Bisa upload lewat FTP/manual kalau perlu

## 2. Pemilihan Stack

Setelah diskusi (dengan Chat GPT), dan berdasarkan pengalaman Wordpress sebelumnya, akhirnya dipilih:

- **Hugo** sebagai Static Site Generator (SSG)
- **PaperMod** sebagai tema
- **Azure Static Web Apps** untuk hosting
- **GitHub Actions** untuk auto deploy (CI/CD)

Kenapa Hugo?

- Super cepat
- Native support Markdown
- Outputnya HTML statis, cocok banget buat Azure

Sebenernya niat awal buat sendiri, menggunakan html, css, dan js. Tapi kalau sudah ada yang sesuai keinginan, why not?.

## 3. Setup Hugo Lokal (Tanpa Install)

Download Hugo Extended ZIP dari GitHub:

- [https://github.com/gohugoio/hugo/releases](https://github.com/gohugoio/hugo/releases)

Ekstrak `hugo.exe` ke folder mana saja. Jalankan langsung dari terminal tanpa masuk PATH.

```bash
.\hugo.exe new site blog-saya
cd blog-saya
```

**figure: `.\hugo.exe` dijalankan langsung tanpa instalasi**

## 4. Menambahkan Tema PaperMod

Download PaperMod dari [https://github.com/adityatelange/hugo-PaperMod](https://github.com/adityatelange/hugo-PaperMod)

Ekstrak dan taruh di:

```text
blog-saya/themes/PaperMod/
```

Lalu edit `config.toml`:

```toml
baseURL = "https://namablog.azurestaticapps.net/" #atau localhost
languageCode = "id"
title = "Blog Saya"
theme = "PaperMod"

[params]
  homeInfoParams = { Title = "Selamat datang", Content = "Blog ini ditulis dengan Markdown dan dibangun dengan Hugo + Azure." }

[outputs]
  home = ["HTML", "JSON"]

[params]
  ShowSearch = true
  ShowReadingTime = true
  ShowShareButtons = true
```

## 5. Menulis Post Pertama

```bash
.\hugo.exe new posts/halo-dunia.md
```

Isi Markdown:

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

Taruh gambar kamu di:

```text
static/images/halo-dunia/foto1.jpg
```

## 6. Menjalankan Lokal

```bash
.\hugo.exe server
```

Buka [http://localhost:1313](http://localhost:1313) untuk lihat hasilnya.

**image: hasil tampilan blog lokal di browser**

## 7. Menyiapkan Deploy ke Azure

1. Buat repository GitHub
2. Push semua isi project Hugo
3. Tambahkan folder `.github/workflows/azure.yml`:

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

## 8. Fitur Search, Tag, Categories

- `index.json` dibuat otomatis oleh Hugo untuk search
- PaperMod pakai JavaScript (Fuse.js) untuk cari dari isi `index.json`
- Tags dan Categories diambil dari front matter masing-masing `.md`

{{< img src="https://stdwanprod.blob.core.windows.net/dannyaguspublic/img/202505/20250502092521-petualanganHugo-tampilanSearchTagsCategories.png" alt="Tampilan search, tags, categories" width="400" caption="Gambar 2: Tampilan navigasi search, tags, categories pada Hugo PaperMod." >}}

---

## 9. Menyisipkan Gambar dengan Caption

Salah satu kebutuhan umum saat menulis blog (terutama tutorial atau dokumentasi) adalah menyisipkan gambar dengan **caption**, seperti `Figure 1: ...`. PaperMod mendukung HTML dalam Markdown, tapi agar lebih praktis dan rapi, kita bisa membuat **shortcode custom bernama `img`**.

### 1️⃣ Menyimpan Gambar

Dalam kasus ini, gambar-gambar disimpan di Azure Blob Storage agar mudah diakses secara publik. Struktur dan pengaturannya akan dibahas lebih detail nanti di bagian tersendiri.

Contoh URL gambar:

```
https://stdwanprod.blob.core.windows.net/dannyaguspublic/img/202505/20250502092521-petualanganHugo-tampilanSearchTagsCategories.png
```

> Keuntungan menyimpan di storage: tidak membebani repo GitHub, dan mudah dikelola terpisah dari konten Markdown.

---

### 2️⃣ Menambahkan Shortcode `img`

Buat file di:

```
layouts/shortcodes/img.html
```

Isi file:

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

Shortcode ini:

- Menampilkan gambar dengan ukuran fleksibel dan responsif.
- Memposisikan caption di tengah, mirip gaya tesis atau laporan ilmiah.

---

### 3️⃣ Contoh Penggunaan

Di file Markdown (`.md`), gunakan seperti ini:

```md
{{< img src="https://stdwanprod.blob.core.windows.net/dannyaguspublic/img/202505/20250502092521-petualanganHugo-tampilanSearchTagsCategories.png" alt="Tampilan search, tags, categories" width="500" caption="Gambar 2: Tampilan navigasi search, tags, categories pada Hugo PaperMod." >}}
```

Hasilnya akan menampilkan gambar dan caption seperti pada bagian sebelumnya. Kamu juga bisa mengganti URL dengan gambar dari lokal (`/images/...`) jika tidak pakai Azure Storage.

---

## Penutup

Petualangan ini baru dimulai. Next:

- Memastikan semua fitur saat ini baik baik saja, terutama search.
- Menambahkan fitur multilingual
- Eksperimen dengan halaman khusus proyek & dokumentasi
- Berfikir mengenai deployment, apakah worth harganya?

> Semua ini dimulai dari `.md` sederhana. Tapi hasilnya? Sebuah blog modern, ringan, dan powerful.
