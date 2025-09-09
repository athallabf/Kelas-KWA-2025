# Challenge: Login Jim (SQL Injection)

## Resource

[OWASP Juice Shop - Injection Challenges](https://juice-shop.herokuapp.com/#/score-board?categories=Injection)

## Step-by-Step Solution

1. Kita coba cari email jim di product reviews
   ![](images/step1-all-products-page.png)
2. Disini kita menemukan email dari Jim
   [](images/step2-email-jim.png)
3. Kita ke login page dan input email jim dan coba login tapi disini kita belum tau password
   ![](images/step3-login-jim.png)
4. Tambahkan `'--` pada kolom email setelah `jim@juice-sh.op` untuk memanfaatkan kerentanan SQL Injection.
   ![](images/step4-login-with-sqli.png)
5. Kita berhasil login menggunakan akun jim
   ![](images/step5-success-login.png)

## Reflection

- Status: ✅ Berhasil
- Catatan: Kueri SQL tidak memiliki validasi input => bypass autentikasi tercapai.
