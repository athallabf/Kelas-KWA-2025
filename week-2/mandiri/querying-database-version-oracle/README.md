# Challenge:

Lab: SQL injection attack, querying the database type and version on Oracle

## Challenge Description

This lab contains a SQL injection vulnerability in the product category filter. You can use a UNION attack to retrieve the results from an injected query.

To solve the lab, display the database version string.

## Resource

[Port Swigger - SQL Injection (querying database version oracle)](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-oracle)

## Step-by-Step Solution

1. **Akses Lab**
   Buka lab SQL injection untuk querying database version pada Oracle
   ![](images/step1-lab.png)

2. **Setup Interception**
   Aktifkan Burp Suite interceptor untuk menangkap HTTP request
   ![](images/step2-interceptor.png)

3. **Capture Request**
   Tangkap request category query dan kirim ke Burp Repeater untuk modifikasi
   ![](images/step3-categorytorepeater.png)

4. **Inject Payload**
   Masukkan payload SQL injection untuk mengekstrak database version:

   ```
   '+UNION+SELECT+BANNER,+NULL+FROM+v$version--
   ```

   **Penjelasan Payload:**

   - `'` - Menutup string parameter category
   - `UNION SELECT` - Menambahkan query untuk mengekstrak data
   - `BANNER` - Kolom yang berisi informasi version Oracle
   - `NULL` - Placeholder untuk kolom kedua (karena UNION memerlukan jumlah kolom yang sama)
   - `FROM v$version` - View Oracle yang berisi informasi version database
   - `--` - Comment untuk mengabaikan sisa query
   - `+` - URL encoding untuk spasi

   ![](images/step4-payload.png)

5. **Verifikasi Hasil**
   Berhasil mengekstrak informasi version Oracle database
   ![](images/step5-success.png)

## Reflection

- **Status:** ✅ Berhasil
- **Root Cause:** Parameter category filter tidak memiliki validasi input yang proper untuk UNION-based SQL injection
- **Attack Vector:** UNION-based SQL injection melalui parameter category untuk query database version
- **Key Insight:**
  - Berhasil menggunakan UNION SELECT untuk menambahkan query database version ke hasil query
  - Payload `'+UNION+SELECT+BANNER,+NULL+FROM+v$version--` berhasil mengekstrak informasi version Oracle database
  - Demonstrasi bagaimana SQL injection bisa digunakan untuk reconnaissance database
  - Menggunakan view `v$version` yang spesifik untuk Oracle database
  - URL encoding `+` untuk spasi memungkinkan bypass input validation
  - Teknik ini memungkinkan attacker untuk mengetahui versi database yang digunakan
