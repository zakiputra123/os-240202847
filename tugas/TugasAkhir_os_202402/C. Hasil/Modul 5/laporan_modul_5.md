# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi

**Semester**: Genap / Tahun Ajaran 2024–2025

**Nama**: `<Zaki Saputra>`

**NIM**: `<240202847>`

**Modul yang Dikerjakan**:
`Modul 5 – Audit dan Keamanan Sistem`

---

## 📌 Deskripsi Singkat Tugas

Modul ini bertujuan untuk menambahkan fitur keamanan berupa audit logging terhadap setiap system call yang dilakukan dalam sistem xv6. Selain itu, disediakan syscall `get_audit_log()` yang hanya dapat diakses oleh proses dengan PID 1 untuk membaca log, guna menjaga kerahasiaan dan integritas data audit.

---

## 🛠️ Rincian Implementasi

rincian langkah-langkah implementasi:

### 🔧 Penambahan Fitur Audit Logging

* Menambahkan struktur `audit_entry` dan array `audit_log[]` di `syscall.c`
* Setiap kali terjadi syscall yang valid, sistem akan mencatat:

  * PID proses
  * Nomor syscall
  * Nilai `tick` saat itu (waktu global sistem)
* Audit dicatat maksimal sebanyak 128 entri (`MAX_AUDIT`)

### 🧩 Implementasi syscall `get_audit_log()`

* Menambahkan syscall baru `get_audit_log()` pada:

  * `sysproc.c`: implementasi kernel
  * `syscall.c`: registrasi ke tabel syscall
  * `user.h`: deklarasi syscall dan struct
  * `usys.S`: makro syscall
  * `syscall.h`: nomor syscall baru `SYS_get_audit_log`
* Perlindungan akses: syscall hanya akan berhasil jika dipanggil oleh proses dengan PID 1.

### 💡 Program Uji

* Menambahkan program `audit.c` untuk membaca log melalui syscall `get_audit_log()`
* Program ditambahkan ke bagian `UPROGS` di `Makefile`

---

## ✅ Uji Fungsionalitas

### 🔍 Program Uji:

* `audit`: membaca log system call yang telah tercatat

### 📌 Cara Uji:

* Jalankan `audit` dalam kondisi biasa → akan gagal karena bukan PID 1
* Jalankan `audit` sebagai proses init:

  * Ubah `init.c` untuk memanggil `exec("audit", argv)` sebelum shell default
  * Rebuild dan boot ulang sistem

---

## 📷 Hasil Uji

### 📍 Contoh Output jika `audit` dijalankan oleh PID 1:

```
=== Audit Log ===
[0] PID=1 SYSCALL=5 TICK=12
[1] PID=1 SYSCALL=6 TICK=13
[2] PID=1 SYSCALL=10 TICK=14
...
```

### 📍 Contoh Output jika dijalankan oleh proses biasa:

```
Access denied or error.
```

## 📷 Screenshot

<img width="852" height="730" alt="image" src="https://github.com/user-attachments/assets/ad64299e-809a-41ed-b54d-5df013163369" />

---

## ⚠️ Kendala yang Dihadapi

* Lupa melakukan validasi PID saat implementasi awal `get_audit_log()`, sehingga proses biasa sempat bisa mengakses log
* Buffer `buf` di syscall sempat gagal diakses karena kesalahan dalam penggunaan `argptr()`
* Lupa menambahkan `_audit` di bagian `Makefile`, sehingga `audit` tidak bisa dijalankan

---

## 📚 Referensi

* xv6 Book MIT: [https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf](https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf)
* Repositori xv6-public: [https://github.com/mit-pdos/xv6-public](https://github.com/mit-pdos/xv6-public)
* Diskusi asisten praktikum dan dokumentasi kode
* Stack Overflow: berbagai error terkait pointer dan syscall di kernel

---
