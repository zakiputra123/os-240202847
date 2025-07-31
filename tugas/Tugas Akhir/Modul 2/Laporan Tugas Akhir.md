# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi

**Semester**: Genap / Tahun Ajaran 2024–2025

**Nama**: `<Zaki Saputra>`

**NIM**: `<240202847>`

---

## 📌 Deskripsi Singkat Tugas

Memodifikasi algoritma penjadwalan proses di sistem operasi xv6-public dari yang awalnya menggunakan metode Round Robin menjadi Priority Scheduling Non-Preemptive. Dalam perubahannya, ditambahkan field `priority` pada setiap proses untuk menyimpan nilai prioritas, serta pembuatan system call baru bernama `set_priority(int)` yang memungkinkan pengguna mengatur prioritas proses secara langsung. Selain itu, fungsi `scheduler` dimodifikasi agar supaya menjalankan proses `RUNNABLE` dengan prioritas tertinggi (yaitu proses dengan nilai prioritas paling kecil).  

---


## 🛠️ Rincian Implementasi

⦁	Modifikasi pada fungsi `scheduler()` di `proc.c` untuk mengganti algoritma Round Robin menjadi Non-Preemptive Priority Scheduling  
⦁	Menambahkan Field `priority` pada `Struct proc` di file `proc.h`  
⦁	Menambahkan fungsi `sys_set_priority()` di `sysproc.c` untuk mengambil argumen prioritas dari user dan menyimpannya ke field `priority`  
⦁	Menambahkan definisi syscall `SYS_set_priority` dengan nomor 22 di `syscall.h`  
⦁	Menambahkan deklarasi eksternal dan entri `syscall set_priority` di `syscall.c`  
⦁	menambah deklarasi `int set_priority(int priority);`  di user.h  
⦁	Menambahkan deklarasi `SYSCALL(set_priority)` di `usys.S` untuk mendefinisikan syscall  
⦁	Menambahkan implementasi fungsi `sys_set_priority()` di `sysproc.c` untuk mengatur nilai prioritas proses melalui system call  
⦁	Membuat program uji `ptest.c` untuk memverifikasi proses dengan prioritas lebih tinggi agar dijalankan lebih lebih dulu oleh `scheduler`  
⦁	menambahkan `_ptest` ke `Makefile` bagian `UPROGS=\`

---

## ✅ Uji Fungsionalitas

⦁	`ptest.c` untuk menguji apakah proses dengan prioritas lebih tinggi dieksekusi lebih dulu. Child 2 diberi prioritas 0, Child 1 prioritas 10.

---

## 📷 Hasil Uji
📍 Contoh Output `cowtest`:

```
$ ptest
Child 2 selesai
Child 1 selesai
Parent selesai
$ 
```
📷 screenshot  
---
![hasil  screenshot modul 2](https://github.com/user-attachments/assets//804ddd5f-e3ed-426a-aeec-91a6fb62ac96))
```

---

## ⚠️ Kendala yang Dihadapi

⦁	Output dari `ptest` sempat tidak sesuai harapan karena proses cetak `(printf)` dari beberapa proses tumpang tindih akibat eksekusi paralel tanpa sinkronisasi output  
⦁	Awalnya output menunjukkan urutan proses tidak sesuai prioritas karena delay `sleep()` belum diatur dengan benar untuk menghindari tabrakan antar proses.

---

## 📚 Referensi

⦁	Buku xv6 MIT: https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf  
⦁	Repositori xv6-public: https://github.com/mit-pdos/xv6-public  
---
