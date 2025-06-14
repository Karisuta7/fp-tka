# 💻 Final Project - TKA (fp-tka)

## 👥 Anggota Kelompok

| Nama                     | NRP        |
|--------------------------|------------|
| Adi Satria Pangestu      | 5027231043 |
| Ryan Adya Purwanto       | 5027231046 |
| Danar Bagus Rasendriya   | 5027231055 |
| Callista Meyra Azizah    | 5027231060 |
| Gandhi Ert Julio         | 5027231081 |

---

## 🧠 Arsitektur Final Project

![Arsitektur](https://github.com/user-attachments/assets/eb80ed94-b284-4493-a0e8-9b10638cc044)

### 📝 Penjelasan Detail Diagram

Diagram ini memvisualisasikan arsitektur tiga tingkat (**3-tier architecture**) yang modern dan scalable.

#### 🔹 Load Balancer (Tier 1 - Web Tier)
- **VM**: `vm1` seharga \$4/bulan  
- **Software**: Nginx  
- **Fungsi**: Ini adalah gerbang utama aplikasi. VM ini memiliki Public IP yang bisa diakses oleh pengguna dari internet. Tugasnya adalah menerima semua permintaan masuk dan meneruskannya secara seimbang ke salah satu dari dua App Server. Ini mencegah satu server menjadi terlalu terbebani.

#### 🔹 Application Servers (Tier 2 - Logic Tier)
- **VM**: 2 unit `vm3`, masing-masing seharga \$12/bulan (**Total \$24/bulan**)  
- **Software**: Backend (Python FastAPI) dan Frontend (React JS)  
- **Fungsi**: Ini adalah "otak" dari aplikasi. Mereka menjalankan kode yang menangani logika bisnis (seperti pengenalan wajah) dan berkomunikasi dengan database. Kedua server ini berada di **Jaringan Privat**, sehingga tidak bisa diakses langsung dari internet. Mereka hanya menerima traffic dari Load Balancer.

#### 🔹 Database Server (Tier 3 - Data Tier)
- **VM**: `vm2` seharga \$6/bulan  
- **Software**: MongoDB  
- **Fungsi**: Tempat penyimpanan semua data aplikasi. Seperti App Server, VM ini juga berada di Jaringan Privat dan hanya mengizinkan koneksi dari App Server. Ini adalah lapisan paling aman untuk melindungi data sensitif.

---

### 🔄 Alur Kerja (Workflow)
1. Seorang pengguna membuka aplikasi di browser mereka.
2. Permintaan dikirim melalui Internet ke Public IP dari Load Balancer.
3. Load Balancer menerima permintaan dan memilih salah satu App Server yang paling tidak sibuk.
4. App Server memproses permintaan. Jika perlu data, ia akan menghubungi Database Server.
5. Database Server mengembalikan data yang diminta ke App Server.
6. App Server mengirimkan respons melalui Load Balancer kembali ke pengguna.
---

### 🏗️ Rencana Arsitektur Aplikasi

Kita akan pakai **3 jenis server (total 4 VM)**:

1. **VM1 - Load Balancer**
   - Harga: \$4
   - ⚙Isi: Nginx
   - Fungsi: Bagi traffic ke server aplikasi.

2. **VM3 (x2) - App Server**
   - Harga: \$12 x 2 = \$24
   - ⚙Isi: FastAPI + React
   - Fungsi: Proses backend dan frontend.

3. **VM2 - Database Server**
   - Harga: \$6
   - Isi: MongoDB
   - Fungsi: Simpan semua data aplikasi.

**Keunggulan:**

- Scalable: Bisa tambah App Server kapan saja.
- High availability: Kalau 1 App Server mati, yang lain tetap jalan.
- Best practice industri: Cocok untuk aplikasi modern.

**Kekurangan:**

- Setup agak rumit, tapi masih manageable.

**Total biaya: \$34 / bulan** (masih jauh di bawah budget \$100)

---

### 🔁 Alur Permintaan User

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
