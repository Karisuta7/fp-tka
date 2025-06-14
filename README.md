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
