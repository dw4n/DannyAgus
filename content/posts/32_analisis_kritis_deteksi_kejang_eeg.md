---
title: "Analisis Kritis dan Relevansi Metodologi Deteksi Kejang EEG dalam Studi Jothi & Jayaraj (2024) untuk Penelitian Klasifikasi Sinyal EEG Umum"
title_underscore: "analisis_kritis_deteksi_kejang_eeg"
summary: Artikel ini mengulas pendekatan CNN-RNN dengan attention mechanism dalam deteksi kejang EEG serta potensi adaptasinya untuk klasifikasi sinyal EEG lain.
date: 2025-05-27T21:10:00+08:00
tags: ["EEG", "deep learning", "CNN-RNN", "wavelet", "attention"]
categories: ["Paper"]
draft: false
---

> #### Outline Artikel  
> 1. [Pendahuluan](#pendahuluan)  
> 2. [Metodologi dan Tahapan Prapemrosesan](#metodologi-dan-prapemrosesan)  
> 3. [Arsitektur Model Hibrida CNN-RNN dengan Atensi](#arsitektur-model-hibrida)  
> 4. [Kinerja dan Relevansi untuk Tugas Klasifikasi EEG Umum](#relevansi-dan-adaptasi)  
> 5. [Kesimpulan dan Implikasi Penelitian Lanjutan](#kesimpulan)

<a id="pendahuluan"></a>

**Penelitian Jothi & Jayaraj (2024) menawarkan pendekatan terintegrasi dalam mendeteksi kejang epilepsi melalui pemrosesan sinyal EEG dengan kombinasi model CNN-RNN dan attention mechanism.** Artikel tersebut mengembangkan model yang memadukan Convolutional Neural Networks (CNN), Recurrent Neural Networks (RNN)—termasuk LSTM dan GRU—serta attention mechanism, dengan dukungan teknik Explainable AI (XAI). Tujuan utama pendekatan ini adalah mengoptimalkan deteksi kejang dengan meningkatkan akurasi, sensitivitas, dan spesifisitas. Kajian ini bertujuan untuk mengevaluasi pendekatan tersebut secara kritis, menyoroti fondasi metodologinya, serta mengeksplorasi potensi penerapannya dalam konteks klasifikasi EEG yang lebih luas di luar deteksi kejang.

<a id="metodologi-dan-prapemrosesan"></a>

**Landasan metodologis dari studi ini bertumpu pada tahapan prapemrosesan EEG yang cermat, yang dirancang untuk memaksimalkan kualitas sinyal sebelum dianalisis oleh model pembelajaran mendalam.** Penelitian ini menggunakan dataset Bonn yang populer dalam studi EEG, dan menerapkan teknik prapemrosesan penting seperti noise reduction—kemungkinan dengan bandpass filters—dan segmentasi sinyal menjadi *epoch*. Normalisasi data dengan metode seperti z-score juga digunakan untuk menyamakan skala antar segmen. Yang paling menonjol adalah penggunaan Wavelet Transform, yang memungkinkan dekomposisi sinyal ke dalam berbagai pita frekuensi dengan tetap mempertahankan dimensi waktu. Ini memungkinkan ekstraksi fitur multi-skala yang penting dalam menangkap variasi sinyal EEG pada resolusi berbeda.

<a id="arsitektur-model-hibrida"></a>

**Kontribusi utama penelitian ini terletak pada arsitektur model hibrida yang memanfaatkan kekuatan CNN, RNN, dan attention mechanism secara sinergis, serta transparansi model melalui XAI.** CNN digunakan untuk mengekstrak fitur spasial dari sinyal EEG yang mencerminkan pola aktivitas otak, sedangkan RNN, terutama dalam bentuk BiLSTM dan GRU, digunakan untuk menangkap hubungan temporal dalam sekuens EEG. attention mechanism, termasuk self-attention, diintegrasikan untuk menyoroti bagian sinyal yang paling relevan bagi klasifikasi. Untuk meningkatkan interpretabilitas model, teknik XAI seperti SHAP, LIME, dan saliency maps digunakan agar proses pengambilan keputusan model dapat dipahami. Proses akhir melibatkan fusi fitur dari berbagai sumber sebelum masuk ke tahap klasifikasi.

<a id="relevansi-dan-adaptasi"></a>

**Model hibrida ini menunjukkan performa tinggi dalam deteksi kejang dan menyajikan kerangka kerja yang relevan untuk tugas klasifikasi EEG lainnya, termasuk yang bersifat non-patologis.** Dalam uji coba, model mencapai akurasi 95.2%, sensitivitas 94.1%, dan spesifisitas 96.5%. Meskipun studi ini lebih berfokus pada kejang epilepsi, arsitekturnya cukup fleksibel untuk diterapkan pada klasifikasi kondisi otak seperti mata terbuka atau tertutup. Untuk kasus tersebut, alternatif seperti Independent Component Analysis (ICA) mungkin lebih efektif untuk mengatasi artefak spesifik seperti kedipan mata. Wavelet Transform tetap relevan sebagai alat ekstraksi fitur, tetapi kombinasi teknik prapemrosesan dapat disesuaikan dengan karakteristik sinyal dan jenis klasifikasi. Perbedaan tujuan klasifikasi juga akan memengaruhi fitur yang dianggap penting dan attention mechanism yang digunakan.

{{< mermaid >}}
graph TD
    A[Raw EEG Signals] --> B(Preprocessing & Noise Reduction <br> e.g., Filters, ICA, Wavelet Transform);
    B --> C{Feature Extraction};
    C -- Spatial Features --> D[CNN];
    C -- Temporal Dynamics --> E[RNN <br> e.g., LSTM/GRU];
    D --> F[Feature Fusion];
    E --> F;
    F --> G[Classification];
    G --> H[Output <br> e.g., Seizure Detection, Brain State Classification];
{{< /mermaid >}}

<a id="kesimpulan"></a>

**Meskipun difokuskan pada deteksi kejang, studi ini membuka jalur baru bagi penerapan metodologi CNN-RNN dengan atensi dalam berbagai tugas klasifikasi EEG melalui pendekatan yang adaptif dan modular.** Efektivitas model hibrida CNN-RNN yang ditunjukkan oleh Jothi dan Jayaraj memberikan dasar kuat untuk mengeksplorasi arsitektur serupa dalam konteks yang lebih luas. Pendekatan ini tidak hanya mampu mengenali pola abnormal seperti kejang, tetapi juga memiliki potensi untuk mendeteksi kondisi otak yang lebih subtil dan stabil. Penerapan prapemrosesan yang solid, termasuk kemungkinan kombinasi ICA dan Wavelet Transform, menjadi aspek krusial yang menentukan keberhasilan klasifikasi sinyal.

**Manfaat teknik multi-skala dan attention mechanism tidak hanya meningkatkan performa, tetapi juga memungkinkan model lebih selektif dalam memproses informasi yang relevan.** Dalam konteks klasifikasi EEG, kebutuhan akan interpretabilitas menjadi semakin penting, terutama ketika model digunakan dalam pengambilan keputusan klinis. Oleh karena itu, integrasi Explainable AI (XAI) bukan sekadar tambahan teknis, melainkan komponen esensial dalam desain sistem berbasis pembelajaran mendalam yang dapat dipercaya.

**Studi ini juga menggarisbawahi pentingnya validasi lebih lanjut pada dataset dan domain aplikasi yang lebih beragam.** Keberhasilan model dalam satu domain, seperti deteksi kejang, belum tentu langsung tertransfer ke domain lain tanpa penyesuaian metodologis. Ini membuka ruang eksplorasi bagi peneliti untuk menguji ulang pendekatan serupa dalam klasifikasi seperti beban kognitif, status kesadaran, atau respon terhadap stimulus eksternal menggunakan sinyal EEG. Pendekatan serial atau paralel dalam penggabungan CNN dan RNN, serta posisi relatif dari modul prapemrosesan, menjadi aspek teknis penting yang layak diinvestigasi lebih lanjut.

**Dengan demikian, penelitian ini memberikan kontribusi yang bukan hanya teknis, tetapi juga konseptual terhadap pengembangan sistem klasifikasi EEG berbasis pembelajaran mendalam yang lebih efektif dan dapat diandalkan.** Peneliti selanjutnya disarankan untuk membangun di atas fondasi ini, dengan memperluas cakupan aplikasi, menguji variasi arsitektur, serta mengevaluasi berbagai konfigurasi prapemrosesan dan teknik interpretabilitas untuk menjawab kebutuhan nyata dalam dunia klinis maupun eksperimental.

> Sumber: Jothi, B., & Jayaraj, D. (2024). *The Role of CNN-RNN Hybrid Models and Attention Mechanisms in EEG Signal Recognition for Correct Seizure Detection.* SEEJPH, XXV. ISSN: 2197-5248.
