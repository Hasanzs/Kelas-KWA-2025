# Broken Access Control - OWASP Juice Shop: Admin Login

## 1. Soal
Eksploitasi Broken Access Control pada aplikasi **OWASP Juice Shop** untuk mendapatkan akses admin dan login sebagai administrator.

### Challenge Details:
- **Kategori**: Broken Access Control
- **Application**: OWASP Juice Shop
- **Target**: Login sebagai admin user
- **Difficulty**: Beginner/Easy

### Deskripsi:
Challenge ini mengharuskan kita untuk berhasil login sebagai administrator pada aplikasi OWASP Juice Shop tanpa mengetahui kredensial yang valid.

---

## 2. Link Resource
- [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/)
- [OWASP Top 10 - Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)
- [SQL Injection Authentication Bypass](https://owasp.org/www-community/attacks/SQL_Injection)
- [Juice Shop Solutions Guide](https://pwning.owasp-juice.shop/)

---

## 3. Jawaban + Bukti

### Step 1: Initial Investigation
- Mengakses aplikasi OWASP Juice Shop
- Navigasi ke halaman login melalui menu "Account" → "Login"
- Menganalisis form login untuk mencari vulnerability
- Form memiliki field Email dan Password

<img width="1590" height="1079" alt="image" src="https://github.com/user-attachments/assets/7dbc820c-bc9d-4b8b-80d5-7c7b012860fe" />

---

### Step 2: Admin Email Discovery
- Mencari informasi tentang admin email
- Biasanya admin menggunakan email seperti `admin@juice-sh.op`
- Atau bisa ditemukan melalui error messages, reviews, atau source code
- Testing dengan email admin yang umum digunakan


<img width="1857" height="1070" alt="image" src="https://github.com/user-attachments/assets/2eeec1b2-1f0c-4026-bbc6-8d6a2a60af05" />


---

### Step 3: SQL Injection Authentication Bypass
- Menggunakan teknik SQL injection untuk bypass authentication
- Payload yang digunakan pada field email:
```sql
admin@juice-sh.op'--
```
- Atau menggunakan payload universal:
```sql
' OR 1=1--
```
- Password bisa diisi dengan value apapun


<img width="1850" height="1079" alt="image" src="https://github.com/user-attachments/assets/4e767a0e-886f-4ebe-a171-26eb709a8334" />

---

### Step 4: Successful Admin Login
- SQL injection berhasil membypass authentication
- Login sebagai admin berhasil tanpa mengetahui password yang benar
- Dashboard admin atau fitur admin menjadi accessible
- Challenge "Login Admin" berhasil diselesaikan

step 4
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/9e19a180-8cfb-454a-856a-004942793c99" />

---

## 4. Payload yang Berhasil

### Method 1: Comment-based bypass
```sql
Email: ' OR true--
Password: kosong
```

### Method 2: Boolean-based bypass
```sql
Email: admin@juice-sh.op' OR '1'='1'--
Password: (any value)
```

### Method 3: Universal bypass
```sql
Email: ' OR 1=1--
Password: (any value)
```

---

## 5. Penjelasan Teknis

### Vulnerability Analysis:
Aplikasi menggunakan query SQL yang rentan terhadap injection pada proses authentication. Backend query kemungkinan:
```sql
SELECT * FROM Users WHERE email='$email' AND password='$password'
```

### Exploitation:
Dengan payload `admin@juice-sh.op'--`, query menjadi:
```sql
SELECT * FROM Users WHERE email='admin@juice-sh.op'-- AND password='...'
```

Comment `--` membuat kondisi password diabaikan, sehingga query hanya memeriksa email admin dan langsung memberikan akses.

### Broken Access Control:
Challenge ini menunjukkan vulnerability Broken Access Control karena:
- Tidak ada proper input validation
- Authentication mechanism dapat di-bypass
- Akses admin dapat diperoleh tanpa kredensial yang benar

---

## 6. Refleksi dan Pembelajaran

### Berhasil:
✅ Identifikasi admin email address  
✅ Bypass authentication menggunakan SQL injection  
✅ Mendapatkan akses admin ke aplikasi  
✅ Challenge "Login Admin" completed

### Catatan Penting:
Challenge ini adalah contoh klasik Broken Access Control yang dikombinasikan dengan SQL Injection. Admin access yang tidak terlindungi dengan baik dapat memberikan akses penuh ke aplikasi dan data sensitif.

### Pembelajaran:
Authentication bypass melalui SQL injection adalah salah satu attack vector paling berbahaya. Akses admin yang tidak terlindungi dapat mengakibatkan complete system compromise. Selalu implementasikan proper input validation dan parameterized queries.

---

## 7. Mitigation
Untuk mencegah vulnerability serupa:

### Parameterized Queries:
```javascript
// Node.js example
const query = 'SELECT * FROM Users WHERE email = ? AND password = ?';
db.query(query, [email, hashedPassword], callback);
```

### Input Validation:
Implementasi strict input validation untuk email format dan length limitation pada semua input fields.

### Multi-Factor Authentication:
Implementasi MFA khususnya untuk admin accounts sebagai additional security layer.

### Access Control:
Proper role-based access control (RBAC) dan regular audit untuk admin privileges.
