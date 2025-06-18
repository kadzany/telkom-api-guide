# API Contract BA Reconciliation 

### 1. API Overview 

API ini memberikan kapabilitas untuk membuat BA (Berita Acara) Rekoknsiliasi yang digunakan oleh mitra sebagai acuan untuk melakukan rekonsiliasi fee profit sharing yang menjadi haknya.

BA Rekonsiliasi diakses dari DTP (Digital Touch Point):
* FABD Product
* FABD Core (Order & CRX)
* Odissey 

 

### 2. Authentication and Authorization 

Authentication Method: Login via API FABD

Authorization: Token yang didapatkan setelah login harus disertakan dalam header permintaan sebagai Authorization: Bearer {token}. 

 

### 3. API Endpoint Definitions 

**Endpoint 1:** Post dari Order ke CRX

**URL:** `/crx/api/v1/cash`
 **Method:** `POST` 
 **Description:** Mensubmit data Cash berdasarkan data dari order. 
 **Parameters:** 
- `Request Body` (required): Data dari order yang ingin di submit ke CRX.

**Contoh request:**
```http 
POST /crx/api/v1/cash
```
```json 
{
  "orders": [
    {
      "ecosystem_code": "QADSZ",
      "channel_name": "MYIB",
      "payment_status": "paid",
      "activation_status": "delivered",
      "order_id": "0019d2e7-155b-4db6-b4ab-425318ec1882",
      "billing_id": "d3d7ad88-4ea3-4ad7-8f5f-95827e855320",
      "invoice_id": "29326c6c-74cc-40ba-aecc-1338818c496e",
      "payment_id": "2410280845FR8W9L",
      "order_created_at": "2024-10-23T20:41:30.872975+07:00",
      "subtotal_price": 10000,
      "subtotal_ppn": 1100,
      "subtotal_discount": 0,
      "total": 11100,
      "order_items": [
          {
            "service_id": "302792cf-8dfc-4b60-bb7f-8557b1a1d798",
            "price": 110,
            "qty": 1,
            "discount": 10,
            "discount_type": "amount", // amount or percentage
            "total_price": 110,
            "total_discount": 10,
            "total_price_after_discount": 100,
            "dpp": 100,
            "ppn": 12,
            "pph": 2,
            "is_include_ppn": true,
            "currency": "IDR"
        }
      ]
    }
  ]
}
```

**Contoh response:**
```json
{ 
  "status": "success", 
  "message": "Cash note created successfully."
} 
```

**Endpoint 2:** Update dari Odissey ke CRX

**URL:** `/crx/api/v1/cash/{paymentId} `
 **Method:** `PUT` 
 **Description:** Memperbarui data cash note berdasarkan paymentId. 
 **Parameters: **

- `paymentId` (path, required): ID payment yang unik. 
- `Request body` (required): Data yang ingin diperbarui. 

**Contoh request:**

```http 
PUT /crx/api/v1/cash/2410280845FR8W9L
```
```json 
Content-Type: application/json 
{
  "action": "verified-transaction",
  "data": {
    "verification_date": 1748399669,
    "collected_date": 1748278800,
    "verification_by": "Bisya A****a",
    "mps_telkom_trx_id": "ff82f5c1-9e5c-498b-af68-0871e1bf9687",
    "mps_merchant_trx_id": "7594d899-0da8-4e22-af4e-6c13e15e7d14",
    "mps_channel_trx_id": "2505271814MDZXQW",
    "nominal_settled": 109890
  }
}
```

**Contoh response:**
```json
{ 
  "status": "success", 
  "message": "Cash note updated successfully."
} 
```

 

### 4. Request and Response Format 

Request Format: JSON 

Response Format: JSON 

 

### 5. Error Handling 

Menjelaskan kode status HTTP yang dapat dikembalikan, beserta pesan kesalahan jika ada. Ini termasuk kesalahan umum seperti 400 (Bad Request), 401 (Unauthorized), 404 (Not Found), dan 500 (Internal Server Error). 

Error Codes: 

`400` - Bad Request: Jika ada parameter yang hilang atau tidak valid. 

`401` - Unauthorized: Token autentikasi tidak valid atau hilang. 

`404` - Not Found: Endpoint tidak ditemukan. 

`500` - Internal Server Error: Kesalahan di server. 

**Contoh response (Error):** 

```json  
{ 
  "status": "error", 
  "message": "User not found" 
} 
``` 

 

### 6. Rate Limiting 

Jika berlaku, menjelaskan pembatasan jumlah permintaan yang dapat dilakukan dalam periode waktu tertentu (misalnya, 1000 permintaan per menit). 

**Contoh:** 

API ini membatasi hingga 1000 permintaan per menit per pengguna. Jika batas terlampaui, akan dikembalikan status 429 (Too Many Requests). 

 

### 7. API Versioning 

API ini mendukung versi melalui prefiks URI, contoh: /api/v1/ untuk versi pertama dan /api/v2/ untuk versi kedua. 

 

### 8. OpenAPI Specification 

Dokumentasi API ini harus mematuhi format OpenAPI 3.0 untuk memastikan discoverability dan kemampuan generasi klien secara otomatis. Setiap layanan harus mempublikasikan spesifikasi API ini ke registri API internal. 

**Spesifikasi OpenAPI untuk API ini dapat ditemukan di:** `/api/v1/discovery/openapi.json. `

 
### 9. Security Considerations  

API ini hanya dapat diakses melalui HTTPS untuk memastikan bahwa semua komunikasi dienkripsi dengan baik. 

 

### 10. Deprecation Policy 

Versi lama API ini akan dihentikan dalam waktu 6 bulan setelah pengumuman, dengan pengumuman sebelumnya minimal 3 bulan. 

***
 