---
title: "Menerapkan Deployment Aplikasi .NET 6 Menggunakan DockerHub ke Azure App Service"
title_underscore: "menerapkan_deployment_aplikasi_net6_docker_azure"
summary: Panduan terstruktur untuk membangun, mengemas, dan mendistribusikan aplikasi PDF Generator berbasis .NET 6 ke Azure melalui Docker.
date: 2025-05-27T10:10:00+08:00
tags: ["azure", "docker", "aspnetcore", "deployment", "devops"]
categories: ["Blog"]
draft: false
---

> #### Outline Artikel  
> 1. [Pendahuluan](#pendahuluan)  
> 2. [Mempersiapkan Aplikasi dan Lingkungan Docker](#mempersiapkan-aplikasi-dan-lingkungan-docker)  
> 3. [Distribusi Image melalui Docker Hub](#distribusi-image-melalui-docker-hub)  
> 4. [Proses Deployment ke Azure App Service](#proses-deployment-ke-azure-app-service)  
> 5. [Refleksi dan Penutup](#refleksi-dan-penutup)

<a id="pendahuluan"></a>  
### 1. Pendahuluan

***Proses deployment aplikasi modern memerlukan pemahaman menyeluruh terhadap teknologi kontainerisasi dan layanan cloud.*** Dalam konteks pengembangan berbasis .NET, pendekatan yang mengintegrasikan Docker dan Microsoft Azure memungkinkan pengembang untuk menciptakan pipeline distribusi yang tangguh, portabel, dan dapat diskalakan. Artikel ini membahas secara sistematis bagaimana sebuah layanan API PDF Generator yang dikembangkan menggunakan .NET 6 dapat dibangun sebagai image Docker, dipublikasikan ke Docker Hub, dan akhirnya di-deploy ke Azure App Service untuk pengujian maupun produksi. Dengan pendekatan berbasis CLI dan container registry publik, proses ini mencerminkan praktik DevOps yang profesional dan reproducible.

<a id="mempersiapkan-aplikasi-dan-lingkungan-docker"></a>  
### 2. Mempersiapkan Aplikasi dan Lingkungan Docker

***Langkah awal yang penting adalah mempersiapkan struktur proyek dan konfigurasi build agar kompatibel dengan proses kontainerisasi.*** Proyek ini menggunakan ASP.NET Core 6 dan berbagai dependensi yang memerlukan build yang konsisten sebelum dikemas ke dalam Docker image. Setelah memastikan struktur proyek dapat dipublikasikan, langkah pertama adalah melakukan proses build dan publish dari aplikasi .NET.

```bash
dotnet publish -c Release -o ./publish
```

Setelah itu, kita dapat membangun image Docker dengan menggunakan Dockerfile yang sudah disiapkan di dalam subdirektori proyek:

```bash
docker build -f Path/To/YourProject/Dockerfile -t yourappimage .
```

Perintah ini akan menghasilkan sebuah Docker image lokal bernama `yourappimage`, yang siap untuk tahap berikutnya yaitu tagging dan pushing ke registry.

<a id="distribusi-image-melalui-docker-hub"></a>  
### 3. Distribusi Image melalui Docker Hub

***Image yang sudah dibangun secara lokal menggunakan Docker Desktop kemudian perlu diunggah ke registry publik agar dapat diakses oleh Azure.*** Sebelum melakukan push ke Docker Hub, pastikan Anda telah login menggunakan akun Docker Anda. Ini penting untuk memastikan Docker Desktop memiliki izin untuk mengunggah image, terutama jika akun Anda menggunakan registry privat.

```bash
docker login
```

***Setelah image Docker berhasil dibangun secara lokal, tahap berikutnya adalah mengunggahnya ke Docker Hub sebagai registry publik.*** Untuk memungkinkan image ini diakses dari Azure, kita perlu memberi tag sesuai dengan format yang dikenali oleh Docker Hub.

```bash
docker tag yourappimage:latest yourdocker/yourappimage:latest
```

Setelah proses tagging selesai, image dapat dikirim ke Docker Hub menggunakan perintah berikut:

```bash
docker push yourdocker/yourappimage:latest
```

Langkah ini memastikan bahwa image kita dapat diakses oleh Azure App Service Container secara publik (atau privat, bila menggunakan autentikasi).

<a id="proses-deployment-ke-azure-app-service"></a>  
### 4. Proses Deployment ke Azure App Service

***Deployment ke Azure App Service dilakukan melalui Azure CLI dengan mengatur konfigurasi container dan merestart instans layanan.*** Pastikan CLI Azure sudah login dan terhubung dengan subscription yang benar. Kemudian, atur image yang akan digunakan oleh Web App melalui perintah berikut:

```bash
az webapp config container set   --name yourappname   --resource-group yourresourcegroup   --docker-custom-image-name yourdocker/yourappimage:latest
```

Untuk memastikan image berjalan dengan baik, lakukan restart terhadap Web App:

```bash
az webapp restart   --name yourappname   --resource-group yourresourcegroup
```

Dan untuk memverifikasi hasil deployment, kita bisa melihat log deployment:

```bash
az webapp log deployment show   --name yourappname   --resource-group yourresourcegroup
```

Jika berhasil, aplikasi Anda seharusnya dapat diakses melalui URL:
```
http://yourappname.azurewebsites.net
```

<a id="refleksi-dan-penutup"></a>  
### 5. Refleksi dan Penutup

***Proses deployment yang terstandarisasi ini tidak hanya mencerminkan praktik DevOps modern, tetapi juga meningkatkan reusabilitas dan keandalan sistem.*** Dengan menggunakan Docker sebagai mekanisme pembungkusan dan Azure sebagai target deployment, tim pengembang dapat membangun jalur distribusi yang konsisten dan dapat direplikasi untuk berbagai aplikasi. Praktik seperti ini juga mempercepat integrasi ke dalam pipeline CI/CD, dan memudahkan debug ketika terjadi error. Studi kasus ini menunjukkan bagaimana integrasi antara alat modern dapat menyederhanakan proses yang dulu kompleks. Oleh karena itu, penguasaan terhadap alur ini bukan hanya nilai tambah, melainkan keharusan dalam pengembangan cloud-native saat ini.
