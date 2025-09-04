# Injection - Soal 3 (Login sebagai Bender)

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
   

2. Disini saya asumsikan semua email formatnya sama jadi saya lgsg tembak email bender bender@juice-sh.op
   
<img width="1917" height="1053" alt="step 1" src="https://github.com/user-attachments/assets/7ec93a68-df7b-464d-af53-9db524bd283d" />



4. setelah itu kita lgsg beri payload ``` '-- ``` jadinya bender@juice-sh.op'--
   
<img width="1918" height="1076" alt="step 2" src="https://github.com/user-attachments/assets/c0c3ed77-6cda-4a89-bdbd-006cbb20cf62" />


6. Hasilnya berhasil masuk ke akun admin

   
<img width="1918" height="1078" alt="done" src="https://github.com/user-attachments/assets/8d21c1f9-6b99-475c-8486-b2368918f0c4" />
