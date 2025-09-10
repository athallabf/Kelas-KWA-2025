# Challenge: Login Bender (SQL Injection)

Category: Injection
Points: 3 Stars
Difficulty: Easy

## Challenge Description

Log in with the Bender's user account.

## Resource

[OWASP Juice Shop - Injection Challenges](https://juice-shop.herokuapp.com/#/score-board?categories=Injection)

## Step-by-Step Solution

1. Masuk ke login page
   ![](images/step1-login-page.png)
2. Lalu mengikuti format email yang sama kita masukkan email `bender@juice-sh.op` (bisa dicari lewat review products juga)
   ![](images/step2-fill-email.png)
3. Lalu tambahkan `'--` dibelakang email untuk comment SQL selanjutnya, makanya password bisa asal aja
   ![](images/step3-fill-login.png)
4. Klik login dan lihat akunnya
   ![](images/step4-see-detail-account.png)

## Reflection

- Status: ✅ Berhasil
- Catatan: Kueri SQL tidak memiliki validasi input => bypass autentikasi tercapai.
