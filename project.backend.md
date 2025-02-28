## **Milestones untuk Pengembangan Backend**

### **Milestone 1: Setup Proyek dan Konfigurasi Awal**

- **Tujuan**: Menyiapkan fondasi proyek backend agar siap untuk pengembangan.
- **Tugas**:
  - Inisialisasi proyek Go dengan perintah `go mod init`.
  - Instalasi framework GoFiber untuk menangani permintaan HTTP.
  - Membuat struktur direktori proyek yang rapi, seperti:
    - `controllers/` untuk logika endpoint.
    - `models/` untuk definisi data.
    - `routes/` untuk konfigurasi routing.
    - `middleware/` untuk fungsi tengah seperti autentikasi.
    - `database/` untuk koneksi dan migrasi database.
  - Mengatur koneksi ke database relasional (misalnya, PostgreSQL atau MySQL) menggunakan library seperti `gorm`.
  - Membuat migrasi database untuk tabel dasar seperti `Users` dan `WorkOrders`.

### **Milestone 2: Implementasi Autentikasi dan RBAC**

- **Tujuan**: Membuat sistem autentikasi dan otorisasi berbasis peran untuk mengamankan akses.
- **Tugas**:
  - Membuat model `User` dengan kolom `id`, `username`, `password`, dan `role`.
  - Mengembangkan endpoint registrasi (`POST /register`) untuk admin atau Production Manager.
  - Mengembangkan endpoint login (`POST /login`) yang menghasilkan token JWT.
  - Membuat _middleware_ autentikasi untuk memverifikasi token JWT pada setiap permintaan yang dilindungi.
  - Membuat _middleware_ RBAC untuk memastikan akses hanya diberikan sesuai peran (misalnya, Production Manager atau Operator).

### **Milestone 3: Fitur Manajemen Work Order**

- **Tujuan**: Mengembangkan fungsi inti untuk mengelola _work order_ berdasarkan peran pengguna.
- **Tugas**:
  - **Untuk Production Manager**:
    - Endpoint `POST /workorders`: Menambah _work order_ baru dengan validasi input (nama produk, jumlah, tenggat waktu, operator).
      - Generate nomor _work order_ otomatis (contoh: `WO-20231101-001`).
      - Simpan dengan status awal "Pending".
    - Endpoint `PUT /workorders/:id`: Memperbarui detail _work order_ (status, operator, dll.), hanya untuk Production Manager.
    - Endpoint `GET /workorders`: Menampilkan daftar _work order_ dengan filter status atau tanggal, serta dukungan _pagination_.
  - **Untuk Operator**:
    - Endpoint `GET /workorders/assigned`: Menampilkan _work order_ yang ditugaskan ke Operator yang login.
    - Endpoint `PUT /workorders/:id/status`: Memperbarui status _work order_ (misalnya, "Pending" ke "In Progress" atau "Completed") oleh Operator yang ditugaskan, dengan pencatatan _quantity_.

### **Milestone 4: Pelacakan Progres Work Order (Opsional)**

- **Tujuan**: Menambahkan kemampuan untuk melacak progres _work order_.
- **Tugas**:
  - Endpoint `POST /workorders/:id/progress`: Operator mencatat keterangan progres (disimpan di tabel `WorkOrderProgress` dengan _timestamp_).
  - Merekam riwayat status dan _quantity_ di tabel `WorkOrderStatusHistory` saat status diperbarui.

### **Milestone 5: Laporan (Opsional)**

- **Tujuan**: Menyediakan laporan berbasis data _work order_.
- **Tugas**:
  - Endpoint `GET /reports/workorders`: Menampilkan rekapitulasi _work order_, termasuk total _quantity_ per status.
  - Endpoint `GET /reports/operators`: Menampilkan laporan hasil kerja Operator berdasarkan _work order_ yang diselesaikan.

---

## **Rincian Teknis**

- **Framework**: GoFiber untuk routing dan penanganan HTTP.
- **Database**: PostgreSQL dengan ORM `gorm` untuk kemudahan akses data.
- **Autentikasi**: Token JWT untuk mengamankan endpoint.
- **RBAC**: Logika peran diatur melalui _middleware_.
- **Validasi**: Menggunakan fitur validasi GoFiber atau library seperti `go-playground/validator`.
- **Keamanan**: Password dienkripsi dengan `bcrypt`.

---
