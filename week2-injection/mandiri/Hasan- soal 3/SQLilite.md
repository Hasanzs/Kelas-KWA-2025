# SQL Injection - PicoCTF: SQLite

## 1. Soal
Eksploitasi SQL Injection pada challenge **SQLite** di platform PicoCTF untuk mendapatkan flag.

### Challenge Details:
- **Kategori**: Web Exploitation
- **Difficulty**: Medium  
- **Points**: Variable
- **Author**: MUBARAK MIKAIL

### Deskripsi:
Can you login to this website? Try to login here.

Challenge launches an instance on demand dengan URL: `http://saturn.picoctf.net:64992`

---

## 2. Link Resource
- [Challenge URL](http://saturn.picoctf.net:64992)
- [PicoCTF Platform](https://play.picoctf.org/)
- [SQLite SQL Injection](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/SQL%20Injection/SQLite%20Injection)
- [SQL Injection Authentication Bypass](https://owasp.org/www-community/attacks/SQL_Injection)

---

## 3. Jawaban + Bukti

### Step 1: Challenge Setup
- Mengakses challenge page di PicoCTF platform
- Launch instance untuk mendapatkan URL aktif
- Instance status: RUNNING
- Challenge berjalan di `http://saturn.picoctf.net:64992`
<img width="1863" height="1031" alt="step 1" src="https://github.com/user-attachments/assets/d8dcffaf-cc25-47b4-be4b-427e10ae6133" />



---

### Step 2: Initial Investigation
- Mengakses login page di URL yang diberikan
- Menemukan form login standard dengan field username dan password
- Testing normal login untuk melihat behavior aplikasi
<img width="1918" height="1078" alt="step 2" src="https://github.com/user-attachments/assets/0493ca7c-2cef-4013-af57-4dbd4a110924" />



---

### Step 3: SQL Injection Testing
- Mencoba payload SQL injection klasik
- Testing dengan payload: `' OR '1'='1' --`
- Dari screenshot terlihat query SQL yang dieksekusi:
- 
<img width="1918" height="1078" alt="step3" src="https://github.com/user-attachments/assets/3ec38f76-d08e-4eb6-aa21-6e8f4da17f38" />

```sql
SELECT * FROM users WHERE name='' OR '1'='1' -- AND password=''
```


---

### Step 4: Successful Authentication Bypass
- Login berhasil dengan payload SQL injection
- Aplikasi menampilkan pesan: "Logged in! But can you see the flag, it is in plainsight."
- Flag tersembunyi di dalam HTML source code


<img width="1918" height="1078" alt="berhasil" src="https://github.com/user-attachments/assets/f09012fb-aed1-42f4-a67f-97502043593a" />

---

### Step 5: Flag Discovery
- Menggunakan Developer Tools untuk inspect HTML source
- Flag ditemukan dalam HTML comment atau hidden element
- Dari screenshot terlihat flag: `picoCTF{L00k5_l1k3_y0u_solv3d_1t_d3c660ac}`

### Flag yang ditemukan:
```
picoCTF{L00k5_l1k3_y0u_solv3d_1t_d3c660ac}
```

---

## 4. Payload yang Berhasil
```sql
Username: ' OR '1'='1' --
Password: (any value)
```

Alternative payload:
```sql
Username: admin'--
Password: (any value)
```

---

## 5. Penjelasan Teknis

### Vulnerability Analysis:
Website menggunakan SQLite database dengan query yang rentan terhadap injection. Query backend kemungkinan:
```sql
SELECT * FROM users WHERE name='$username' AND password='$password'
```

### Exploitation:
Dengan payload `' OR '1'='1' --`, query menjadi:
```sql
SELECT * FROM users WHERE name='' OR '1'='1' -- AND password=''
```

Comment `--` membuat bagian password diabaikan dan kondisi `'1'='1'` selalu bernilai true, sehingga authentication berhasil di-bypass.

### Flag Location:
Flag tersembunyi dalam HTML source code setelah successful login, memerlukan inspection menggunakan browser Developer Tools.

---

## 6. Refleksi dan Pembelajaran

### Berhasil:
✅ Identifikasi vulnerability SQL injection pada SQLite database  
✅ Bypass authentication mechanism  
✅ Menemukan flag tersembunyi dalam HTML source  
✅ Flag: `picoCTF{L00k5_l1k3_y0u_solv3d_1t_d3c660ac}`

### Catatan Penting:
Challenge ini menggabungkan SQL injection dengan teknik information gathering. Setelah bypass authentication, diperlukan kemampuan untuk inspect HTML source dan menemukan flag yang tersembunyi.

### Pembelajaran:
SQL injection vulnerability sama berbahayanya di SQLite maupun database lainnya. Selalu lakukan source code inspection setelah mendapat akses. Developer sering menyembunyikan informasi penting dalam HTML comments atau hidden elements.

---

## 7. Mitigation
Untuk mencegah vulnerability serupa:

### Parameterized Queries:
```python
# SQLite Python example
cursor.execute("SELECT * FROM users WHERE name=? AND password=?", (username, password))
```

### Input Validation:
Implementasi whitelist validation, length limitation, dan proper sanitization untuk semua user input.

### Security Headers:
Jangan menyimpan informasi sensitif dalam HTML source yang bisa diakses client-side. Gunakan proper session management dan authorization checks.