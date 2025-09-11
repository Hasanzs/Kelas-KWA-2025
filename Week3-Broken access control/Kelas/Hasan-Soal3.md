# Injection - OWASP Juice Shop: Forged Feedback
## 1. Soal
Eksploitasi vulnerability pada OWASP Juice Shop untuk menyelesaikan challenge **Forged Feedback** dengan memanipulasi feedback form untuk mengirim feedback atas nama user lain.

---

## 2. Link Resource
- [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/)
- [OWASP Juice Shop Challenges](https://pwning.owasp-juice.shop/)
- [Burp Suite](https://portswigger.net/burp)
- [OWASP Top 10 - Injection](https://owasp.org/www-project-top-ten/2017/A1_2017-Injection)

---

## 3. Jawaban + Bukti

### Step 1: Initial Reconnaissance
- Akses halaman feedback pada OWASP Juice Shop
- Analisis form feedback untuk memahami struktur dan parameter yang digunakan
- Identifikasi field yang tersedia dan mekanisme pengiriman feedback

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/5a5fefa1-1c5d-4327-bc16-22cce667e48f" />

---

### Step 2: Intercept Feedback Request
- Menggunakan Burp Suite untuk intercept request saat mengirim feedback normal
- Analisis struktur HTTP request dan parameter yang dikirim
- Identifikasi parameter yang dapat dimanipulasi (seperti user ID atau author information)

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/c48360b8-37ac-49bf-b3fc-6797aa93d6bb" />

---

### Step 3: Identify Target User
- Eksplorasi untuk menemukan user lain yang dapat dijadikan target
- Identifikasi user ID atau parameter identifikasi user lainnya
- Mencari informasi tentang user yang akan dijadikan target forgery

<img width="1916" height="1075" alt="image" src="https://github.com/user-attachments/assets/994511f0-5d18-4ef6-9ae7-7ae6653ab5a6" />

---

### Step 4: Forge Feedback Request
- Modifikasi request untuk mengirim feedback atas nama user lain
- Manipulasi parameter user ID atau authentication token
- Craft payload untuk bypass validation dan mengirim feedback palsu

<img width="1919" height="1070" alt="image" src="https://github.com/user-attachments/assets/a8d2bf4f-0dc2-4808-8278-1f6b293153f4" />

---

### Step 5: Challenge Completion
- Berhasil mengirim feedback atas nama user lain
- Konfirmasi bahwa feedback telah tersimpan dengan author yang salah
- Challenge "Forged Feedback" berhasil diselesaikan

<img width="1440" height="1076" alt="image" src="https://github.com/user-attachments/assets/cdc9ac58-7262-41e2-bdcb-39db5aa844f8" />

---

## 4. Refleksi
- **Berhasil**: Eksploitasi kelemahan authorization pada feedback system untuk mengirim feedback palsu atas nama user lain
- **Teknik**: Manipulation parameter user ID atau session token untuk bypass authentication
- **Catatan**: Challenge ini menunjukkan pentingnya proper authorization check pada setiap action yang dilakukan user
- **Pembelajaran**: 
  - Pentingnya validasi authorization pada server-side
  - Risiko parameter manipulation dalam web application
  - Implementasi proper session management dan user verification
