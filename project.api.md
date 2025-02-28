## **Deskripsi Sistem**

Sistem Manajemen Work Order adalah aplikasi web untuk mengelola proses manufaktur, termasuk pembuatan, pelacakan, dan pembaruan _work order_. Sistem ini mendukung:

- **Peran Pengguna**:
  - **Production Manager**: Membuat, memperbarui, dan melihat semua _work order_ serta laporan (opsional).
  - **Operator**: Melihat dan memperbarui _work order_ yang ditugaskan kepadanya.
- **Fitur Utama**: Manajemen _work order_.
- **Fitur Opsional**: Pelacakan progres dan laporan.

---

## **Struktur Database**

Berikut adalah struktur database yang menjadi dasar mock API:

1. **Tabel Users**: Informasi pengguna dan peran.
   - `id`, `username`, `password`, `role` ("Production Manager" atau "Operator").
2. **Tabel WorkOrders**: Detail _work order_.
   - `id`, `work_order_number` (otomatis, misal: WO-20240226-001), `product_name`, `quantity`, `production_deadline`, `status` ("Pending", "In Progress", "Completed", "Canceled"), `operator_id`.
3. **Tabel WorkOrderProgress** (Opsional): Pelacakan progres.
   - `id`, `work_order_id`, `progress_description`, `timestamp`.
4. **Tabel WorkOrderStatusHistory** (Opsional): Riwayat status.
   - `id`, `work_order_id`, `status`, `timestamp`, `quantity`.

---

## **Mock API**

Berikut adalah spesifikasi mock API yang mencakup endpoint untuk autentikasi, manajemen _work order_, dan fitur opsional.

### **Base URL**

```
http://api.workorder-system.mock/v1
```

### **1. Autentikasi**

#### **POST /auth/login**

- **Deskripsi**: Login pengguna untuk mendapatkan token autentikasi.
- **Hak Akses**: Semua pengguna.
- **Request Body**:
  ```json
  {
    "username": "john_doe",
    "password": "password123"
  }
  ```
- **Response (Success)**:
  - Status: `200 OK`
  - Body:
    ```json
    {
      "status": "success",
      "data": {
        "token": "jwt_token_example",
        "user": {
          "id": 1,
          "username": "john_doe",
          "role": "Production Manager"
        }
      }
    }
    ```
- **Response (Error)**:
  - Status: `401 Unauthorized`
  - Body:
    ```json
    {
      "status": "error",
      "message": "Invalid credentials"
    }
    ```

Catatan: Token diperlukan untuk semua endpoint berikutnya melalui header `Authorization: Bearer <token>`.

---

### **2. Manajemen Work Order**

#### **POST /work-orders**

- **Deskripsi**: Membuat _work order_ baru.
- **Hak Akses**: Production Manager.
- **Request Body**:
  ```json
  {
    "product_name": "Gearbox",
    "quantity": 50,
    "production_deadline": "2024-03-10T12:00:00Z",
    "operator_id": 2
  }
  ```
- **Response (Success)**:
  - Status: `201 Created`
  - Body:
    ```json
    {
      "status": "success",
      "data": {
        "id": 1,
        "work_order_number": "WO-20240226-001",
        "product_name": "Gearbox",
        "quantity": 50,
        "production_deadline": "2024-03-10T12:00:00Z",
        "status": "Pending",
        "operator_id": 2
      }
    }
    ```
- **Catatan**: `work_order_number` dihasilkan otomatis (format: WO-YYYYMMDD-XXX).

#### **GET /work-orders**

- **Deskripsi**: Mendapatkan daftar _work order_.
- **Hak Akses**:
  - Production Manager: Semua _work order_.
  - Operator: Hanya _work order_ yang ditugaskan kepadanya.
- **Query Params** (Opsional untuk Production Manager):
  - `status`: Filter berdasarkan status (misal: "Pending").
  - `date`: Filter berdasarkan tanggal (misal: "2024-02-26").
- **Response (Success)**:
  - Status: `200 OK`
  - Body:
    ```json
    {
      "status": "success",
      "data": [
        {
          "id": 1,
          "work_order_number": "WO-20240226-001",
          "product_name": "Gearbox",
          "quantity": 50,
          "production_deadline": "2024-03-10T12:00:00Z",
          "status": "Pending",
          "operator_id": 2
        }
      ]
    }
    ```

#### **PUT /work-orders/{id}**

- **Deskripsi**: Memperbarui _work order_.
- **Hak Akses**:
  - Production Manager: Mengubah semua field.
  - Operator: Hanya mengubah `status` dan mencatat `quantity`.
- **Request Body (Production Manager)**:
  ```json
  {
    "product_name": "Gearbox V2",
    "quantity": 60,
    "production_deadline": "2024-03-15T12:00:00Z",
    "status": "In Progress",
    "operator_id": 3
  }
  ```
- **Request Body (Operator)**:
  ```json
  {
    "status": "In Progress",
    "quantity": 20
  }
  ```
- **Response (Success)**:
  - Status: `200 OK`
  - Body:
    ```json
    {
      "status": "success",
      "data": {
        "id": 1,
        "work_order_number": "WO-20240226-001",
        "product_name": "Gearbox V2",
        "quantity": 60,
        "production_deadline": "2024-03-15T12:00:00Z",
        "status": "In Progress",
        "operator_id": 3
      }
    }
    ```
- **Catatan**: Operator hanya dapat mengubah status dalam urutan: "Pending" → "In Progress" → "Completed".

---

### **3. Pelacakan Progres Work Order (Opsional)**

#### **POST /work-orders/{id}/progress**

- **Deskripsi**: Mencatat progres untuk _work order_.
- **Hak Akses**: Operator.
- **Request Body**:
  ```json
  {
    "progress_description": "Pemotongan Selesai"
  }
  ```
- **Response (Success)**:
  - Status: `201 Created`
  - Body:
    ```json
    {
      "status": "success",
      "data": {
        "id": 1,
        "work_order_id": 1,
        "progress_description": "Pemotongan Selesai",
        "timestamp": "2024-02-26T10:00:00Z"
      }
    }
    ```

#### **GET /work-orders/{id}/progress**

- **Deskripsi**: Mendapatkan daftar progres untuk _work order_.
- **Hak Akses**: Production Manager dan Operator.
- **Response (Success)**:
  - Status: `200 OK`
  - Body:
    ```json
    {
      "status": "success",
      "data": [
        {
          "id": 1,
          "work_order_id": 1,
          "progress_description": "Pemotongan Selesai",
          "timestamp": "2024-02-26T10:00:00Z"
        }
      ]
    }
    ```

---

### **4. Laporan (Opsional)**

#### **GET /reports/summary**

- **Deskripsi**: Rekapitulasi _work order_ berdasarkan status.
- **Hak Akses**: Production Manager.
- **Response (Success)**:
  - Status: `200 OK`
  - Body:
    ```json
    {
      "status": "success",
      "data": {
        "Pending": {
          "product_name": "Gearbox",
          "quantity": 50
        },
        "In Progress": {
          "product_name": "Gearbox V2",
          "quantity": 60
        },
        "Completed": {
          "product_name": "Gearbox V3",
          "quantity": 100
        },
        "Canceled": {}
      }
    }
    ```

#### **GET /reports/operators**

- **Deskripsi**: Laporan hasil per Operator (status "Completed").
- **Hak Akses**: Production Manager.
- **Response (Success)**:
  - Status: `200 OK`
  - Body:
    ```json
    {
      "status": "success",
      "data": [
        {
          "operator_id": 2,
          "results": {
            "product_name": "Gearbox",
            "quantity": 100
          }
        }
      ]
    }
    ```

---

## **Catatan Tambahan**

- **Header Autentikasi**: Semua endpoint kecuali `/auth/login` memerlukan header `Authorization: Bearer <token>`.
- **Format Nomor Work Order**: Otomatis dengan pola `WO-YYYYMMDD-XXX`.
- **Validasi Status**: Operator hanya dapat mengubah status dalam urutan yang valid (Pending → In Progress → Completed).
- **Fitur Opsional**: Endpoint untuk pelacakan progres dan laporan dapat diaktifkan sesuai kebutuhan.
