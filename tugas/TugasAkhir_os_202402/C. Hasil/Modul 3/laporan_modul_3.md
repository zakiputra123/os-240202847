# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi

**Semester**: Genap / Tahun Ajaran 2024–2025

**Nama**: `<Zaki Saputra>`

**NIM**: `<240202847>`

**Modul yang Dikerjakan**:
`Modul 3 – Manajemen Memori Tingkat Lanjut (Copy-on-Write & Shared Memory)`

---

## 📌 Deskripsi Singkat Tugas

Pada modul ini, saya mengimplementasikan dua fitur manajemen memori di dalam kernel xv6:

1. **Copy-on-Write Fork (CoW)**
   Proses `fork()` dimodifikasi agar tidak langsung menyalin seluruh memori. Salinan halaman dilakukan hanya saat proses anak atau induk melakukan penulisan (*write*) terhadap halaman memori bersama. Teknik ini menggunakan reference counting dan penanganan *page fault*.

2. **Shared Memory ala System V**
   Proses dapat berbagi memori menggunakan `shmget(int key)` dan melepaskannya dengan `shmrelease(int key)`. Fitur ini memungkinkan komunikasi antar proses melalui halaman memori yang sama, dengan manajemen reference count.

---

## 🛠️ Rincian Implementasi

### Copy-on-Write Fork (CoW)

* Menambahkan array `ref_count[]` global di `vm.c` untuk menyimpan jumlah referensi setiap halaman fisik.
* Membuat fungsi `incref()` dan `decref()` untuk mengatur refcount dan `kfree()`.
* Menambahkan flag baru `PTE_COW` di `mmu.h`.
* Memodifikasi fungsi `copyuvm()` menjadi `cowuvm()` di `vm.c` untuk membagi halaman dan menandai COW.
* Mengganti `copyuvm()` dengan `cowuvm()` dalam `fork()` di `proc.c`.
* Menangani *page fault* (`T_PGFLT`) di `trap.c`, termasuk menyalin halaman baru jika ada penulisan pada halaman COW.

### Shared Memory ala System V

* Menambahkan array `shmtab[]` di `vm.c` untuk menyimpan key, frame, dan refcount.
* Menambahkan syscall `shmget(int key)` dan `shmrelease(int key)` di `sysproc.c`.
* Melakukan mapping halaman shared memory di area tinggi (`USERTOP`) untuk setiap proses.
* Registrasi syscall pada `syscall.c`, `syscall.h`, `user.h`, dan `usys.S`.

---

## ✅ Uji Fungsionalitas

Program uji yang digunakan:

* `cowtest`: Menguji Copy-on-Write pada proses `fork()`
* `shmtest`: Menguji penggunaan `shmget()` dan `shmrelease()` antar proses

---

## 📷 Hasil Uji

### 📍 Contoh Output `cowtest`:

```
Child sees: Y
Parent sees: X
```

✅ Artinya, salinan halaman hanya dilakukan saat proses anak menulis.

---

### 📍 Contoh Output `shmtest`:

```
Child reads: A
Parent reads: B
```

✅ Menunjukkan bahwa kedua proses membaca dan menulis ke halaman shared memory yang sama.

---

## Screenshot
### `cowtest`:
<img width="900" height="606" alt="image" src="https://github.com/user-attachments/assets/1d975daf-5a8d-47e0-a540-9f3c7cb9c2b9" />

### `shmtest`:
<img width="1009" height="638" alt="image" src="https://github.com/user-attachments/assets/5859c6b7-da9b-42fc-b8d0-b17d0fbaa518" />


## ⚠️ Kendala yang Dihadapi

* **Page Fault Error**: Salah pengecekan flag `PTE_COW` menyebabkan proses mati karena invalid access.
* **Kehilangan Referensi**: Lupa menambahkan `incref()` saat memetakan halaman membuat halaman di-*free* sebelum waktunya.
* **Alamat Map Salah**: Awalnya memetakan shared memory di alamat yang sudah digunakan proses (bukan di `USERTOP`), menyebabkan overwrite memori.
* **Keliru Return Value di Syscall**: `shmget()` sempat salah dikembalikan sebagai `int` bukan `void*`.

---

## 📚 Referensi

* Buku xv6 MIT: [https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf](https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf)
* Repositori xv6-public: [https://github.com/mit-pdos/xv6-public](https://github.com/mit-pdos/xv6-public)
* Stack Overflow, GitHub Issues, diskusi praktikum
* Chat GPT

---
