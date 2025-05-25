---
title: "Memahami Azure Eventing & Messaging: Service Bus, Event Hub, dan Event Grid"
title_underscore: "memahami_azure_eventing_messaging_service_bus_event_hub_dan_event_grid_dalam_8_paragraf"
date: 2025-05-23T00:00:00+08:00
tags: ["azure", "eventing", "messaging", "service bus", "event hub", "event grid", "cloud", "arsitektur"]
categories: ["Konsep Pemrograman"]
draft: false
---

---
> #### Outline Artikel
> 1. [Pengantar: Azure dan Arsitektur Event-Driven](#pengantar)
> 2. [Apa Itu Azure Service Bus?](#service-bus)
> 3. [Apa Itu Azure Event Hub?](#event-hub)
> 4. [Apa Itu Azure Event Grid?](#event-grid)
> 5. [Tabel Perbandingan Fitur Utama](#tabel-perbandingan)
> 6. [Kapan Menggunakan Masing-Masing?](#kapan-menggunakan)
---

<span id="pengantar"></span>

Dalam arsitektur modern berbasis cloud, komunikasi antar sistem dan layanan menjadi komponen inti. Di platform Azure, Microsoft menyediakan beberapa layanan yang dirancang khusus untuk menangani berbagai jenis komunikasi dan event: **Azure Service Bus**, **Event Hub**, dan **Event Grid**. Meskipun semuanya terkesan “mirip” karena berurusan dengan data dan peristiwa, masing-masing memiliki tujuan spesifik dan cara kerja berbeda.

<span id="service-bus"></span>

**Azure Service Bus** adalah layanan messaging enterprise-grade yang dirancang untuk **komunikasi andal antar aplikasi atau layanan**. Ia mendukung konsep **message queue** dan **publish-subscribe** model. Ideal untuk komunikasi antar microservices backend atau sistem transaksi.

```pseudo
queueClient.send("OrderQueue", message: { orderId: 123, item: "Laptop" })

while message = queueClient.receive("OrderQueue"):
  processOrder(message)
```

<span id="event-hub"></span>

**Azure Event Hub** adalah layanan untuk mengelola **aliran data berdurasi tinggi**, cocok untuk **telemetri, log, atau data streaming** dari banyak sumber.

```pseudo
for each second:
  eventHub.send("TemperatureSensor", { deviceId: "sensor1", value: 28.4 })
```

<span id="event-grid"></span>

**Azure Event Grid** berfungsi sebagai **event router**. Ia menerima **event dari berbagai sumber** (Blob, Resource Group, dll) dan menyampaikannya ke handler yang sesuai (Function, Webhook, Logic App).

```pseudo
on BlobCreated in "MyContainer":
  send event to Function("GenerateThumbnail")
```

<span id="tabel-perbandingan"></span>

| Fitur              | Service Bus             | Event Hub                | Event Grid              |
|--------------------|--------------------------|---------------------------|--------------------------|
| Fokus              | Messaging (command)      | Data streaming            | Event notification       |
| Delivery           | Reliable queue/pub-sub   | High-throughput ingestion | Lightweight push         |
| Durability         | Tinggi                   | Tinggi                    | Menengah (event TTL)     |
| Use case utama     | Backend komunikasi       | IoT, telemetry, logs      | Resource event routing   |

<span id="kapan-menggunakan"></span>

**Service Bus** cocok untuk sistem backend transactional, **Event Hub** untuk streaming skala besar, dan **Event Grid** untuk event-driven ringan. Ketiganya bisa dikombinasikan dalam arsitektur eventing yang kompleks dan scalable di Azure.

