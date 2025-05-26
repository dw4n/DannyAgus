---
title: "Kenapa README.md Tidak Muncul di NuGet? Refleksi dari Sebuah Paket Internal"
title_underscore: "readme_nuget_internal_refleksi"
summary: Pengalaman langsung mengeksplorasi penyebab README.md tidak muncul di tampilan NuGet, yang ternyata bukan berasal dari kesalahan teknis, melainkan keterbatasan feed internal Azure DevOps.
date: 2025-05-26T20:25:00+08:00
tags: ["nuget", "azure devops", "readme.md", "internal feed", "pipeline", "dotnet", "AI"]
categories: ["blog"]
draft: false
---

> #### Outline Artikel
> 1. [Pendahuluan](#pendahuluan)
> 2. [Eksperimen awal dan bantuan AI](#eksperimen-awal-dan-bantuan-ai)
> 3. [AI to the rescue — but not quite](#ai-to-the-rescue)
> 4. [Penyadaran tentang keterbatasan platform internal](#penyadaran-platform-internal)
> 5. [Kesimpulan dan pelajaran teknis](#kesimpulan)

---

<a id="pendahuluan"></a>

***Awalnya saya mengira ada kesalahan dalam cara saya mengemas file `README.md` ke dalam paket NuGet yang sedang saya kembangkan.*** Seperti banyak pengembang lainnya, saya berharap dokumentasi README akan muncul secara otomatis di tampilan Visual Studio atau halaman NuGet Package Manager. Saya sudah mengikuti dokumentasi resmi NuGet, menggunakan properti `PackageReadmeFile`, dan menyertakan file `README.md` ke dalam `.csproj`. Namun, setelah pipeline DevOps saya berjalan dan paket ter-push ke feed, tab README yang saya harapkan tidak pernah muncul.

<a id="eksperimen-awal-dan-bantuan-ai"></a>

***Saya pun mulai bereksperimen untuk mencari tahu penyebabnya, berbekal dokumentasi dan bantuan AI.*** Saya membuat dua versi pipeline: satu untuk test (tanpa push) dan satu untuk produksi (dengan push ke feed internal). Dengan rasa penasaran tinggi, saya memeriksa file `.nupkg` hasil pipeline test, dan benar saja — `README.md` dan tag `<readme>` muncul dengan sempurna di `.nuspec`. Tapi begitu saya lihat paket dari pipeline produksi, README tidak muncul di UI. Dalam proses ini saya juga melibatkan AI, yang membantu meninjau isi `.nupkg` dan metadata-nya, sekaligus menyarankan agar saya memastikan SDK yang digunakan sudah mendukung `PackageReadmeFile`.

<a id="ai-to-the-rescue"></a>

***AI to the rescue, but not quite.*** Meski AI memberikan bantuan analitis yang sangat membantu, ia sempat salah menilai isi `.nupkg`. AI menyatakan bahwa tag `<readme>` tidak ada, padahal ketika saya buka sendiri isi `.nupkg` dengan tool ZIP, metadata itu ada. Ini menjadi pengingat yang menarik: AI memang bisa mempercepat investigasi, tapi hasilnya tetap perlu diverifikasi secara manual. Dokumentasi tetap relevan, dan pemahaman manusia terhadap konteks sistem masih krusial — terutama saat berurusan dengan format file yang tidak transparan.

<a id="penyadaran-platform-internal"></a>

***Puncaknya adalah ketika saya menyadari: masalahnya bukan pada file, bukan pada pipeline, tapi pada platform tempat saya mempublikasikannya.*** Feed yang saya gunakan adalah internal feed dari Azure DevOps Artifacts. Meskipun feed ini mendukung metadata seperti `<readme>`, UI-nya — baik di web DevOps maupun Visual Studio — belum mendukung tampilan README. Akhirnya, saya menyadari bahwa README saya sebenarnya sudah valid dan tersemat, hanya saja tidak ditampilkan karena platform tersebut belum menyediakan fitur visualisasi README layaknya NuGet.org.

<a id="kesimpulan"></a>

***Pengalaman ini memberi saya pelajaran penting: tidak semua masalah teknis berasal dari kesalahan konfigurasi; kadang, masalah muncul dari asumsi terhadap sistem yang kita pakai.*** Menguasai alat bantu seperti AI dan dokumentasi sangat penting, namun pemahaman mendalam terhadap platform tempat kita bekerja jauh lebih menentukan. README tidak muncul bukan berarti ada yang salah — bisa jadi platformnya memang tidak mendukungnya. Ini adalah pengingat bahwa dalam pengembangan perangkat lunak, refleksi dan pemahaman menyeluruh terhadap konteks sangat diperlukan — bahkan lebih dari sekadar mengikuti dokumentasi.

