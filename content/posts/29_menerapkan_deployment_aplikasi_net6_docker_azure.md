---
title: "Menerapkan Deployment Aplikasi .NET 6 Menggunakan DockerHub ke Azure App Service"
title_underscore: "menerapkan_deployment_aplikasi_net6_docker_azure"
summary: Panduan alternatif untuk mendistribusikan image Docker dari aplikasi .NET 6 menggunakan Docker Hub dan mengintegrasikannya dengan Azure App Service.
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

***Proses deployment aplikasi modern memerlukan pemahaman menyeluruh terhadap teknologi kontainerisasi dan layanan cloud.*** Dalam konteks pengembangan berbasis .NET, pendekatan yang mengintegrasikan Docker dan Microsoft Azure memungkinkan pengembang untuk menciptakan pipeline distribusi yang tangguh, portabel, dan dapat diskalakan. Artikel ini membahas secara sistematis bagaimana sebuah aplikasi .NET 6 dapat dibangun sebagai image Docker, dipublikasikan ke Docker Hub, dan akhirnya di-deploy ke Azure App Service untuk pengujian maupun produksi. Dengan pendekatan berbasis CLI dan container registry publik, proses ini mencerminkan praktik DevOps yang profesional dan reproducible.

<a id="mempersiapkan-aplikasi-dan-lingkungan-docker"></a>  
### 2. Mempersiapkan Aplikasi dan Lingkungan Docker

***Langkah awal yang penting adalah mempersiapkan struktur proyek dan konfigurasi build agar kompatibel dengan proses kontainerisasi.*** Proyek .NET 6 dikompilasi dan dipublikasikan terlebih dahulu, kemudian dikemas menggunakan Dockerfile. Proses ini dijalankan menggunakan Docker Desktop secara lokal.

```bash
dotnet publish -c Release -o ./publish
```

Setelah itu, bangun image Docker dengan perintah berikut:

```bash
docker build -f Path/To/YourProject/Dockerfile -t yourappimage .
```

<a id="distribusi-image-melalui-docker-hub"></a>  
### 3. Distribusi Image melalui Docker Hub

***Image yang sudah dibangun secara lokal menggunakan Docker Desktop kemudian perlu diunggah ke registry publik agar dapat diakses oleh Azure.*** Pastikan Anda telah login terlebih dahulu ke akun Docker Hub:

```bash
docker login
```

Setelah login, beri tag pada image Anda:

```bash
docker tag yourappimage:latest yourdocker/yourappimage:latest
```

Kemudian push ke Docker Hub:

```bash
docker push yourdocker/yourappimage:latest
```

Langkah ini memungkinkan Azure App Service untuk menarik image dari Docker Hub secara publik maupun privat (dengan kredensial).

<a id="proses-deployment-ke-azure-app-service"></a>  
### 4. Proses Deployment ke Azure App Service

***Deployment ke Azure App Service dilakukan melalui Azure CLI dengan mengatur konfigurasi container dan merestart instans layanan.*** Gunakan perintah berikut untuk mengatur Web App Anda agar menggunakan image dari Docker Hub:

```bash
az webapp config container set   --name yourappname   --resource-group yourresourcegroup   --docker-custom-image-name yourdocker/yourappimage:latest
```

Restart aplikasi agar image diterapkan:

```bash
az webapp restart   --name yourappname   --resource-group yourresourcegroup
```

Lihat log untuk memastikan deployment berhasil:

```bash
az webapp log deployment show   --name yourappname   --resource-group yourresourcegroup
```

Akses aplikasi Anda melalui:

```
http://yourappname.azurewebsites.net
```

<a id="refleksi-dan-penutup"></a>  
### 5. Refleksi dan Penutup

***Menggunakan Docker Hub sebagai registry untuk deployment aplikasi .NET 6 ke Azure App Service adalah metode sederhana dan efektif untuk integrasi container dengan layanan cloud.*** Meskipun ACR menawarkan fleksibilitas lebih untuk skala besar, Docker Hub tetap menjadi pilihan praktis dalam tahap awal pengembangan atau skenario open-source. Dengan pendekatan ini, pengembang dapat membangun pipeline deployment yang fleksibel dan cepat tanpa bergantung pada infrastruktur tambahan.
