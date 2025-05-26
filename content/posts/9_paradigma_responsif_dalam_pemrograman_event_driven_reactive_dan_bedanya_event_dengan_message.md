---
title: "Paradigma Responsif dalam Pemrograman: Event-Driven, Reactive, dan Bedanya Event dengan Message"
title_underscore: "paradigma_responsif_dalam_pemrograman_event_driven_reactive_dan_bedanya_event_dengan_message"
summary: Penjelasan perbedaan mendasar antara event-driven dan reactive programming, serta konsep event dan message dalam arsitektur perangkat lunak. Panduan memilih paradigma yang sesuai untuk aplikasi interaktif dan sistem real-time.
date: 2025-05-23T00:00:00+08:00
tags: ["pemrograman", "event-driven", "reactive", "event", "message", "konsep"]
categories: ["Konsep Pemrograman"]
draft: false
---

---
> #### Outline Artikel
> 1. [Pendahuluan: Paradigma Responsif](#pendahuluan)
> 2. [Apa Itu Event-Driven Programming?](#event-driven)
> 3. [Apa Itu Reactive Programming?](#reactive)
> 4. [Perbedaan antara Event dan Message](#event-vs-message)
> 5. [Kapan Menggunakan Event-Driven vs Reactive](#penggunaan-paradigma)
---

<span id="pendahuluan"></span>

Dalam membangun aplikasi yang interaktif dan adaptif, dua pendekatan yang sering digunakan adalah **Event-Driven Programming** dan **Reactive Programming**. Meskipun keduanya berfokus pada "respon terhadap sesuatu", cara kerjanya secara struktural sangat berbeda. Memahami perbedaan ini penting agar kita bisa memilih arsitektur yang tepat, terutama saat membangun aplikasi UI, sistem real-time, atau layanan mikro. Artikel ini akan membahas keduanya beserta perbedaan antara *event* dan *message* melalui penjelasan dan pseudocode.

<span id="event-driven"></span>

**Event-Driven Programming** adalah paradigma di mana eksekusi kode bergantung pada event yang terjadi. Dalam pendekatan ini, kita biasanya menetapkan listener (pendengar) yang siap menjalankan fungsi tertentu saat sebuah event dipicu. Misalnya, ketika user mengklik tombol, event “onClick” dipicu, lalu handler dijalankan. Program secara pasif menunggu event daripada aktif berjalan terus-menerus.

```pseudo
function onButtonClick() {
  showAlert("Tombol diklik!");
}

button.listen("click", onButtonClick);
```

<span id="reactive"></span>

**Reactive Programming** mengasumsikan bahwa aliran data (stream) akan terus berubah, dan sistem akan secara otomatis bereaksi terhadap perubahan itu. Dalam paradigma ini, data bersifat dinamis dan bisa di-*observe*. Ketika nilai data berubah, semua proses yang *dependen* terhadap data itu akan diperbarui secara otomatis, tanpa kita perlu memicu secara manual.

```pseudo
temperatureStream = observeSensor("room_temperature")

temperatureStream.onChange(temp => {
  if (temp > 30) {
    fan.turnOn()
  }
});
```

<span id="event-vs-message"></span>

**Event** adalah **notifikasi satu arah** bahwa sesuatu telah terjadi. Event biasanya tidak membawa instruksi eksplisit atau harapan balasan. Sementara *message* adalah **komunikasi eksplisit**, biasanya berisi data atau perintah, dan sering kali mengharapkan balasan.

```pseudo
// Event
eventBus.emit("userLoggedIn")

// Message
sendMessage(to: "UserService", message: { action: "getUser", id: 42 })
```

<span id="penggunaan-paradigma"></span>

Event-driven menunggu interaksi lalu bereaksi, sementara reactive terus mengamati aliran data dan bertindak otomatis saat ada perubahan. Keduanya valid dan kuat, tergantung kebutuhan aplikasi. Sementara itu, memahami perbedaan antara *event* dan *message* membantu kita mendesain sistem yang lebih bersih dan efektif, terutama dalam arsitektur mikroservis.