---
title: "Praktik Menentukan Sizing dan Velocity dalam Proyek Teknologi"
title_underscore: "praktik_menentukan_sizing_dan_velocity_dalam_proyek_teknologi"
summary: Berbagai pendekatan praktis estimasi pekerjaan dan kecepatan tim dalam proyek teknologi, mulai dari pengalaman sebelumnya hingga model estimasi relatif. Dilengkapi strategi menghadapi tenggat, pengaruh stack teknologi, serta pentingnya asumsi dan batasan backlog.
date: 2025-05-23T10:00:00+08:00
tags: ["agile", "estimasi", "sizing", "velocity", "proyek teknologi", "scrum", "manajemen proyek"]
categories: ["Manajemen Proyek"]
draft: false
---

---
> #### Outline Artikel
> 1. [Tantangan Estimasi dalam Proyek Teknologi](#tantangan-estimasi)
> 2. [Menggunakan Pengalaman Proyek Sebelumnya](#pengalaman-proyek)
> 3. [Model Estimasi Relatif dan Kapasitas Tim](#estimasi-relatif)
> 4. [Pengaruh Stack Teknologi terhadap Estimasi](#stack-teknologi)
> 5. [Strategi Saat Menghadapi Tenggat Ketat](#tenggat-ketat)
> 6. [Estimasi pada Proyek Non-CRUD](#proyek-non-crud)
> 7. [Pentingnya Asumsi dan Batasan dalam Backlog](#asumsi-batasan)
> 8. [Estimasi sebagai Kesepakatan Tim dan Scope](#estimasi-kesepakatan)
---

<span id="tantangan-estimasi"></span>

**Menentukan estimasi pekerjaan dalam proyek teknologi sering kali menghadirkan tantangan tersendiri**, terutama saat klien mengharapkan kepastian waktu dalam bentuk jam kerja, sementara tim justru mengandalkan pendekatan Agile yang bersifat iteratif dan berbasis empiris. Artikel ini membahas berbagai pendekatan praktis yang digunakan oleh tim dalam menentukan ukuran pekerjaan (*sizing*) dan kecepatan penyelesaian (*velocity*), termasuk cara mengelola asumsi dan batasan dalam backlog. Semua pembahasan bersumber dari pengalaman aktual yang terjadi di lapangan.

<span id="pengalaman-proyek"></span>

**Pengalaman dari proyek terdahulu menjadi salah satu titik tolak dalam membuat estimasi.** Jika proyek yang dikerjakan memiliki konteks yang serupa dengan proyek sebelumnya, maka dokumentasi dan kode lama dapat digunakan sebagai referensi awal. Pendekatan ini membantu efisiensi waktu, meskipun tetap memiliki risiko apabila terdapat perbedaan mendasar dalam requirement atau lingkungan teknis.

<span id="estimasi-relatif"></span>

**Model estimasi relatif juga digunakan untuk mengukur kompleksitas pekerjaan secara komparatif.** Dalam pendekatan ini, satu pekerjaan dijadikan acuan dasar, lalu pekerjaan lain diukur secara relatif terhadap acuan tersebut. Tingkat ketidakpastian dan asumsi harus dicatat agar estimasi tetap realistis. Selain itu, kecepatan tim dihitung berdasarkan kapasitas aktual—mengacu pada komposisi peran dalam tim dan jumlah backlog yang tersedia.

<span id="stack-teknologi"></span>

**Penyesuaian terhadap teknologi atau stack juga menjadi faktor penting dalam proses estimasi.** Misalnya, pekerjaan CRUD pada SQL mungkin bernilai lebih rendah dibandingkan dengan pekerjaan serupa pada NoSQL yang memiliki kompleksitas lebih tinggi. Dengan demikian, estimasi tidak hanya mempertimbangkan fungsionalitas, tetapi juga konteks teknis dan tools yang digunakan dalam implementasi.

<span id="tenggat-ketat"></span>

**Ketika proyek memiliki tenggat waktu yang ketat, strategi estimasi perlu disesuaikan dengan kapasitas tim.** Komposisi tim menjadi faktor awal untuk menentukan estimasi sprint. Bila diperlukan ekspansi jumlah anggota tim, pendekatan ini tetap harus dilakukan dengan hati-hati agar tidak menciptakan beban koordinasi yang berlebihan. Diberikan pula saran untuk menambahkan buffer waktu sebesar 10–15% sebagai antisipasi terhadap revisi atau pekerjaan tak terduga.

<span id="proyek-non-crud"></span>

**Dalam situasi proyek yang tidak berbasis CRUD, pendekatan estimasi harus dimulai dari pemetaan pekerjaan secara rinci.** Langkah awal biasanya dimulai dengan pembuatan proof of concept (POC), yang kemudian digunakan sebagai dasar untuk menurunkan pekerjaan menjadi unit-unit yang lebih jelas. Proses sizing kemudian dilakukan bersama analis yang memahami kebutuhan bisnis dan teknis.

<span id="asumsi-batasan"></span>

**Penetapan asumsi dan batasan sangat penting dalam penyusunan backlog proyek.** Asumsi mendefinisikan kondisi ideal yang dianggap tersedia, seperti akses data atau keterlibatan stakeholder. Sementara batasan menjelaskan ruang lingkup pekerjaan yang akan dan tidak akan dilakukan. Dokumen pendukung dan kesepakatan bersama dengan klien menjadi acuan utama untuk menjaga kejelasan dan kesepahaman.

<span id="estimasi-kesepakatan"></span>

**Estimasi dalam proyek bukan sekadar angka waktu yang ditebak di awal, tetapi kesepakatan atas ruang lingkup, pendekatan teknis, dan kapasitas tim.** Pengalaman tim dalam menerapkan berbagai metode ini menjadi fondasi penting dalam membangun sistem estimasi yang lebih matang. Dengan pemahaman ini, tim proyek dapat bergerak lebih percaya diri dan adaptif di tengah tantangan perubahan dan kompleksitas yang terus berkembang.

>Berdasarkan meeting notes dari Pertemuan Internal SME November 2024
