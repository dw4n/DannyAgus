---
title: "Thinking Frameworks Case Study: Mengurai Masalah Data Hilang di Sistem Microservices"
title_underscore: "thinking_frameworks_case_study_data_hilang"
summary: Studi kasus nyata penggunaan kerangka berpikir sistemik untuk menyelidiki dan mengelola masalah kehilangan data dalam sistem mikroservis cloud pendidikan.
date: 2025-05-26T23:19:00+08:00
tags: ["microservices", "data integrity", "problem solving", "framework", "resilience thinking"]
categories: ["Thinking Framework"]
draft: false
---

> #### Outline Artikel  
> 1. [Pendahuluan Reflektif](#pendahuluan)  
> 2. [Memilih Framework: IDEAL & Resilience Thinking](#memilih-framework)  
> 3. [Kronologi Kasus: Data Hilang Tanpa Jejak](#kronologi)  
> 4. [Bagaimana Framework Membantu](#framework-dalam-praktik)  
> 5. [Takeaway: Apa yang Bisa Dipelajari](#takeaway)  
> 6. [Penutup: Menuju Praktik Berpikir yang Lebih Matang](#penutup)

<a id="pendahuluan"></a>  

***Pernahkah Anda diminta menyelidiki hilangnya data di sistem produksi, padahal semua service terlihat “baik-baik saja”?*** Tidak ada error mencolok, tidak ada notifikasi gagal, dan sistem monitoring menampilkan status “green”. Lalu muncul laporan: *data siswa hilang, laporan tidak lengkap*. Dalam dunia sistem mikroservis, ini bukan skenario fiksi — ini kenyataan sehari-hari.

<a id="memilih-framework"></a>  

***Untuk menghadapi situasi seperti ini, kami menggunakan dua pendekatan: IDEAL Problem-Solving dan Resilience Thinking.*** IDEAL membantu memetakan langkah sistematis dalam menyelidiki masalah, sedangkan Resilience Thinking membantu menggeser fokus dari “menemukan penyebab” ke “menjamin ketahanan dan pemulihan sistem”. Ini bukan soal siapa yang salah, tetapi bagaimana sistem berperilaku saat gagal — dan apa yang bisa dilakukan saat kita tidak tahu di mana letak kegagalannya.

<a id="kronologi"></a>  

***Kasus ini terjadi pada platform edukasi berbasis cloud, dengan arsitektur mikroservis yang kompleks.*** Ada service *course* yang mencatat aktivitas siswa, *report* yang mengagregasi data, serta *forum*, *video conference*, dan event-event lain yang masuk melalui Event Hub. Di atas kertas, semua sistem saling mendengar dan mencatat. Tapi kenyataannya, beberapa data siswa tidak pernah muncul di laporan. Setelah investigasi panjang, ditemukan bahwa ada logika kode yang diam-diam mengabaikan error: `if error then do nothing`. Parahnya, tidak semua consumer di Event Hub dilengkapi retry logic atau logging. Namun, ketika dibuat secondary consumer group dan dicatat ke DB, semua event ternyata masuk — artinya bukan masalah infrastruktur. Karena tidak ada root cause tunggal, maka pendekatannya berubah: setiap kali data hilang terdeteksi, sistem melakukan revalidasi dan rekonstruksi data dari service lain.

<a id="framework-dalam-praktik"></a>  

***Framework IDEAL digunakan untuk menjaga struktur berpikir selama investigasi, terutama dalam situasi yang penuh spekulasi dan tekanan waktu.*** Kami mulai dengan mengidentifikasi masalah (Identify): data tidak muncul di laporan. Kemudian kami mendefinisikan ruang lingkupnya (Define): data yang hilang berasal dari event tertentu dan tidak semua siswa terdampak. Selanjutnya, kami menjelajahi strategi (Explore): tracing log, membuat secondary consumer, dan membandingkan behavior antar environment. Kami bertindak (Act): mengembangkan script rekonsiliasi otomatis, lalu meninjau ulang (Look back): bahwa error silent memang tidak bisa dideteksi dari luar. IDEAL menjaga tim agar tetap fokus dan tidak terjebak di asumsi awal.

***Sementara itu, Resilience Thinking mendorong tim untuk berhenti mencari “kesempurnaan sistem” dan mulai membangun sistem yang siap menerima kegagalan.*** Kami mulai menyusun ulang mindset: bahwa sistem tidak harus selalu benar, tapi harus selalu bisa pulih. Ini memunculkan keputusan strategis: menambahkan fallback logic, logging tambahan, dan “missing-data recovery path” sebagai bagian dari arsitektur. Bukan solusi final, tapi jaring pengaman yang membuat sistem lebih tahan terhadap anomali di masa depan.

<a id="takeaway"></a>  

***Takeaway utama dari kasus ini bukan “perbaiki bug-nya” — karena bug-nya tersembunyi atau bahkan tidak bisa dibuktikan.*** Pelajaran pentingnya: siapkan sistem untuk gagal, dan rancang mekanisme pemulihan (fallback & reconciliation) sebagai standar, bukan reaksi. Framework seperti IDEAL membantu menjaga objektivitas langkah demi langkah, sementara Resilience Thinking membantu menjaga mentalitas engineering tetap solutif meskipun tidak ada jawaban pasti.

<a id="penutup"></a>  

***Berpikir struktural seperti ini bukan hanya soal menyelesaikan masalah — tetapi membentuk cara kerja profesional.*** Dalam proyek berikutnya, kami bahkan mulai merancang sistem bukan dari alur sukses, tapi dari skenario *bagaimana jika data tidak pernah sampai*. Ini bukan paranoia, tapi kematangan berpikir sebagai praktisi teknologi.