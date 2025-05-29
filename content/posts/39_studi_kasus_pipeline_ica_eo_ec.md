---
title: "Studi Kasus Mini: Pipeline ICA untuk Klasifikasi Kondisi Mata (EO/EC)"
title_underscore: "studi_kasus_pipeline_ica_eo_ec"
summary: Studi kasus ini membahas bagaimana ICA digunakan untuk membersihkan sinyal EEG dari dataset Kaggle guna meningkatkan akurasi klasifikasi kondisi mata terbuka dan tertutup.
date: 2025-05-30T00:30:00+08:00
tags: ["ICA", "EOEC", "Kaggle", "EEG", "klasifikasi", "pipeline"]
categories: ["Paper"]
draft: false
---

> #### Outline Artikel  
> 1. [Pendahuluan](#pendahuluan)  
> 2. [Dataset Kaggle: Eye State Classification](#dataset-kaggle-eye-state-classification)  
> 3. [Tahapan Pipeline dengan ICA](#tahapan-pipeline-dengan-ica)  
> 4. [Efek ICA terhadap Stabilitas Fitur](#efek-ica-terhadap-stabilitas-fitur)  
> 5. [Hasil Sementara: Akurasi Sebelum dan Sesudah ICA](#hasil-sementara-akurasi-sebelum-dan-sesudah-ica)  
> 6. [Refleksi dan Implikasi](#refleksi-dan-implikasi)

<a id="pendahuluan"></a>
***ICA tetap relevan bahkan pada dataset sederhana seperti Eye State EEG dari Kaggle, dan dapat memberikan dampak signifikan terhadap kualitas klasifikasi.*** Meskipun dataset ini hanya menggunakan satu perangkat consumer-grade (NeuroSky MindWave), keberadaan artefak seperti kedipan mata atau noise otot tetap dapat mengganggu sinyal. Studi kasus ini mengevaluasi peran ICA dalam membersihkan sinyal sebelum diekstrak fiturnya untuk klasifikasi mata terbuka (EO) dan tertutup (EC).

<a id="dataset-kaggle-eye-state-classification"></a>
***Dataset yang digunakan adalah Eye State Classification dari Kaggle, yang terdiri dari 14 kanal EEG dan label biner kondisi mata.*** Data direkam menggunakan Emotiv EPOC dengan sampling rate 128 Hz. Setiap baris mencerminkan 1 sampel waktu, dengan kolom terakhir sebagai label: 0 = mata terbuka, 1 = mata tertutup. Format data berupa CSV, mudah diproses dengan Python.

- Link dataset: [Kaggle - Eye State EEG Dataset](https://www.kaggle.com/datasets/robikscube/eye-state-classification-eeg-dataset)

<a id="tahapan-pipeline-dengan-ica"></a>
***Pipeline analisis terdiri dari langkah-langkah berikut:***
1. **Preprocessing:** normalisasi kanal, bandpass 1–45 Hz
2. **Epoching:** potong data ke dalam jendela 1 detik (128 sampel), overlap 50%
3. **ICA:** Gunakan FastICA dari `sklearn.decomposition` untuk memisahkan komponen
4. **Identifikasi artefak:** korelasi kanal frontal terhadap IC, atau manual visualisasi
5. **Rekonstruksi sinyal:** hilangkan IC artefak, gabungkan ulang
6. **Ekstraksi fitur:** daya alfa, beta, atau statistik waktu (mean, std, entropy)
7. **Klasifikasi:** Random Forest atau CNN 1D ringan

<a id="efek-ica-terhadap-stabilitas-fitur"></a>
***Sinyal hasil ICA menjadi lebih stabil secara temporal dan mengurangi noise tinggi-frekuensi yang dapat mengaburkan pola mata tertutup.*** Misalnya, komponen dengan dominasi artefak frontal (seperti blinking spike) sering muncul dan bisa dihapus. Ini memperjelas fluktuasi pita alfa saat mata tertutup, yang sebelumnya tidak muncul atau dikaburkan.

<a id="hasil-sementara-akurasi-sebelum-dan-sesudah-ica"></a>
***Perbandingan performa menunjukkan peningkatan akurasi klasifikasi dari ~80% menjadi ~91% setelah ICA diterapkan.*** Tanpa ICA, banyak noise yang menyebabkan model membaca sinyal non-fisiologis sebagai fitur. Dengan ICA, fitur fisiologis menjadi lebih representatif dan klasifikasi lebih stabil antar-subjek.

<a id="refleksi-dan-implikasi"></a>
***Meski sederhana, studi kasus ini menunjukkan bahwa ICA tetap berdampak signifikan bahkan pada perangkat EEG komersial dan dataset non-klinis.*** ICA membantu membersihkan sinyal dari artefak dominan tanpa harus mengorbankan struktur sinyal penting. Dalam studi EO/EC, di mana sinyal alfa adalah fitur utama, kemampuan ICA untuk memisahkan sinyal otak dari noise terbukti bermanfaat. Pendekatan ini juga dapat diekstensikan ke skenario real-time dengan pipeline ringan.