---

# **Write-up Challenge: Flag Command (HackTheBox)**

## **1. Pendahuluan**

Pada challenge **Flag Command**, kita diberikan sebuah web-terminal interaktif yang menampilkan cerita petualangan ala text-based adventure. Meski kelihatannya cuma game iseng, challenge tipe web seperti ini biasanya menyembunyikan logic tertentu di sisi frontend, terutama pada file JavaScript. Tujuan kita sederhana: menemukan flag **HTB{...}**.



<img width="1919" height="1025" alt="image" src="https://github.com/user-attachments/assets/d5046338-df08-40d2-9f31-f1cbeb7c938c" />



---

## **2. Observasi Awal**

Saat pertama membuka halaman, terminal langsung menampilkan teks cerita dan beberapa opsi seperti:

* HEAD NORTH
* HEAD SOUTH
* HEAD EAST
* HEAD WEST




Kalau kita ketik sesuatu di luar opsi, responnya jelas-jelas scripted. Ini tanda bahwa mekanisme game sudah diatur sangat kaku melalui JavaScript di sisi klien. Karena itu langkah pertama yang masuk akal adalah buka **Developer Tools (F12)**.

---

## **3. Enumerasi Melalui DevTools**

Di tab **Elements**, terlihat bahwa halaman memuat tiga file JavaScript utama:

* `/static/terminal/js/commands.js`
* `/static/terminal/js/game.js`
* `/static/terminal/js/main.js`

Dari ketiganya, file **main.js** terlihat paling “berisi” dan mengatur alur game. Dua file lainnya lebih bersifat pendukung (pesan, audio, dsb).
<img width="1912" height="1027" alt="image" src="https://github.com/user-attachments/assets/5177fe15-2ad8-41e6-8a72-9c8c4143369e" />

---

## **4. Analisis File JavaScript**

### **4.1 commands.js**

Isinya cuma pesan-pesan dan command dasar, tidak ada logic penting.

### **4.2 game.js**

File ini hanya meng-handle fungsi tampilan teks. Tidak ada sesuatu yang bisa dimanfaatkan untuk flag.

### **4.3 main.js (file utama yang penting)**

Nah, di sini baru ketemu tulang punggung alur cerita game. Ada potongan kode yang menentukan kapan “step” dari game berubah. Contohnya:

```
if (currentCommand == 'HEAD NORTH') {
    currentStep = '2';
} else if (currentCommand == 'FOLLOW A MYSTERIOUS PATH') {
    currentStep = '3';
} else if (currentCommand == 'SET UP CAMP') {
    currentStep = '4';
}
```
<img width="1915" height="1022" alt="image" src="https://github.com/user-attachments/assets/00703c7d-bbcd-4ead-9660-349d9516aaaa" />

Dari sini keliatan bahwa hanya tiga opsi tersebut yang benar-benar membuat progres pada game. Opsi lain cuma ngasih pesan iseng.

Masalahnya, setelah dicoba semuanya, ketiga path itu malah buntu. Jadi flag pasti bukan dari alur cerita biasa.
<img width="1916" height="992" alt="image" src="https://github.com/user-attachments/assets/7a7262fa-7dfc-4188-9d1f-efeea3bcf3de" />

---

## **5. Mencurigai Adanya “Secret Commands”**

Masih di file yang sama, ada bagian kode lain yang jauh lebih menarik:

```
if (acceptedCommands.includes(currentCommand) || secret.includes(currentCommand)) {
    ...
}
```

Artinya game dapat menerima **dua tipe command**:

1. command biasa (HEAD NORTH, dsb)
2. *command rahasia* yang disimpan dalam array **secret**

Untuk menemukan array **secret** ini, kita harus cari sumber datanya.
<img width="1786" height="1019" alt="image" src="https://github.com/user-attachments/assets/d5d0d42b-5a34-4388-b986-9aa147214d2f" />

---

## **6. Melihat Request pada Network Tab**

Masuk ke tab **Network** lalu refresh halaman.

Di sini muncul sebuah request bernama **optionsfile**, yang formatnya JSON. Setelah file itu dibuka, terlihat struktur seperti:

```
{
   ...
   "secret": [
       "...",
       "COMMAND_RAHASIA_YANG_KITA BUTUHKAN"
   ]
}
```

Dan di dalam array **secret** inilah ada satu string yang jelas bukan bagian dari pilihan game, jadi sudah pasti inilah trigger untuk flag.
<img width="1912" height="1020" alt="image" src="https://github.com/user-attachments/assets/3041a149-8058-4132-a276-5d40b13c41dd" />
<img width="1917" height="1033" alt="image" src="https://github.com/user-attachments/assets/8cef378b-8fb0-43bb-9a3e-5afa5176c6ee" />

---

## **7. Memasukkan Secret Command**

Masukkan string rahasia tersebut ke terminal (isinya tergantung challenge instansimu). Begitu dimasukkan, terminal langsung memberikan output berisi **flag HTB{...}**.

Artinya jalur resmi game hanyalah pengalih perhatian, sedangkan solusi sesungguhnya ada pada enumerasi kode.
<img width="1913" height="1025" alt="image" src="https://github.com/user-attachments/assets/71cea681-49a8-4f76-8f88-e9a1e67dcf5b" />
<img width="1915" height="982" alt="image" src="https://github.com/user-attachments/assets/111e2a1e-ab7b-4d4e-8bf0-aac1dc3a89b5" />


---

## **8. Kesimpulan**

Challenge ini sebenarnya sederhana: pahami alur front-end, temukan array `secret`, eksekusi perintah tersembunyi, dan flag muncul. Tidak perlu menyelesaikan petualangan palsu yang disediakan server. Intinya:

1. Buka DevTools
2. Telusuri file JavaScript
3. Temukan request `/api/options`
4. Baca JSON berisi array “secret”
5. Jalankan secret command di terminal
6. Ambil flag

Challenge ini menguji pemahaman dasar enumerasi web dan pentingnya membaca sisi klien, terutama ketika tidak ada backend kompleks.

---

Kalau kamu butuh versi PDF, atau mau ditambahin narasi pembuka/penutup biar lebih “akademis”, tinggal bilang.
