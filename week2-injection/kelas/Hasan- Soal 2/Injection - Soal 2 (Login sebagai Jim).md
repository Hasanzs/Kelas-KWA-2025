# Injection - Soal 2 (Login sebagai Jim)

## 1. Soal
Login ke akun milik user **Jim** tanpa menggunakan kredensial password normal.

---

## 2. Link Resource
- [OWASP Juice Shop](https://owasp-juice.shop)
- [SQL Injection Cheat Sheet - PortSwigger](https://portswigger.net/web-security/sql-injection/cheat-sheet)
- [PayloadsAllTheThings - SQL Injection](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/SQL%20Injection)

---

## 3. Jawaban + Bukti

### Step-by-step
1. Buka halaman login di Juice Shop.  
   

2. Masukkan payload SQL Injection berikut di field **Email**:
   ```sql
   admin@juice-sh.op' OR '1'='1' --
dan isi password sembarang, misalnya 123.

3. Tekan tombol Log in. 

Hasilnya berhasil masuk ke akun admin
<img width="1917" height="1058" alt="step 1" src="https://github.com/user-attachments/assets/fa8753f3-a50f-433f-988e-551dcca1b315" />

4. setelah itu cari email jim di bagian review
<img width="1918" height="1078" alt="step 2" src="https://github.com/user-attachments/assets/7d836ee4-d5f6-4714-89fd-aa1892a5e36e" />

5. setelah itu masuk ke halaman login dan login sebagai email jim dengan payload 
```'--``` 
jadinya jim@juice-sh.op'--
<img width="1915" height="1032" alt="step3" src="https://github.com/user-attachments/assets/1fca7f07-7aba-4a3a-84bc-f85e676caa76" />

6. hasilnya berhasil masuk akun jim
   <img width="1918" height="1075" alt="done" src="https://github.com/user-attachments/assets/6d38e3e9-73f8-408c-9152-6d9748ae8e14" />
