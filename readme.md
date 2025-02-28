# Sistem Manajemen Work Order

## Deskripsi Proyek

Sistem Manajemen Work Order adalah aplikasi web yang dirancang untuk mengelola proses manufaktur secara efisien. Aplikasi ini memungkinkan pembuatan, pelacakan, dan pembaruan _work order_ dengan kontrol akses berbasis peran. Sistem ini dirancang untuk meningkatkan efisiensi operasional dan memberikan visibilitas yang lebih baik terhadap alur kerja produksi.

## Fitur Utama

- **Manajemen Work Order**: Membuat, memperbarui, dan melihat _work order_.
- **Kontrol Akses Berbasis Peran (RBAC)**:
  - **Production Manager**: Memiliki akses penuh untuk mengelola _work order_, menetapkan operator, dan melihat laporan.
  - **Operator**: Dapat melihat dan memperbarui status _work order_ yang ditugaskan.
- **Nomor Work Order Otomatis**: Setiap _work order_ diberi nomor unik dengan format `WO-YYYYMMDD-XXX`.
- **Pelacakan Status Work Order**: Status _work order_ dapat diperbarui oleh Operator (Pending → In Progress → Completed).
- **Responsif Frontend**: Antarmuka pengguna yang dirancang responsif menggunakan Astro dan TailwindCSS.
- **Backend API**: API backend dibangun dengan GoFiber, siap untuk integrasi frontend.

### Fitur Opsional

- **Pelacakan Progres Work Order**: Operator dapat mencatat progres tahapan produksi.
- **Laporan**: Rekapitulasi _work order_ dan laporan kinerja Operator.

## Peran Pengguna

1.  **Production Manager**
    - Membuat _work order_ baru dan menetapkan Operator.
    - Memperbarui detail _work order_ dan status.
    - Melihat daftar semua _work order_ dan laporan.
2.  **Operator**
    - Melihat daftar _work order_ yang ditugaskan.
    - Memperbarui status _work order_ dan mencatat _quantity_.
    - (Opsional) Mencatat progres produksi.

## Struktur Database

Sistem ini menggunakan struktur database relasional dengan tabel-tabel berikut:

1.  **Users**: Informasi pengguna (id, username, password, role).
2.  **WorkOrders**: Detail _work order_ (id, work_order_number, product_name, quantity, production_deadline, status, operator_id).
3.  **WorkOrderProgress** (Opsional): Pelacakan progres _work order_ (id, work_order_id, progress_description, timestamp).
4.  **WorkOrderStatusHistory** (Opsional): Riwayat status _work order_ (id, work_order_id, status, timestamp, quantity).

## Milestones Proyek

### Milestone 1: Desain Database

- Merancang skema database yang efisien.

### Milestone 2: Implementasi Autentikasi dan RBAC

- Mengembangkan sistem login dan kontrol akses berbasis peran.

### Milestone 3: Fitur Manajemen Work Order

- Mengembangkan fungsi inti untuk manajemen _work order_ untuk Production Manager dan Operator.

### Milestone 4: Pelacakan Progres Work Order (Opsional)

- Menambahkan fitur untuk melacak progres _work order_.

### Milestone 5: Laporan (Opsional)

- Menyediakan laporan rekapitulasi dan kinerja Operator.

## Teknologi yang Digunakan

- **Frontend**: Astro, TypeScript, TailwindCSS, IndexedDB
- **Backend**: Go, GoFiber, GORM (ORM), PostgreSQL
- **API**: REST API dengan otentikasi JWT

## Pengembangan Backend

- **Framework**: GoFiber
- **Database**: PostgreSQL
- **ORM**: GORM
- **Autentikasi**: JWT
- **Fitur**: Autentikasi, RBAC, Manajemen Work Order, (Opsional) Pelacakan Progres, (Opsional) Laporan
- **Endpoint**:
  - `/auth/login`
  - `/work-orders (POST, GET, PUT /{id})`
  - `/work-orders/{id}/progress (POST, GET)` (Opsional)
  - `/reports/summary, /reports/operators` (Opsional)

## Pengembangan Frontend

- **Framework**: Astro
- **Styling**: TailwindCSS
- **Manajemen State**: IndexedDB untuk sesi dan cache
- **Komponen**: Reusable components (Button, Table, Card, Input, Popup Card)
- **Fitur**:
  - Halaman Login
  - CMS untuk Production Manager (Daftar Work Order, Tambah/Edit Work Order)
  - Dashboard untuk Operator (Daftar Work Order Ditugaskan, Detail Work Order, Update Status)
  - Error Handling dengan Popup Card

## Pengembangan API (Mock API)

- **Base URL**: `http://api.workorder-system.mock/v1`
- **Autentikasi**: `/auth/login` (POST)
- **Work Orders**:
  - `/work-orders` (POST, GET) - Production Manager
  - `/work-orders/assigned` (GET) - Operator
  - `/work-orders/{id}` (PUT) - Production Manager & Operator (status update)
- **Progress Tracking (Opsional)**:
  - `/work-orders/{id}/progress` (POST, GET)
- **Reports (Opsional)**:
  - `/reports/summary` (GET)
  - `/reports/operators` (GET)
