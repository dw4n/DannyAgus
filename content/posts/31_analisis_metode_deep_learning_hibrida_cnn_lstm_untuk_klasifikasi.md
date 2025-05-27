---
title: "Analisis Metode Deep Learning Hibrida CNN-LSTM untuk Klasifikasi Aktivitas Manusia dan Potensinya pada Sinyal EEG"
title_underscore: "analisis_metode_deep_learning_hibrida_cnn_lstm_untuk_klasifikasi"
summary: Artikel ini mengulas metodologi deep learning hibrida Convolutional Neural Network (CNN) dan Long Short-Term Memory (LSTM) yang digunakan untuk pengenalan aktivitas manusia berdasarkan data sensor smartphone, serta mengeksplorasi relevansi dan potensi aplikasinya untuk klasifikasi sinyal Electroencephalogram (EEG).
date: 2025-05-27T21:00:00+08:00
tags: ["Deep Learning", "CNN", "LSTM", "EEG", "Klasifikasi Sinyal", "HAR"]
categories: ["Paper"]
draft: false
---

---
> #### Outline Artikel
>
> 1. [Pendahuluan: Pengenalan Aktivitas Manusia Melalui Deep Learning](#pendahuluan)
> 2. [Metodologi Inti: Arsitektur Hibrida CNN-LSTM](#metodologi-inti)
> 3. [Rasionalisasi dan Optimalisasi Kinerja Metode Hibrida](#rasionalisasi-metode)
> 4. [Potensi Aplikasi Metode CNN-LSTM pada Klasifikasi Sinyal EEG](#relevansi-eeg)
> 5. [Implikasi Pemrosesan Data dan Kesimpulan](#kesimpulan)
---

<a id="pendahuluan"></a>

***Studi terbaru menunjukkan keberhasilan signifikan dalam pengenalan aktivitas manusia (Human Activity Recognition/HAR) menggunakan data sensor ponsel pintar, dengan memanfaatkan arsitektur deep learning gabungan untuk mencapai akurasi klasifikasi yang tinggi.*** Penelitian oleh Ellouze et al. (2024) bertujuan menganalisis perilaku manusia berdasarkan pengenalan aktivitas fisik, yang hasilnya digunakan untuk mengembangkan sistem rekomendasi kesehatan. Data yang digunakan berasal dari dataset MMASH (Multi-level Monitoring Activity and Sleep on Healthy people), yang mencakup data akselerometer tri-aksial dari 22 partisipan beserta atribut pendukung lainnya seperti detak jantung, usia, berat, gender, dan jenis aktivitas. Fokus utama adalah bagaimana sistem dapat secara akurat mengklasifikasikan berbagai aktivitas fisik untuk memberikan umpan balik yang bermanfaat bagi pengguna terkait status kesehatan mereka.

<a id="metodologi-inti"></a>

***Inti dari keberhasilan metodologi yang diusulkan terletak pada penggunaan arsitektur deep learning hibrida yang mengkombinasikan Convolutional Neural Network (CNN) dan Long Short-Term Memory (LSTM), sebuah varian dari Recurrent Neural Network (RNN).*** Peneliti mengidentifikasi bahwa data aktivitas manusia mengandung informasi spasial dan temporal secara simultan. Oleh karena itu, diusulkan sebuah arsitektur yang mampu mengekstraksi kedua jenis informasi tersebut secara bersamaan untuk meningkatkan akurasi klasifikasi. Dalam model CNN-LSTM yang diajukan, lapisan CNN bertanggung jawab untuk mengekstraksi fitur-fitur spasial dari data sensor mentah, yang kemudian hasilnya diteruskan ke lapisan LSTM untuk mempelajari dependensi temporal dalam data. Struktur spesifik yang digunakan meliputi dua lapisan CNN dengan ukuran filter 64 dan 128 (kernel size 3, aktivasi ReLU), diikuti oleh lapisan max pooling (ukuran 2), yang outputnya kemudian diratakan dan dimasukkan ke dua lapisan LSTM (ukuran sel 64), dan diakhiri dengan lapisan dense dengan aktivasi softmax untuk klasifikasi.

Berikut adalah diagram konseptual alur kerja arsitektur CNN-LSTM yang diadaptasi dari deskripsi paper:

{{< mermaid >}}
graph TD
  A["Input: Data Sensor Time-Series"] --> B["Lapisan CNN"]
  B --> C["Lapisan Max Pooling"]
  C --> D["Flatten"]
  D --> E["LSTM"]
  E --> F["Fully Connected (Dense)"]
  F --> G["Output: Klasifikasi Aktivitas"]

  subgraph "CNN untuk Ekstraksi Fitur Spasial"
    B
    C
  end

  subgraph "LSTM untuk Pembelajaran Temporal"
    E
  end
{{< /mermaid >}}

<a id="rasionalisasi-metode"></a>

***Penggunaan arsitektur gabungan CNN-LSTM didasarkan pada pemahaman bahwa data aktivitas manusia dari sensor memiliki karakteristik spasial dan temporal yang kaya, dimana CNN unggul dalam menangkap pola lokal atau spasial sementara LSTM efektif dalam memodelkan urutan dan dependensi jangka panjang.*** Sementara CNN dirancang untuk informasi spasial dan RNN (termasuk LSTM) untuk data sekuensial, kombinasi keduanya memungkinkan pemanfaatan keunggulan masing-masing untuk tugas klasifikasi yang kompleks. Pendekatan ini terbukti menghasilkan akurasi yang superior; model convolutional LSTM, sebagai varian dari CNN-LSTM, mencapai akurasi tertinggi sebesar 96% pada klasifikasi independen. Lebih lanjut, untuk mengatasi ketidakpastian dan potensi kesalahan dalam data sensor, penelitian ini mengimplementasikan mekanisme fusi menggunakan teori Dempster-Shafer (DS) dan metode Voting. Kombinasi klasifikasi berdasarkan teori DS berhasil meningkatkan akurasi hingga 98%, menunjukkan kemampuannya dalam memodelkan data yang tidak akurat dan ambigu untuk pengambilan keputusan yang lebih reliabel.

<a id="relevansi-eeg"></a>

***Metodologi CNN-LSTM yang terbukti efektif untuk data sensor aktivitas manusia memiliki relevansi yang kuat untuk diterapkan pada klasifikasi sinyal Electroencephalogram (EEG).*** Sinyal EEG, serupa dengan data sensor gerak, secara inheren bersifat spatiotemporal. Aspek spasial pada EEG tecermin dari rekaman aktivitas listrik otak melalui berbagai elektroda yang tersebar di permukaan kulit kepala, di mana pola aktivitas antar elektroda pada satu waktu dapat ditangkap oleh CNN. Di sisi lain, aspek temporal terlihat dari dinamika sinyal EEG yang berubah seiring waktu, yang mana LSTM sangat cocok untuk memodelkan dependensi dan pola sekuensial tersebut. Kombinasi CNN untuk ekstraksi fitur spasial dari topografi elektroda dan LSTM untuk analisis dinamika temporal dari fitur-fitur tersebut berpotensi menghasilkan representasi fitur yang kaya dan diskriminatif untuk berbagai tugas klasifikasi berbasis EEG, seperti deteksi kondisi neurologis, antarmuka otak-komputer (BCI), atau analisis kondisi kognitif. Keberhasilan pendekatan serupa pada domain lain dengan data spatiotemporal memperkuat argumen ini.

<a id="kesimpulan"></a>

***Meskipun penelitian Ellouze et al. (2024) tidak secara eksplisit merinci langkah-langkah pra-pemrosesan data secara ekstensif seperti pembersihan data mendalam atau teknik penyeimbangan kelas seperti SMOTE, fokusnya pada kemampuan model deep learning untuk mempelajari fitur secara otomatis dari data mentah atau yang telah melalui segmentasi dasar tetap memberikan wawasan penting.*** Pendekatan yang memanfaatkan kekuatan CNN dalam ekstraksi fitur spasial dan LSTM dalam pemodelan dependensi temporal menawarkan kerangka kerja yang sangat adaptif. Untuk aplikasi pada sinyal EEG, meskipun langkah pra-pemrosesan spesifik domain EEG (seperti filtering, penghapusan artefak) akan tetap krusial, arsitektur inti CNN-LSTM seperti yang didemonstrasikan memiliki potensi besar untuk menghasilkan klasifikasi yang akurat dan bermakna. Dengan penyesuaian arsitektur dan parameter yang tepat, serta strategi penanganan ketidakpastian data jika diperlukan, metode ini dapat menjadi alat yang berharga dalam memajukan penelitian klasifikasi sinyal EEG menggunakan deep learning.

> Sumber : Ellouze, A., Kadri, N., Alaerjan, A., & Ksantini, M. (2024). Combined CNN-LSTM deep learning algorithms for recognizing human physical activities in large and distributed manners: A recommendation system. Computers, Materials & Continua, 79(1), 351–372. https://doi.org/10.32604/cmc.2024.048061