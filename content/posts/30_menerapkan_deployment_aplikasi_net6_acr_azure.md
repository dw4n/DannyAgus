---
title: "Menerapkan Deployment Aplikasi .NET 6 Menggunakan Azure Container Registry (ACR) ke Azure App Service"
title_underscore: "menerapkan_deployment_aplikasi_net6_acr_azure"
summary: Panduan alternatif untuk mendistribusikan image Docker dari aplikasi .NET 6 menggunakan Azure Container Registry dan mengintegrasikannya dengan Azure App Service.
date: 2025-05-27T10:20:00+08:00
tags: ["azure", "acr", "docker", "deployment", "appservice"]
categories: ["Blog"]
draft: false
---

> #### Outline Artikel  
> 1. [Pendahuluan](#pendahuluan)  
> 2. [Memahami Azure Container Registry (ACR)](#memahami-acr)  
> 3. [Membuat Azure Container Registry](#membuat-azure-container-registry)  
> 4. [Build dan Push Image dengan Docker Desktop](#build-dan-push-image-dengan-docker-desktop)  
> 5. [Menghubungkan ACR dengan Azure App Service](#menghubungkan-acr-dengan-azure-app-service)  
> 6. [Refleksi dan Penutup](#refleksi-dan-penutup)

<a id="pendahuluan"></a>  
### 1. Pendahuluan

**Azure Container Registry (ACR) adalah solusi registry private dari Microsoft Azure untuk menyimpan dan mengelola image Docker secara aman dan terintegrasi.** Dalam praktik DevOps modern, menggunakan registry private seperti ACR memberi kontrol lebih terhadap distribusi image dan mendukung integrasi yang lebih ketat dengan layanan Azure lainnya. Artikel ini menyajikan alternatif dari penggunaan Docker Hub, dengan memanfaatkan ACR untuk menyimpan image dan mendistribusikannya ke Azure App Service.

<a id="memahami-acr"></a>  
### 2. Memahami Azure Container Registry (ACR)

**ACR memberikan keamanan, fleksibilitas, dan integrasi mendalam dengan ekosistem Azure yang tidak dimiliki oleh registry publik seperti Docker Hub.** Karena ACR merupakan layanan native Azure, ia mendukung autentikasi berbasis Azure Active Directory, peran berbasis akses (RBAC), enkripsi data, dan geo-replikasi. Keunggulan ini sangat penting dalam skenario enterprise, di mana kontrol terhadap siapa yang bisa menarik atau mendorong image sangat krusial. ACR juga mempermudah integrasi pipeline CI/CD berbasis Azure.

<a id="membuat-azure-container-registry"></a>  
### 3. Membuat Azure Container Registry

**Langkah pertama dalam menggunakan ACR adalah membuat instance registry baru melalui Azure CLI.** Gunakan perintah berikut dengan mengganti `youracrname`, `yourresourcegroup`, dan `yourlocation` sesuai dengan kebutuhan:

```bash
az acr create   --name youracrname   --resource-group yourresourcegroup   --sku Basic   --location yourlocation   --admin-enabled true
```

Setelah itu, login ke akun Azure dan ke ACR untuk mengizinkan Docker mendorong image:

```bash
az login
az acr login --name youracrname
```

<a id="build-dan-push-image-dengan-docker-desktop"></a>  
### 4. Build dan Push Image dengan Docker Desktop

**Image aplikasi dibangun secara lokal menggunakan Docker Desktop, kemudian ditag dan didorong ke ACR.** Proses ini identik dengan penggunaan Docker Hub, hanya berbeda pada tujuan push-nya. Pertama, bangun image menggunakan Docker:

```bash
docker build -t yourappimage .
```

Lalu dapatkan login server dari ACR:

```bash
az acr show --name youracrname --query loginServer --output tsv
```

Tag image sesuai format ACR:

```bash
docker tag yourappimage:latest youracrname.azurecr.io/yourappimage:latest
```

Dan dorong ke registry Azure:

```bash
docker push youracrname.azurecr.io/yourappimage:latest
```

Pastikan perintah `docker push` dijalankan setelah login ACR berhasil agar akses tidak ditolak.

<a id="menghubungkan-acr-dengan-azure-app-service"></a>  
### 5. Menghubungkan ACR dengan Azure App Service

**Azure App Service dapat langsung menarik image dari ACR jika sudah terhubung dan memiliki hak akses.** Gunakan perintah berikut untuk mengatur App Service agar menggunakan image dari ACR:

```bash
az webapp config container set   --name yourappname   --resource-group yourresourcegroup   --docker-custom-image-name youracrname.azurecr.io/yourappimage:latest
```

Jika ACR menggunakan kredensial admin, kamu juga bisa menyertakan username dan password:

```bash
az webapp config container set   --name yourappname   --resource-group yourresourcegroup   --docker-custom-image-name youracrname.azurecr.io/yourappimage:latest   --docker-registry-server-url https://youracrname.azurecr.io   --docker-registry-server-user youracrusername   --docker-registry-server-password youracrpassword
```

<a id="refleksi-dan-penutup"></a>  
### 6. Refleksi dan Penutup

**Menggunakan Azure Container Registry sebagai alternatif Docker Hub memberikan fleksibilitas dan keamanan tambahan dalam pengelolaan image.** ACR terintegrasi langsung dengan Azure Active Directory dan layanan Azure lainnya, sehingga memudahkan pengelolaan akses, pemantauan, dan otomatisasi. Dengan pendekatan ini, tim DevOps dapat menjaga kontrol penuh atas image yang digunakan dalam produksi, sekaligus mempercepat integrasi ke dalam pipeline CI/CD yang aman dan efisien.
