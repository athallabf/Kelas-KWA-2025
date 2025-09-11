# Challenge: Admin Section

Category: Broken Access Control
Points: 2 Stars
Difficulty: Easy

## Challenge Description

Access the administration section of the store.

## Resource

[OWASP Juice Shop - Broken Access Control Challenges](https://juice-shop.herokuapp.com/#/score-board?categories=Broken%20Access%20Control)

## Step-by-Step Solution

1. **Login sebagai Administrator**
   Login menggunakan akun administrator (sudah dilakukan di challenge Login Admin sebelumnya)
   ![](images/step1-loginadmin.png)

2. **Identifikasi Tech Stack**
   Cek teknologi yang digunakan oleh website menggunakan extension Wappalyzer atau cari hint di website

   **Penjelasan:**

   - Identifikasi tech stack membantu memahami struktur aplikasi
   - Angular menggunakan routing berbasis URL untuk setiap komponen
   - Admin section kemungkinan memiliki route khusus

   ![](images/step2-techstack.png)

3. **Test Admin Routes**
   Karena menggunakan Angular, setiap komponen di-route menggunakan URL. Coba akses route admin yang umum:

   - `/admin` - Route yang umum digunakan
   - `/administration` - Route alternatif

   ![](images/step3-adminsalah.png)

   Tidak terjadi apa-apa ketika mengakses `/admin`, coba `/administration`

4. **Verifikasi Success**
   Berhasil mengakses halaman administration
   ![](images/step4-success.png)

## Reflection

- **Status:** ✅ Berhasil
- **Root Cause:** Admin section tidak memiliki proper access control dan route enumeration
- **Attack Vector:** Direct URL access dan route enumeration untuk mengakses admin section
- **Key Insight:**
  - Berhasil menggunakan route enumeration untuk menemukan admin section
  - Angular routing memungkinkan direct access ke komponen melalui URL
  - Demonstrasi bagaimana broken access control bisa digunakan untuk unauthorized admin access
  - Teknik ini memungkinkan attacker untuk mengakses admin section tanpa proper authorization
  - Route enumeration memungkinkan discovery of hidden functionality
  - Vulnerability ini berbahaya karena memungkinkan akses ke administrative functions
