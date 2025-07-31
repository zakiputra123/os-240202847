# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi

**Semester**: Genap / Tahun Ajaran 2024–2025

**Nama**: `<Zaki Saputra>`

**NIM**: `<240202847>`

**Modul yang Dikerjakan**:
`Modul 1 – System Call dan Instrumentasi Kernel`
---

## 📌 Deskripsi Singkat Tugas

Modul ini bertujuan untuk menambah dua buah system call baru pada kernel **xv6-public**, yaitu:

* `getpinfo(struct pinfo *ptable)`: Mengembalikan informasi proses-proses aktif, termasuk PID, ukuran memori, dan nama proses.
* `getReadCount()`: Mengembalikan jumlah pemanggilan system call `read()` sejak sistem boot.

---

## 🛠️ Rincian Implementasi

Langkah-langkah implementasi yang dilakukan:

* **Struktur `pinfo`** ditambahkan pada `proc.h` untuk menyimpan informasi proses aktif.
* **Counter `readcount`** ditambahkan secara global di `sysproc.c` untuk mencatat jumlah `read()`.
* Menambahkan **nomor syscall baru** di `syscall.h`: `SYS_getpinfo` dan `SYS_getreadcount`.
* Menambahkan **deklarasi syscall** di `user.h` dan **entri syscall** di `usys.S`.
* Mendaftarkan syscall pada tabel di `syscall.c` dan menambahkan implementasi fungsi handler `sys_getpinfo()` dan `sys_getreadcount()` di `sysproc.c`.
* Menambahkan increment `readcount++` di fungsi `sys_read()` pada file `sysfile.c`.
* Membuat dua program uji user-level:

  * `ptest.c`: menguji syscall `getpinfo()`
  * `rtest.c`: menguji syscall `getreadcount()`
* Menambahkan file uji ke `Makefile` pada bagian `UPROGS`.

---

## ✅ Uji Fungsionalitas

Program uji yang dijalankan:

* `ptest` → Berhasil menampilkan daftar proses aktif beserta PID, memori, dan nama proses.
* `rtest` → Berhasil mencatat dan menampilkan perubahan jumlah pemanggilan `read()`.

---

## 📷 Hasil Uji

### 📍 Contoh Output `cowtest`:
```
PID	MEM	NAME
1	4096	init
2	2048	sh
3	2048	ptest
```
Child sees: Y
Parent sees: X
```

### 📍 Contoh Output `shmtest`:

```
Read Count Sebelum: 4
hello
Read Count Setelah: 5
```

## Screenshot
<WhatsApp Image 2025-07-31 at 17.01.33_a132d466.jpg />

## ⚠️ Kendala yang Dihadapi

Beberapa kendala yang ditemui:

* Awalnya terjadi `segmentation fault` saat menggunakan `argptr()` karena kesalahan dalam cast pointer.
* Lupa menggunakan `acquire()` dan `release()` pada `ptable.lock`, yang menyebabkan data proses tidak konsisten.
* Harus menyesuaikan kembali deklarasi forward struct `pinfo` di `user.h`.
* Lupa meng-include spinlock.h, menyebabkan error saat kompilasi.

---

## 📚 Referensi

Buku xv6 MIT: https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf
* Repositori xv6-public: https://github.com/mit-pdos/xv6-public
* Stack Overflow, GitHub Issues, diskusi praktikum
* Chat GPT 
