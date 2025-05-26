---
title: "Memahami Konsep Event, Reactive, dan Message dalam Pemrograman"
summary: Penjelasan sederhana tentang event-driven dan reactive programming beserta perbedaan event dan message dalam sistem perangkat lunak. Dilengkapi analogi sehari-hari agar mudah dipahami pemula.
date: 2025-05-17T20:00:00+08:00
tags: ["pemrograman", "event-driven", "reactive", "message", "konsep", "penjelasan"]
categories: ["Konsep Pemrograman"]
draft: false
---

---
> #### Outline Artikel
> 1. [Pendahuluan: Sistem yang Responsif](#pendahuluan)
> 2. [Apa Itu Event-Driven Programming?](#event-driven)
> 3. [Contoh Sehari-hari Event-Driven](#contoh-event)
> 4. [Apa Itu Reactive Programming?](#reactive)
> 5. [Contoh Reactive di Kehidupan Nyata](#contoh-reactive)
> 6. [Perbedaan Event dan Message](#event-vs-message)
> 7. [Kesimpulan: Kapan Menggunakan Apa?](#kesimpulan)
---

<span id="pendahuluan"></span>

Dalam dunia pemrograman, banyak sistem dibuat agar bisa merespons sesuatu yang terjadi di sekitarnya, mirip seperti manusia yang bereaksi terhadap kejadian sehari-hari. Dua pendekatan yang banyak digunakan untuk membuat sistem seperti itu adalah ***Event-Driven Programming*** dan ***Reactive Programming***. Meskipun keduanya sering terdengar mirip, mereka memiliki cara kerja dan tujuan yang berbeda. Dengan contoh-contoh ringan seperti bel pintu, ember air, dan ajakan bermain, kita bisa memahami konsep-konsep ini tanpa harus mengerutkan dahi.

<span id="event-driven"></span>

***Event-Driven Programming*** adalah cara menulis program yang bekerja berdasarkan "kejadian". Bayangkan kamu sedang santai di rumah. Kamu tidak melakukan apa-apa sampai seseorang ***menekan bel pintu***. Saat bel berbunyi, kamu tahu ada tamu dan langsung ***membuka pintu***. Dalam dunia kode, sistem seperti ini akan menunggu event — seperti "klik tombol" atau "user login" — baru kemudian menjalankan aksi. Sama seperti kamu hanya membuka pintu saat bel ditekan, program ini hanya aktif saat ada kejadian tertentu.

<span id="contoh-event"></span>

Contoh lain, ketika kamu menyalakan lampu kamar dengan menekan sakelar. Sakelar ditekan adalah ***event***, dan lampu menyala adalah ***reaksinya***. Program yang event-driven seperti ini tidak bekerja terus-menerus, tapi hanya saat dibutuhkan. Itu sebabnya banyak antarmuka pengguna (UI) seperti aplikasi web dan mobile menggunakan gaya pemrograman event-driven.

<span id="reactive"></span>

***Reactive Programming*** mengasumsikan bahwa aliran data (stream) akan terus berubah, dan sistem akan secara otomatis merespons perubahan itu. Dalam paradigma ini, data bersifat dinamis dan bisa di-*observe*. Ketika nilai data berubah, semua proses yang *dependen* terhadap data itu akan diperbarui secara otomatis, tanpa kita perlu memicu secara manual.

<span id="contoh-reactive"></span>

Contoh lainnya: kamu punya jam digital pintar. Saat jam menunjukkan pukul 7 malam, lampu kamar menyala otomatis karena kamu atur begitu. Tidak ada tombol ditekan, tidak ada perintah dikirim. Sistem tahu waktunya berubah, dan langsung bertindak. Reactive programming sangat berguna untuk aplikasi real-time seperti IoT, dashboard monitoring, atau aplikasi keuangan.

<span id="event-vs-message"></span>

***Event*** adalah kejadian. Misalnya kamu ***tersandung dan jatuh***, dan temanmu tertawa karena melihat itu — kejadianmu adalah event, dan tawa temanmu adalah reaksi. ***Message***, di sisi lain, adalah komunikasi yang kamu kirimkan. Misalnya kamu bilang ke temanmu, “Ayo main ke luar yuk!” dan dia menjawab “Oke!”. Di sini, kamu mengirim message, dan temanmu bisa memilih untuk menjawab atau tidak.

| Konsep  | Contoh Sehari-Hari                        | Penjelasan                             |
|---------|-------------------------------------------|-----------------------------------------|
| Event   | Kamu jatuh, temanmu tertawa               | Kejadian yang diamati, bisa memicu aksi|
| Message | Kamu bilang "Main yuk!" ke teman          | Komunikasi atau perintah antar pihak   |

<span id="kesimpulan"></span>

Kesimpulannya, ***Event-Driven Programming*** cocok digunakan saat kita ingin sistem bereaksi terhadap tindakan pengguna secara langsung, seperti bel pintu atau tombol. ***Reactive Programming*** lebih cocok untuk sistem yang harus responsif terhadap aliran data yang berubah, seperti ember otomatis atau jam pintar. Sedangkan ***event*** dan ***message*** walaupun sering dipakai bersamaan, punya perbedaan penting: event adalah kejadian, message adalah komunikasi.
