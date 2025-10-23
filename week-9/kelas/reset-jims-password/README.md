# Challenge: Reset Jim's Password

Category: Broken Authentication  
Difficulty: Easy

## Challenge Description

Reset Jim's password via the Forgot Password mechanism using the original answer to his security question.

## Step-by-Step Solution

1. Buka halaman Forgot Password dan masukkan email Jim: `jim@juice-sh.op`.
   ![](images/step1-forgot.png)
   Pertanyaan keamanan: middle name of Jim's oldest sibling.

2. Cari konteks di halaman review.
   Di sana Jim menyebut tentang “Bones’ new tricorder”.
   ![](images/step2-konteks.png)

3. Setelah dicari, “Bones’ new tricorder” terkait dengan universe Star Trek.
   ![](images/step3-search.png)

4. Karena Jim terkait Star Trek, kita cari saudara Jim di Star Trek.
   ![](images/step4-samuel.png)

5. Middle name yang didapat adalah “Samuel”. Masukkan jawaban tersebut.
   ![](images/step5-success.png)

## Reflection

- **Status:** ✅ Berhasil
- **Root Cause:** Pertanyaan keamanan menggunakan trivia publik yang mudah ditebak/ditemukan.
- **Attack Vector:** OSINT ringan melalui review/teks di aplikasi lalu validasi ke pencarian umum.
- **Key Insight:**
  - Security question berbasis trivia rentan karena jawabannya sering publik.
  - Gunakan metode recovery yang lebih kuat (email OTP, TOTP) dan hindari pertanyaan umum.
