## **Milestones dan Tugas Proyek**

Sistem Manajemen Work Order ini dirancang sebagai aplikasi web untuk mengelola proses manufaktur, termasuk pembuatan, pelacakan, dan pembaruan _work order_. Sistem ini menggunakan _Role-Based Access Control_ (RBAC) dengan dua peran pengguna: **Production Manager** dan **Operator**, yang masing-masing memiliki hak akses berbeda. Fitur utama mencakup manajemen _work order_, sedangkan fitur opsional meliputi pelacakan progres dan laporan.

---

## **Struktur Database**

Sebelum menentukan langkah-langkah proyek, berikut adalah rancangan awal struktur database untuk mendukung sistem ini:

1. **Tabel Users**

   - `id` (Primary Key)
   - `username` (unik)
   - `password` (terenkripsi)
   - `role` (enum: "Production Manager", "Operator")

2. **Tabel WorkOrders**

   - `id` (Primary Key)
   - `work_order_number` (unik, otomatis, contoh: WO-20240226-001)
   - `product_name` (string)
   - `quantity` (integer)
   - `production_deadline` (datetime)
   - `status` (enum: "Pending", "In Progress", "Completed", "Canceled")
   - `operator_id` (Foreign Key ke Tabel Users)

3. **Tabel WorkOrderProgress** (Opsional - untuk pelacakan progres)

   - `id` (Primary Key)
   - `work_order_id` (Foreign Key ke Tabel WorkOrders)
   - `progress_description` (string, misalnya: "Pemotongan Selesai")
   - `timestamp` (datetime)

4. **Tabel WorkOrderStatusHistory** (Opsional - untuk mencatat waktu dan _quantity_)
   - `id` (Primary Key)
   - `work_order_id` (Foreign Key ke Tabel WorkOrders)
   - `status` (enum: "Pending", "In Progress", "Completed", "Canceled")
   - `timestamp` (datetime)
   - `quantity` (integer, jumlah yang dicatat saat perubahan status)

Struktur ini akan menjadi dasar untuk semua fitur yang direncanakan.

---

## **Milestones dan Tugas Proyek**

Berikut adalah kerangka kerja dalam bentuk _project management_ yang terdiri dari _milestones_ utama dan tugas-tugas terkait:

### **Milestone 1: Desain Database**

- **Tujuan**: Membuat skema database untuk mendukung sistem.
- **Tugas**:
  - Merancang tabel `Users` untuk menyimpan informasi pengguna dan peran.
  - Merancang tabel `WorkOrders` untuk menyimpan detail _work order_.
  - (Opsional) Merancang tabel `WorkOrderProgress` untuk pelacakan progres.
  - (Opsional) Merancang tabel `WorkOrderStatusHistory` untuk mencatat riwayat status dan _quantity_.

### **Milestone 2: Implementasi Autentikasi dan RBAC**

- **Tujuan**: Mengatur sistem login dan kontrol akses berdasarkan peran.
- **Tugas**:
  - Membuat sistem autentikasi (_login_ dan _logout_).
  - Mengimplementasikan logika RBAC untuk memverifikasi peran pengguna saat login.
  - Mengatur hak akses:
    - **Production Manager**: Membuat, memperbarui, dan melihat laporan _work order_.
    - **Operator**: Melihat dan memperbarui _work order_ yang ditugaskan.

### **Milestone 3: Fitur Manajemen Work Order**

- **Tujuan**: Mengembangkan fitur utama untuk mengelola _work order_.
- **Tugas**:
  - **Untuk Production Manager**:
    - Membuat form untuk menambah _work order_ baru dengan field:
      - Nomor Work Order (otomatis, format: WO-YYYYMMDD-XXX).
      - Nama Produk.
      - Jumlah.
      - Tenggat Waktu Produksi.
      - Status (default: "Pending").
      - Operator yang ditugaskan (pilihan dari daftar Operator).
    - Mengimplementasikan logika pembuatan nomor _work order_ otomatis.
    - Membuat form untuk memperbarui _work order_ (mengubah status, menetapkan ulang operator, dll.).
    - Membuat tampilan daftar _work order_ dengan filter (berdasarkan status atau tanggal).
  - **Untuk Operator**:
    - Membuat tampilan daftar _work order_ yang ditugaskan ke Operator tertentu.
    - Membuat form untuk memperbarui status _work order_ (dari "Pending" ke "In Progress", atau "In Progress" ke "Completed") dengan field tambahan untuk mencatat _quantity_.

### **Milestone 4: Pelacakan Progres Work Order (Opsional)**

- **Tujuan**: Menambahkan fitur pelacakan progres pada _work order_.
- **Tugas**:
  - Membuat form untuk Operator mencatat keterangan tahapan produksi saat status "In Progress" (contoh: "Pemotongan Selesai").
  - Mengimplementasikan logika pencatatan otomatis waktu pada setiap perubahan status (disimpan di `WorkOrderStatusHistory`).

### **Milestone 5: Laporan (Opsional)**

- **Tujuan**: Menyediakan laporan untuk analisis _work order_.
- **Tugas**:
  - Mengembangkan laporan rekapitulasi _work order_ dengan kolom:
    - Nama Barang.
    - Total _quantity_ untuk setiap status (Pending, In Progress, Completed, Canceled).
  - Mengembangkan laporan hasil tiap Operator dengan kolom:
    - Nama Barang.
    - Total _quantity_ dengan status "Completed".
  - Membuat tampilan untuk menampilkan laporan dalam bentuk tabel atau grafik.

---

## **Rincian Hak Akses Berdasarkan Peran**

- **Production Manager**:
  - Membuat _work order_ baru.
  - Menetapkan Operator ke _work order_.
  - Memperbarui detail _work order_ (status, operator, dll.).
  - Melihat daftar semua _work order_ dengan filter.
  - (Opsional) Melihat laporan rekapitulasi dan hasil Operator.
- **Operator**:
  - Melihat daftar _work order_ yang ditugaskan kepadanya.
  - Memperbarui status _work order_ (Pending → In Progress → Completed) dan mencatat _quantity_.
  - (Opsional) Mencatat keterangan tahapan produksi saat status "In Progress".

---

## **Catatan Tambahan**

- **Nomor Work Order Otomatis**: Format seperti `WO-20240226-001` dapat dihasilkan dengan kombinasi tanggal (YYYYMMDD) dan nomor urut harian (XXX).
- **Status Work Order**: Hanya status yang valid (Pending, In Progress, Completed, Canceled) yang diperbolehkan, dengan transisi status oleh Operator terbatas pada Pending → In Progress → Completed.
- **Fitur Opsional**: Pelacakan progres dan laporan dapat ditambahkan sebagai tahap terpisah setelah fitur utama selesai.
