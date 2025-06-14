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
