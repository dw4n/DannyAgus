---
title: "Kapan Kita Tidak Perlu ICA dalam Pipeline EEG?"
title_underscore: "kapan_kita_tidak_perlu_ica_dalam_pipeline_eeg"
summary: Meskipun ICA merupakan alat kuat untuk reduksi artefak, tidak semua skenario analisis EEG membutuhkannya—beberapa kondisi justru lebih efektif tanpa ICA.
date: 2025-05-30T00:13:00+08:00
tags: ["ICA", "EEG", "pipeline", "deep learning", "pra-pemrosesan"]
categories: ["Paper"]
draft: false
---

> #### Outline Artikel  
> 1. [Pendahuluan](#pendahuluan)  
> 2. [ICA: Kekuatan dan Biaya Komputasi](#ica-kekuatan-dan-biaya-komputasi)  
> 3. [Studi yang Menunjukkan Deep Learning Bisa Tanpa ICA](#studi-yang-menunjukkan-deep-learning-bisa-tanpa-ica)  
> 4. [Skenario Spesifik di Mana ICA Bisa Dilewati](#skenario-spesifik-di-mana-ica-bisa-dilewati)  
> 5. [Risiko dan Kompromi Saat Melewatkan ICA](#risiko-dan-kompromi-saat-melewatkan-ica)  
> 6. [Kesimpulan dan Prinsip Praktis](#kesimpulan-dan-prinsip-praktis)

<a id="pendahuluan"></a>
***ICA telah lama dianggap sebagai langkah standar dalam pemrosesan sinyal EEG, namun dalam beberapa konteks, penggunaannya justru tidak diperlukan—atau bahkan bisa kontraproduktif.*** Dalam dekade terakhir, pendekatan machine learning dan deep learning end-to-end telah menantang pandangan tradisional yang menganggap pra-pemrosesan berbasis ICA selalu diperlukan. Dalam artikel ini, kita akan meninjau kapan, mengapa, dan dalam kondisi apa ICA tidak perlu dimasukkan dalam pipeline EEG.

<a id="ica-kekuatan-dan-biaya-komputasi"></a>
***ICA efektif untuk memisahkan dan menghilangkan artefak sistematik seperti EOG dan EMG, tetapi prosesnya memakan waktu dan bergantung pada asumsi statistik yang tidak selalu valid.*** ICA mengasumsikan pencampuran linier dan sumber independen, yang tidak selalu terjadi dalam sinyal otak nyata. Selain itu, langkah identifikasi dan penghapusan komponen artefak dapat memerlukan keterlibatan manusia atau model tambahan, yang menambah kompleksitas pipeline. Dalam lingkungan dengan kebutuhan real-time atau keterbatasan komputasi, ICA bisa menjadi hambatan alih-alih solusi.

<a id="studi-yang-menunjukkan-deep-learning-bisa-tanpa-ica"></a>
***Beberapa studi menunjukkan bahwa model deep learning dapat mencapai kinerja tinggi bahkan tanpa preprocessing ICA—terutama jika artefak bersifat konsisten antar-kondisi.*** Studi oleh Del Pup et al. (2024) menunjukkan bahwa dalam beberapa kasus, pipeline minimal (tanpa ICA) menghasilkan performa klasifikasi yang tidak kalah atau bahkan lebih baik. Hal ini terjadi karena model deep learning seperti CNN atau transformer dapat belajar membedakan sinyal tugas dari noise, selama pola artefak tidak bersifat bias terhadap satu kelas tertentu. Dengan kata lain, artefak yang stabil antar label mungkin tidak merusak, melainkan netral terhadap performa klasifikasi.

<a id="skenario-spesifik-di-mana-ica-bisa-dilewati"></a>
***ICA dapat dilewati secara sah dalam kondisi berikut: (1) data telah cukup bersih, (2) klasifikasi berlangsung dalam domain temporal sempit, (3) artefak relatif stabil antar kelas, dan (4) model memiliki kapasitas denoising implisit.*** Misalnya, untuk klasifikasi motor imagery pada subjek sehat dengan data baseline pendek, artefak seperti EOG mungkin tidak signifikan secara diskriminatif. Demikian pula, autoencoder dan CNN dapat bertindak sebagai pemurni sinyal internal, mengurangi kebutuhan akan pembersihan manual di awal. Dalam BCI real-time, menghindari ICA kadang menjadi kebutuhan teknis agar sistem responsif.

<a id="risiko-dan-kompromi-saat-melewatkan-ica"></a>
***Meski sah untuk dilewati, melewatkan ICA juga membawa risiko: artefak bisa menjadi 'shortcut' bagi model, menyebabkan bias, overfitting, atau generalisasi buruk.*** Jika artefak hanya muncul pada satu kondisi label (misalnya, kedipan mata saat kondisi EC), maka mo
