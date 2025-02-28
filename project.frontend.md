## **Milestones Pengembangan Frontend dengan Astro**

### **Milestone 1: Setup Proyek Astro dan Konfigurasi Awal**

- **Tujuan**: Menyiapkan fondasi proyek frontend yang kokoh.
- **Tugas**:
  - Inisialisasi proyek Astro dengan dukungan **TypeScript**.
  - Instalasi dan konfigurasi **TailwindCSS** untuk styling yang responsif dan konsisten.
  - Membuat struktur direktori proyek:
    - `components/` untuk menyimpan komponen reusable seperti Button, Table, Card, Input, dll.
    - `pages/` untuk halaman aplikasi (misalnya, login, CMS, dashboard).
    - `layouts/` untuk template layout (misalnya, layout CMS dan dashboard).
    - `utils/` untuk fungsi utilitas seperti pengelolaan API atau error handling.
  - Mengatur routing dasar untuk halaman login, CMS Production Manager, dan dashboard Operator.

### **Milestone 2: Implementasi Autentikasi dan Akses Berdasarkan Peran**

- **Tujuan**: Memastikan pengguna diarahkan ke halaman sesuai peran mereka (Production Manager atau Operator).
- **Tugas**:
  - Membuat halaman login dengan form username dan password menggunakan komponen **Input** dan **Button** yang reusable.
  - Mengintegrasikan API login untuk mendapatkan token JWT dari backend.
  - Menyimpan token di **IndexedDB** untuk manajemen sesi pengguna.
  - Membuat logika untuk memeriksa peran pengguna dari token dan mengarahkan ke halaman CMS (Production Manager) atau dashboard (Operator).
  - Menambahkan proteksi rute agar hanya peran yang sesuai yang dapat mengakses halaman tertentu.

### **Milestone 3: Fitur Utama CMS dan Dashboard untuk Production Manager**

- **Tujuan**: Mengembangkan fitur inti untuk Production Manager.
- **Tugas**:
  - **Halaman Daftar Work Order**:
    - Menampilkan tabel dengan kolom seperti nomor, nama produk, jumlah, tenggat waktu, status, dan operator menggunakan komponen **Table** yang reusable.
    - Menambahkan filter berdasarkan status atau tanggal.
  - **Halaman Tambah Work Order**:
    - Form untuk menambah _work order_ baru dengan field seperti nama produk, jumlah, dan tenggat waktu, menggunakan komponen **Input** yang reusable.
    - Validasi input dan menampilkan pesan kesalahan melalui **Popup Card**.
  - **Halaman Edit Work Order**:
    - Form untuk mengedit _work order_ dengan komponen **Input** dan **Select** yang reusable.
    - Validasi dan error handling serupa dengan halaman tambah.
  - **Dashboard**:
    - Menampilkan ringkasan seperti jumlah _work order_ per status menggunakan komponen **Card**.
    - Desain responsif dengan **TailwindCSS**.

### **Milestone 4: Fitur untuk Akses Operator**

- **Tujuan**: Memberikan tampilan dan fungsi khusus untuk Operator.
- **Tugas**:
  - **Halaman Daftar Work Order yang Ditugaskan**:
    - Tabel _work order_ khusus untuk Operator yang login, dibangun dengan komponen **Table** yang reusable.
  - **Halaman Detail Work Order**:
    - Menampilkan detail _work order_ dan form untuk memperbarui status, menggunakan komponen **Input** dan **Button**.
    - Menambahkan field untuk mencatat _quantity_ atau keterangan progres (opsional).
    - Validasi input dan error handling dengan **Popup Card**.

### **Milestone 5: Error Handling dengan Popup Card**

- **Tujuan**: Memastikan pesan kesalahan ditampilkan secara user-friendly.
- **Tugas**:
  - Membuat komponen **Popup Card** untuk menampilkan pesan kesalahan atau keberhasilan.
  - Mengintegrasikan **Popup Card** pada setiap aksi penting seperti login, submit form, atau panggilan API yang gagal.
  - Menangani error dari API dan menampilkan pesan yang jelas kepada pengguna.

### **Milestone 6: Penggunaan IndexedDB untuk Browser Storage**

- **Tujuan**: Mengelola data sesi dan cache di sisi klien.
- **Tugas**:
  - Mengatur **IndexedDB** untuk menyimpan token JWT dan data sesi lainnya.
  - Membuat fungsi utilitas untuk mengakses (simpan, ambil, hapus) data dari **IndexedDB**.
  - Menyimpan data yang sering diakses (misalnya, daftar _work order_) di **IndexedDB** untuk mengurangi panggilan API.

---

## **Spesifikasi Teknis**

- **Framework**: **Astro** untuk frontend yang cepat dan efisien.
- **Styling**: **TailwindCSS** untuk desain yang konsisten dan responsif.
- **Komponen Reusable**: Button, Table, Card, Input, dll., untuk mempercepat pengembangan dan memudahkan perawatan.
- **TypeScript**: Untuk keamanan tipe data dan kemudahan debugging.
- **Error Handling**: Menggunakan **Popup Card** untuk menampilkan pesan kesalahan secara elegan.
- **Browser Storage**: **IndexedDB** untuk menyimpan data sesi dan cache lokal.
