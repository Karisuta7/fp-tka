# 💻 Final Project - TKA

## 🧾 Ringkasan Proyek

- **Tujuan**: Merancang, membangun, dan menguji arsitektur cloud untuk aplikasi absensi face recognition.
- **Aplikasi**: Backend (Python FastAPI), Frontend (React JS), Database.
- **Budget**: Maksimal \$100/bulan.
- **Platform**: Azure
- **Output Utama**: Laporan di GitHub (arsitektur, implementasi, hasil load testing).

---

## 👥 Anggota Kelompok

| Nama                     | NRP        | Tugas      |
|--------------------------|------------|------------|
| Adi Satria Pangestu      | 5027231043 |Back-end & Database|
| Ryan Adya Purwanto       | 5027231046 |Setup & Implementasi Arsitektur|
| Danar Bagus Rasendriya   | 5027231055 |Setup Locust, Load Testing & Documentation|
| Callista Meyra Azizah    | 5027231060 |Merancang Arsitektur Cloud|
| Gandhi Ert Julio         | 5027231081 |Front-end & Webserver/Load Balancer|

---

## 🧠 Arsitektur Final Project

![Arsitektur](https://github.com/user-attachments/assets/eb80ed94-b284-4493-a0e8-9b10638cc044)

### 📝 Penjelasan Detail Diagram

Diagram ini memvisualisasikan arsitektur tiga tingkat (**3-tier architecture**) yang modern dan scalable.

#### 🔹 Load Balancer (Tier 1 - Web Tier)
- **VM**: `vm1` seharga \$4/bulan  
- **Software**: Nginx  
- **Fungsi**: Sebagai gerbang utama aplikasi, VM ini memiliki Public IP dan menerima semua permintaan dari pengguna. Ia akan meneruskan permintaan tersebut ke salah satu App Server secara seimbang agar beban terdistribusi merata.

#### 🔹 Application Servers (Tier 2 - Logic Tier)
- **VM**: 2 unit `vm3`, masing-masing \$12/bulan (**Total \$24/bulan**)  
- **Software**: Backend (Python FastAPI) dan Frontend (React JS)  
- **Fungsi**: Menangani logika bisnis seperti pemrosesan data dan pengenalan wajah. Kedua server ini berada dalam jaringan privat dan hanya menerima koneksi dari Load Balancer.

#### 🔹 Database Server (Tier 3 - Data Tier)
- **VM**: `vm2` seharga \$6/bulan  
- **Software**: MongoDB  
- **Fungsi**: Menyimpan seluruh data aplikasi. Terhubung hanya dengan App Server melalui jaringan privat untuk menjaga keamanan data.

---

### 🔄 Alur Kerja (Workflow)
1. Pengguna membuka aplikasi melalui browser.
2. Permintaan dikirim via Internet ke Public IP Load Balancer.
3. Load Balancer memilih salah satu App Server untuk memproses permintaan.
4. App Server menangani permintaan, dan jika dibutuhkan data, ia menghubungi Database Server.
5. Database Server mengirimkan data kembali ke App Server.
6. App Server mengirimkan respons ke pengguna melalui Load Balancer.

---

### 🏗️ Rencana Arsitektur Aplikasi

Kita menggunakan **3 jenis server (total 4 VM)**:

1. **VM1 - Load Balancer**
   - Harga: \$4
   - Software: Nginx

2. **VM3 (x2) - App Server**
   - Harga: \$12 x 2 = \$24
   - Software: FastAPI + React

3. **VM2 - Database Server**
   - Harga: \$6
   - Software: MongoDB

**Keunggulan:**
- Scalable: Bisa menambah App Server sesuai kebutuhan.
- High availability: Sistem tetap berjalan walau salah satu App Server gagal.
- Best practice industri: Arsitektur modern dan modular.

**Kekurangan:**
- Setup lebih kompleks, tapi masih dalam batas yang bisa dikelola.

**Total biaya: \$34/bulan** — masih jauh di bawah batas budget \$100.

---

### 🔁 Alur Permintaan User (Diagram)

```text
User
  ↓
Internet
  ↓
Public IP - VM1 (Nginx, $4)
  ↓
Private IP - VM3 (FastAPI + React, $12)
      atau
Private IP - VM3 (FastAPI + React, $12)
  ↓
Private IP - VM2 (MongoDB, $6)
```
---
### ✅ Mengapa Arsitektur ini Dipilih
- Opsi ini ideal: aman di budget, siap untuk skala besar, dan tunjukkan pemahaman cloud (load balancing & scale out).
- Sisa budget $66 bisa dipakai kalau nanti mau upgrade VM saat uji performa.
---

## Overview Problem

Anda adalah seorang lulusan Teknologi Informasi, sebagai ahli IT salah satu kemampuan yang harus dimiliki adalah kemampuan merancang, membangun, mengelola aplikasi berbasis komputer menggunakan layanan awan untuk memenuhi kebutuhan organisasi.

Pada suatu saat anda mendapatkan project departemen untuk mendeploy sebuah aplikasi Absen berbasis face recognition dengan komponen Backend menggunakan python Fast API dan frontend menggunakan React JS. Kemudian anda diminta untuk mendesain arsitektur cloud yang sesuai dengan kebutuhan aplikasi tersebut.

---

## Alur Pengerjaan dan Implementasi

### Mempersiapkan Lingkungan Kerja

#### Membuat Resource Group pada Azure
- Nama: `cloudfp`
- Location: Central India (Asia Pacific)

![image9](https://github.com/user-attachments/assets/c17a403d-40e7-4faf-a0d3-be3469f0e1e2)

---

#### Membuat Virtual Network (VNet)
- Nama: `cloudfp-net`
- Address Space: `10.1.0.0/16`
- Location: Central India (Asia Pacific)

![image14](https://github.com/user-attachments/assets/593a7058-5563-4bb0-abeb-0e4d40107ff9)

---

Menambahkan Subnet pada VNet:
- Nama: `default`

![image10](https://github.com/user-attachments/assets/81d074d8-32b1-4c5e-9d33-91d6a05d8949)

---

Setting Subnet `default`

![image1](https://github.com/user-attachments/assets/c8f68161-be6a-47df-ab46-be43f16188c3)

---

#### Membuat VM
1. `vm-loadbalancer`

![image2](https://github.com/user-attachments/assets/7e59d3f7-2550-48c1-a8fb-e708b8b90959)

---

- Public IP:
`20.193.144.232`

- Private IP:
`10.1.0.4`

- Resource:
$8/bulan - Standard B1s (1 vcpu, 1 GiB memory)

- OS: 
Linux (ubuntu 24.04)

---

2. `vm-app-1`

![image7](https://github.com/user-attachments/assets/cd21aeb2-8d13-4020-8cb1-8343791cd908)

- Public IP:
`20.193.143.96`

- Private IP:
`10.1.0.5`

---

3. `vm-app-2`

![image11](https://github.com/user-attachments/assets/6b32b031-d59b-4648-af57-9ebec57e8f5a)

- Public IP:
`20.193.129.183`

- Private IP:
`10.1.0.6`

---

4. `vm-database`

![image4](https://github.com/user-attachments/assets/9fb49ee1-3030-4918-909e-1788482609bc)

- Private IP:
`10.1.0.7`

---

#### Setting NSG (Firewall) untuk masing masing VM
1. `vm-loadbalancer` - Allow any IP

![image12](https://github.com/user-attachments/assets/492e4af8-0e8a-4a6f-bb68-64636e2a51c4)

---

2. `vm-app-1` - Allow Private IP from Load Balancer

![image8](https://github.com/user-attachments/assets/29ed7e89-6071-4ee9-92b5-6dbca1e96022)

---

3. `vm-app-2` - Allow Private IP from Load Balancer

![image3](https://github.com/user-attachments/assets/662cf1df-7359-4853-beb4-137fbc5eacd1)

---

4. `vm-database` - Allow Private IP from `vm-app-1` and `vm-app-2`

![image13](https://github.com/user-attachments/assets/6e80bd89-5cbe-4c05-8e71-1fb4ea8419ed)

---

#### Test Konektivitas
Mencoba menghubungkan dengan `vm-loadbalancer`

![image15](https://github.com/user-attachments/assets/2e211f6a-e539-44d2-a372-984078567827)

---

Setelah terkoneksi, lakukan ping `vm-app-1` dan `vm-app-2` (melalui `vm-loadbalancer`)

![image5](https://github.com/user-attachments/assets/63d41dba-cecd-4a3a-9fe5-8bab9e47b868)

---

Ping berhasil dilakukan, selanjutnya install `nginx` di `vm-loadbalancer`

![image6](https://github.com/user-attachments/assets/fc10b725-b6fd-4e90-9602-01ceeda7c693)

---

 berhasilnya koneksi SSH ke VM database (vm-database) melalui IP privat: 10.1.0.7

![Image7](https://github.com/user-attachments/assets/a87ce428-c47c-476e-bf6d-db8f59571444)

---

### Inisialisasi Backend & Database

Instalasi MongoDB dan Konfigurasi agar bisa diakses dari VM lain: Edit file /etc/mongod.conf, ubah bindIp dari 127.0.0.1 menjadi 0.0.0.0.

![Image](https://github.com/user-attachments/assets/815deda3-0c3d-4ca0-a086-d1b58db039ad)

![Image](https://github.com/user-attachments/assets/9ea5e832-fdc2-436f-8bd2-da82558479e3)
---
Setup environment dan dependensi untuk `vm-app-1` dan `vm-app-2`

#### Result di `vm-app-1`
![image](https://github.com/user-attachments/assets/e24d382d-a061-42eb-a53f-a5c9f15493d1)

![Image](https://github.com/user-attachments/assets/db14e8c6-2c76-4413-ac37-752b9c0633b7)

#### Result yang sama di `vm-app-2`
![Image](https://github.com/user-attachments/assets/ebc805bd-4458-4b69-8ff0-bb0beafbef67)

---

### Konfigurasi Frontend & Load Balancer (NginX)

Masuk ke VM Load Balancer menggunakan SSH, dan install Build Aplikasi React

![Image](https://github.com/user-attachments/assets/936a68d2-f3df-40e0-92e2-3fb9564dafeb)
![Image](https://github.com/user-attachments/assets/c041c8e9-4e26-48e8-9f81-ffd86fa689bc)
---

Folder build sekarang berisi file statis yang siap

![Image](https://github.com/user-attachments/assets/d35db853-275d-4be5-9643-e7d599a696ff)

---

### Setup Load Balancer (Nginx)

Masih ke VM Load Balancer menggunakan SSH, Install Nginx: `sudo apt install -y nginx`.
Dan lakukan perubahan di konfigurasi: `sudo nano /etc/nginx/sites-available/default`.

![Image](https://github.com/user-attachments/assets/b382410e-1ec7-4d0f-afa1-b07cb94ec498)

Lalu ganti dengan konfigurasi ini (sesuaikan path dan IP)

![Image](https://github.com/user-attachments/assets/da4b5a58-25c6-48b4-8795-b32d7662cb90)

lalu test dan restart nginx `sudo systemctl restart nginx`

![Image](https://github.com/user-attachments/assets/36b615ef-41f1-4495-9f6b-752b07d0a910)

NGINX berhasil diaktifkan dan siap digunakan sebagai Load Balancer.

---

## Tampilan Antarmuka

### Halaman Login Aplikasi ITSScence
Tampilan halaman login frontend ReactJS berhasil di-deploy dan diakses melalui IP publik.

![Image](https://github.com/user-attachments/assets/2aa8f124-b1d0-4110-afad-39e7a15f07f5)
---

### Halaman Kiosk Mode
Mode Kiosk digunakan untuk melakukan absensi menggunakan teknologi Face Recognition.

![Image](https://github.com/user-attachments/assets/b8988afe-0c27-48d6-8844-1dfe9b181401)
---

### Endpoint Health Check
Pengujian dilakukan menggunakan Postman ke endpoint `GET /health`.

![Image](https://github.com/user-attachments/assets/ec82043d-cc23-4349-abc8-5aafda4ab6fb)
---
