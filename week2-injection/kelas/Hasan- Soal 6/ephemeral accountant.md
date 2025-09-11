# Juice-Shop Write-up: Login Admin (SQL Injection)

## Challenge Overview

**Title:** Login Admin (SQL Injection)  
**Category:** Injection  
**Difficulty:** ⭐⭐⭐⭐ (4/6)

Tujuan dari challenge ini adalah mengeksploitasi SQL Injection pada form login untuk membuat akun palsu dengan hak akses **Accounting** atau login sebagai user khusus.

---

## Tools Used

- **Burp Suite** → untuk intercept dan modifikasi request.  
- **Web Browser (Chrome/Firefox)** → untuk uji coba login.  

---

## Methodology and Solution

### Step 1: Identifying Injection Point

- Pada halaman **Login**, field `email` terdeteksi rentan terhadap SQL Injection.  
- Payload uji coba dimasukkan ke dalam kolom email.  

---

### Step 2: Crafting the Injection Payload

Payload yang digunakan untuk bypass login:  

```sql
' UNION SELECT * FROM (SELECT 20 AS `id`, 'acc0unt4nt@juice-sh.op' AS `email`, 'set1234' AS `password`, 'acc0unt4nt@juice-sh.op' AS `username`, 'set1234' AS `deluxeToken`, '1.2.3.4' AS `lastLoginIp`, '/assets/public/images/uploads/default.svg' AS `profileImage`, '2024-01-01' AS `createdAt`, NULL AS `deletedAt`)--
```

- Payload ini membuat **akun palsu** dengan email `acc0unt4nt@juice-sh.op` dan password `set1234`.  
- Disisipkan langsung ke query login sehingga server menerima hasil UNION seolah-olah akun tersebut valid.  
<img width="1917" height="1078" alt="step 1" src="https://github.com/user-attachments/assets/77472398-3993-408a-8c27-e0400f72d95c" />



---

### Step 3: Intercepting Request in Burp Suite

- Dengan **Burp Suite**, request login diubah agar payload masuk ke field `email`.  
- Response menunjukkan adanya token autentikasi valid, menandakan login berhasil.  
<img width="1902" height="1067" alt="payload" src="https://github.com/user-attachments/assets/2ad66a3a-1c85-4e80-9e3a-bc6f8aeffa69" />



---

### Step 4: Successful Login

- Setelah login berhasil, akun `acc0unt4nt@juice-sh.op` muncul di menu profil aplikasi.  
- Ini membuktikan bahwa SQL Injection berhasil mem-bypass sistem otentikasi.  
<img width="1918" height="1073" alt="selesai" src="https://github.com/user-attachments/assets/74733308-5479-463f-aa55-9622d532c069" />



---

## Solution Explanation

Challenge ini berhasil diselesaikan dengan memanfaatkan **SQL Injection** pada form login untuk membuat akun palsu dengan role tertentu. Teknik ini memanfaatkan query UNION SELECT untuk menyuntikkan data akun palsu langsung ke dalam hasil query.

---

## Remediation

Agar kerentanan serupa tidak terjadi:

- Gunakan **prepared statements** / parameterized queries.  
- Lakukan **input sanitization** pada setiap field login.  
- Terapkan **rate limiting** dan monitoring query abnormal.  
