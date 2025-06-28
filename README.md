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

## Rincian dan Pengujian API

### Rincian Endpoint API
| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/health` | Health check |
| `GET` | `/api/config` | Get configuration |
| `POST` | `/api/config` | Update configuration |
| `GET` | `/api/models` | Get available models |
| `GET` | `/api/attendance/mode` | Get attendance mode |
| `GET` | `/api/employees` | Get all employees |
| `POST` | `/api/employees/enroll` | Enroll new employee |
| `PUT` | `/api/employees/{id}` | Update employee data |
| `DELETE` | `/api/employees/{id}` | Delete employee |
| `GET` | `/api/employees/{id}/photo` | Get employee photo |
| `POST` | `/api/recognize-face` | **Face recognition** |
| `POST` | `/api/debug-face` | Debug face recognition |
| `POST` | `/api/attendance` | Record attendance |
| `GET` | `/api/attendance` | Get attendance history |
| `GET` | `/api/attendance/{id}/photo` | Get attendance photo |

## Endpoint Categories

### System Endpoints
- `GET /health` - Health check endpoint
- `GET /api/config` - Retrieve system configuration
- `POST /api/config` - Update system configuration
- `GET /api/models` - Get available AI models

### Employee Management
- `GET /api/employees` - List all employees
- `POST /api/employees/enroll` - Add new employee to system
- `PUT /api/employees/{id}` - Update employee information
- `DELETE /api/employees/{id}` - Remove employee from system
- `GET /api/employees/{id}/photo` - Retrieve employee profile photo

### Attendance System
- `GET /api/attendance/mode` - Get current attendance mode
- `POST /api/attendance` - Record new attendance entry
- `GET /api/attendance` - Retrieve attendance history
- `GET /api/attendance/{id}/photo` - Get attendance verification photo

### Face Recognition
- `POST /api/recognize-face` - **Main face recognition endpoint**
- `POST /api/debug-face` - Debug and test face recognition

---

### Endpoint Health Check
Pengujian dilakukan menggunakan Postman ke endpoint `GET /health`.

![Image](https://github.com/user-attachments/assets/ec82043d-cc23-4349-abc8-5aafda4ab6fb)
---

## Load Testing dan Analisis Hasil
Load Testing dilakukan dengan menguji Endpoint API `api/recognize_face` dengan beberapa skenario jumlah pengguna.

### 1 User
**Graph**
![chart locust user 1](https://github.com/user-attachments/assets/8b9174e3-41e7-49f8-99b0-df79d0535907)
***
**Statistic**
![stat locust user 1](https://github.com/user-attachments/assets/caf0b35b-8ce6-4d4b-ac2c-00b1854e8b38)
***
Grafik menunjukkan adanya satu lonjakan waktu respons yang signifikan hingga mencapai 8.505 ms di awal pengujian. Setelah lonjakan tersebut, performa kembali normal dengan median waktu respons 270 ms. Pada skenario ini belum terdeteksi adanya failure.
***

### 3 User
**Graph**
![chart locust user 3](https://github.com/user-attachments/assets/ed40acde-aa97-4f6b-9277-26d7b95a7c95)
***
**Statistic**
![stat locust user 3](https://github.com/user-attachments/assets/c846965e-9c69-4c48-82bf-4ab73035a65c)
***
Aplikasi berjalan sangat stabil dengan 3 pengguna, RPS (Request per Second) konsisten di angka 1.8 dan waktu respons sangat baik dan dapat diprediksi, dengan median 240 ms dan 95th percentile 490 ms. Belum ada failure.
***

### 5 User
**Graph**
![chart locust user 5](https://github.com/user-attachments/assets/9ec085c7-5835-4f45-a0cd-2df7e1370c79)
***
**Statistic**
![stat locust user 5](https://github.com/user-attachments/assets/0c82435e-793e-43ca-8901-d16440f4afab)
***
Performa tetap stabil dengan RPS naik menjadi sekitar 2.8 dan waktu respons sedikit meningkat namun masih dalam batas wajar, dengan median 330 ms dan 95th percentile 640 ms. Belum ada failure.
***

### 10 User
**Graph**
![chart locust user 10](https://github.com/user-attachments/assets/b0cfc10e-ebf4-452f-8803-4cd1ff95f2ce)
***
**Statistic**
![stat locust user 10](https://github.com/user-attachments/assets/4315d047-67a7-4a76-8682-c4de95e15246)
***
Sistem berhasil menangani 10 pengguna tanpa failure, tetapi RPS meningkat menjadi sekitar 5.4
Walaupun median waktu respons tetap rendah (240 ms), grafik 95th percentile mulai menunjukkan variabilitas dan lonjakan yang lebih tinggi dibandingkan skenario sebelumnya. Ini adalah tanda awal sistem mulai terbebani.
***

### 15 User
**Graph**
![chart locust user 15](https://github.com/user-attachments/assets/fcfb2e4a-d0d5-4850-a048-aeffa56331e0)
***
**Statistic**
![stat locust user 15](https://github.com/user-attachments/assets/8f565429-f69a-4400-b42a-66d166824a68)
***
Aplikasi masih berjalan tanpa mencatat kegagalan. Tetapi, grafik 95th percentile response time menunjukkan ketidakstabilan yang jelas, dengan lonjakan-lonjakan tajam yang sering terjadi, beberapa di antaranya mencapai di atas 3.000 ms. Ini mengindikasikan bahwa meskipun sistem tidak gagal, pengalaman pengguna (user experience) menurun drastis.
***

### 20 User (Attempt 1)
**Graph**
![chart locust user 20](https://github.com/user-attachments/assets/e09418c0-0ad6-4770-abf3-91ce7d31eebb)
***
**Statistic**
![stat locust user 20](https://github.com/user-attachments/assets/52d2b732-d1b4-4866-9a5a-4c7e9d54352e)
***
Pengujian ini menunjukkan kegagalan sistem secara total. RPS sempat naik lalu anjlok mendekati nol, menandakan server tidak lagi mampu merespons permintaan. Waktu respons melonjak sangat tinggi, dengan 95th percentile mencapai 119.000 ms (119 detik), yang tidak dapat diterima. Failure tercatat sebesar 14%.

### 20 User (Attempt 2 dan 3)
**Graph 1**
![chart locust user 20 rev](https://github.com/user-attachments/assets/9f3dfcb9-11b4-4a62-a9c0-c90b416b919d)
***
**Graph 2**
![chart locust uer 20 rev rev](https://github.com/user-attachments/assets/91fdf8ca-8078-42c9-bf2d-46c0800261c5)
***
**Statistic 1**
![stat locust user 20 rev](https://github.com/user-attachments/assets/e0a3276a-b378-4673-8615-54a66198fbac)
***
**Statistic 2**
![stat locust user 20 rev rev](https://github.com/user-attachments/assets/de30dc76-1f96-4dd3-ab25-f17bb5ffa4bd)
***
Pada percobaan 20 user ini, dilakukan clear database dengan harapan aplikasi dapat berjalan optimal. Namun meskipun database telah dibersihkan, sistem tetap tidak mampu menangani 20 pengguna secara stabil. Grafik RPS dan waktu respons menunjukkan pola "gergaji" (naik-turun secara drastis), menandakan sistem kewalahan dan hanya bisa memproses permintaan secara tidak teratur dan putus-putus. Waktu respons sangat berfluktuasi, dengan lonjakan hingga 12.000-13.000 ms.
***

**HTOP APP 1**
![Image](https://github.com/user-attachments/assets/1c8a1aef-0e89-4d61-aa81-f09b03ebbbf1)
***
Load Average: 0.05, 0.12, 0.04 (sangat rendah)
CPU Usage: Minimal load, system very stable
Memory: Sufficient, no memory pressure
Python Processes: Multiple uvicorn workers running efficiently

---

**HTOP APP 2**
![Image](https://github.com/user-attachments/assets/07b1453c-7bfd-40ef-9e84-7632a36cc042)
***
Load Average: 0.30, 0.08, 0.03 (rendah)
CPU Usage: Slightly higher than App1, masih dalam batas normal
Process Distribution: Load balancing working properly

---

**HTOP LOAD BALANCING**
![Image](https://github.com/user-attachments/assets/53807aaf-675f-4487-8c75-ca37813a735f)
***

Load Average: 0.00, 0.00, 0.00 (minimal)
CPU Usage: Nginx very efficient, almost no overhead
Network Handling: Optimal request distribution

📈 Faktor yang Mempengaruhi Response Time & RPS
✅ Faktor yang Meningkatkan Performance:
1. Load Distribution
Round-robin balancing memastikan request tersebar merata
No single server bottleneck karena ada 2 app servers
2. Application Server Optimization
FastAPI async framework → Non-blocking I/O operations
Uvicorn ASGI server → High-performance async server
Multiple workers → Concurrent request processing
3. Database Architecture
MongoDB GridFS → Efficient binary data (face images) storage
Indexed queries → Fast employee lookup
Dedicated database VM → No resource sharing with app logic
4. Network Optimization
Private network communication → Low latency antar VM
Minimal network hops → Direct communication paths
Azure network backbone → High-bandwidth, low-latency


⚠️ Potential Bottlenecks (Yang Perlu Diperhatikan):
1. Face Recognition Processing
DeepFace/TensorFlow computation → CPU intensive operations
Image processing → Memory and CPU overhead
Model loading → Initial model load time
2. Database I/O
Face image storage/retrieval → Large binary data transfer
Concurrent database connections → Connection pool managemen

---

## Kesimpulan
Proyek cloud ini merupakan sistem presensi berbasis pengenalan wajah yang telah diuji menggunakan load testing dengan Locust untuk mengevaluasi performa endpoint /api/recognize-face. Berdasarkan pengujian bertahap dengan jumlah pengguna simultan sebanyak 1, 3, 5, 10, 15, hingga 20 user, diperoleh hasil bahwa sistem berfungsi dengan baik hingga 15 pengguna, meskipun dengan penurunan performa yang signifikan (response time > 4 detik). Pada pengujian dengan 20 pengguna, sistem mengalami overload, ditandai dengan RPS mendekati nol dan tingkat kegagalan yang tinggi. Hal ini mengindikasikan bahwa sistem saat ini belum optimal untuk beban berat atau penggunaan skala besar.

## Saran perbaikan dan Optimasi
1. Optimasi Backend
- Gunakan server asynchronous seperti Uvicorn dengan multiple worker (--workers n) untuk meningkatkan kapasitas concurrent request.
- Pastikan semua proses berat seperti face recognition tidak dijalankan blocking (gunakan async jika memungkinkan).

2. Terapkan Sistem Antrian (Queueing)
- Gunakan sistem antrean (contoh: Celery + Redis) untuk menangani proses pengenalan wajah secara asynchronous, sehingga server tidak terbebani langsung oleh banyak request bersamaan.
