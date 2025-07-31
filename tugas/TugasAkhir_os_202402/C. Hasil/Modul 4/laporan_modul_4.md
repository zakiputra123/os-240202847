# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi

**Semester**: Genap / Tahun Ajaran 2024–2025

**Nama**: `<Zaki Saputra>`

**NIM**: `<240202847>`

**Modul yang Dikerjakan**:
`(Modul 4 – Subsistem Kernel Alternatif (xv6-public))`

---

## 📌 Deskripsi Singkat Tugas

* **Modul 4 – Subsistem Kernel Alternatif (xv6-public)**:
  Modul ini dilakukan modifikasi terhadap kernel xv6 dengan menambahkan dua fitur utama:
* System Call `chmod(path, mode)`
Digunakan untuk mengubah mode file menjadi hanya-baca (read-only) atau kembali ke mode normal (read-write). Fitur ini bertujuan memperkenalkan konsep pengaturan izin akses file di tingkat kernel.

* Perangkat Khusus `/dev/random`
Merupakan device file yang dapat dibaca untuk mendapatkan byte acak, berfungsi sebagai generator angka acak sederhana yang diimplementasikan langsung pada kernel, mirip seperti `/dev/random` di sistem Linux.
---

## 🛠️ Rincian Implementasi

1. System Call `chmod(path, mode)`
  Perubahan yang dilakukan meliputi:
* `sysfile.c` → Menambahkan fungsi `sys_chmod()` untuk mengubah nilai `mode` pada inode.
* `fs.c` → Memperbarui `writei()` agar menolak operasi tulis ketika file dalam keadaan hanya-baca.
* `file.c` → Mengintegrasikan logika write-blocking dari `writei()` ke dalam `filewrite()`.
* `user.h` → Menambahkan deklarasi fungsi `chmod()` agar bisa dipanggil dari program pengguna.
*`usys.S` → Menambahkan entri baru untuk syscall `chmod`.
* `syscall.c` & `syscall.h` → Mendaftarkan `SYS_chmod` dengan nomor syscall baru.
* `defs.h` → Menambahkan deklarasi fungsi `sys_chmod()` untuk digunakan di kernel.
* `fs.h` → Menambahkan field `mode` pada struktur `inode` guna menyimpan status mode file.
* `chmodtest.c` → Program uji untuk membuat file, mengubahnya menjadi hanya-baca, lalu mencoba menulis ke file tersebut.
* Makefile → Menambahkan `chmodtest` ke daftar program uji.
  
2. Perangkat `/dev/random`
  Perubahan yang dilakukan:
* `random.c` → Menyediakan fungsi `randomread()` yang mengembalikan byte acak.
* `file.c` & `fs.c` → Mendaftarkan device baru ke dalam array `devsw[]` sehingga pembacaan `/dev/random` memanggil `randomread()`.
* `sysfile.c` → Mendukung pembuatan perangkat ini melalui `mknod()`.
* `defs.h` → Menambahkan deklarasi `randomread` agar bisa digunakan di kernel.
* `init.c` → Menambahkan `mknod("/dev/random", 1, 3);` sehingga device otomatis tersedia saat boot.
* `randomtest.c` → Program uji untuk membaca byte acak dari `/dev/random` dan menampilkannya di layar.
Makefile → Menambahkan `randomtest` ke daftar program uji.

---

## ✅ Uji Fungsionalitas


* `chmodtest` → Berhasil memblokir penulisan pada file yang sudah diubah menjadi read-only.
* `randomtest` → Berhasil menghasilkan dan menampilkan byte acak dari `/dev/random`.
  
---

## 📷 Hasil Uji


### 📍 Output `randomtest`:

```
$ randomtest
47 1 47 1 47 1 47 1
```

### 📍 Output `chmodtest`:

```
$ chmodtest
Write blocked as expected
```


<img width="1366" height="768" alt="Screenshot (101)" src="https://github.com/user-attachments/assets/40e53007-ac61-43eb-ae04-5555dcfd60b4" />


---

## ⚠️ Kendala yang Dihadapi

* Awalnya proteksi tulis tidak berfungsi karena logika pengecekan di `writei()` belum tepat.
* Pernah lupa mendaftarkan syscall `chmod` di `syscall.c`, `syscall.h`, dan `usys.S` sehingga kernel tidak mengenalinya.
* `/dev/random` tidak muncul saat boot karena `mknod()` belum dipanggil di `init.c`.
* Pembacaan `/dev/random` gagal karena randomread belum terhubung dengan devsw[] di `file.c`.
* Sempat terjadi linker error karena `randomread()` belum dideklarasikan di `defs.h`.

---

## 📚 Referensi


* Buku xv6 MIT: [https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf](https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf)
* Repositori xv6-public: [https://github.com/mit-pdos/xv6-public](https://github.com/mit-pdos/xv6-public)

---
