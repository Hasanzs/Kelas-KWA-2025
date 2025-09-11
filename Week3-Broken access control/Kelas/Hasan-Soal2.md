# Broken Access Control - OWASP Juice Shop: View Another User's Shopping Basket

## 1. Soal
Eksploitasi Broken Access Control pada aplikasi **OWASP Juice Shop** untuk mengakses dan melihat shopping basket milik user lain.

### Challenge Details:
- **Kategori**: Broken Access Control
- **Application**: OWASP Juice Shop
- **Target**: View another user's shopping basket
- **Difficulty**: Easy

### Deskripsi:
Challenge ini mengharuskan kita untuk berhasil mengakses shopping basket milik user lain tanpa authorization yang proper, menunjukkan vulnerability Broken Access Control.

---

## 2. Link Resource
- [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/)
- [OWASP Top 10 - Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)
- [Insecure Direct Object References (IDOR)](https://owasp.org/www-community/attacks/Insecure_Direct_Object_References)
- [Juice Shop Solutions Guide](https://pwning.owasp-juice.shop/)

---

## 3. Jawaban + Bukti

### Step 1: Initial Investigation
- Login sebagai admin biasa pada aplikasi OWASP Juice Shop
- Navigasi ke shopping basket sendiri melalui menu atau icon basket
- Menganalisis URL dan parameter yang digunakan untuk mengakses basket

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/34d75656-0030-4a9a-bc17-5611b5dc9438" />


---

### Step 2: URL Analysis and Parameter Discovery
- Menganalisis network traffic menggunakan browser Developer Tools
- Melihat HTTP requests yang dilakukan saat mengakses basket
- Mengidentifikasi parameter ID yang digunakan untuk mengakses basket data
- Parameter bisa berupa user ID, basket ID, atau session identifier


<img width="1739" height="1033" alt="image" src="https://github.com/user-attachments/assets/cea07d11-e30a-401b-97a7-832738c0508a" />


---

### Step 3: Parameter Manipulation
- Melakukan manipulasi parameter untuk mengakses basket user lain
- Testing dengan mengubah ID parameter ke nilai lain (1, 2, 3, dst)
- Menggunakan Browser Developer Tools untuk modify requests
- Atau menggunakan tools seperti Burp Suite untuk intercept dan modify requests

**Contoh manipulation:**
- Original: `/rest/basket/2` (basket milik user saat ini)
- Modified: `/rest/basket/1` (basket milik user lain)

Saya disini merubah id bid valuenya ke 7


<img width="1915" height="1079" alt="image" src="https://github.com/user-attachments/assets/38c02113-a0eb-41f8-87c8-90e9229776f8" />

---

### Step 4: Successful Access to Another User's Basket
- Berhasil mengakses basket milik user lain
- Dapat melihat items, quantities, dan details dari shopping basket user tersebut
- Challenge "View Basket" berhasil diselesaikan
- Sistem menunjukkan data basket user lain tanpa proper authorization check

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/2848ebc5-a7e4-44f6-9c36-2fb64a7e2ac2" />

---

## 4. Exploitation Methods

### Method 1: Direct URL Manipulation
```
Original URL: https://juice-shop.com/rest/basket/2
Modified URL: https://juice-shop.com/rest/basket/1
```

### Method 2: API Parameter Tampering
```javascript
// Original API call
GET /rest/basket/2

// Modified API call
GET /rest/basket/1
```

### Method 3: Session/Cookie Manipulation
Menggunakan browser Developer Tools untuk modify session parameters atau cookies yang terkait dengan user identification.

---

## 5. Penjelasan Teknis

### Vulnerability Analysis:
Aplikasi menggunakan **Insecure Direct Object References (IDOR)** dimana user dapat mengakses resource milik user lain hanya dengan mengubah parameter identifier. Backend tidak melakukan proper authorization check.

### Broken Access Control Pattern:
```javascript
// Vulnerable code example
app.get('/rest/basket/:id', (req, res) => {
  const basketId = req.params.id;
  const basket = getBasketById(basketId); // No ownership check!
  res.json(basket);
});
```

### Expected Secure Implementation:
```javascript
// Secure code example
app.get('/rest/basket/:id', (req, res) => {
  const basketId = req.params.id;
  const userId = req.user.id; // Get current user
  const basket = getBasketById(basketId);
  
  if (basket.userId !== userId) {
    return res.status(403).json({error: 'Forbidden'});
  }
  
  res.json(basket);
});
```

---

## 6. Refleksi dan Pembelajaran

### Berhasil:
✅ Identifikasi IDOR vulnerability pada basket access  
✅ Parameter manipulation untuk access user data  
✅ Berhasil view basket milik user lain  
✅ Challenge "View Basket" completed

### Catatan Penting:
Vulnerability ini sangat common dalam aplikasi web modern. IDOR memungkinkan attacker mengakses data sensitif milik user lain hanya dengan manipulasi parameter sederhana. Impact bisa sangat serius tergantung jenis data yang ter-expose.

### Pembelajaran:
Broken Access Control adalah vulnerability #1 di OWASP Top 10 2021. Setiap endpoint yang mengakses user-specific data harus implement proper authorization checks. Never trust user input termasuk URL parameters.

---

## 7. Mitigation
Untuk mencegah vulnerability serupa:

### Authorization Checks:
```javascript
// Always verify user ownership
function verifyBasketOwnership(userId, basketId) {
  const basket = getBasketById(basketId);
  return basket && basket.userId === userId;
}
```

### Access Control Implementation:
Implementasi role-based atau attribute-based access control pada setiap endpoint yang sensitive.

### Input Validation:
Validate dan sanitize semua input parameters, jangan trust user input apapun.

### Security Testing:
Regular penetration testing dan code review untuk identify IDOR vulnerabilities sebelum production deployment.
