# Challenge: Resource Hub – Path Traversal

Category: Secure Coding
Points: 4 Stars
Difficulty: Medium

## Challenge Description

Patch sebuah celah keamanan pada fitur upload file di aplikasi Resource Hub yang memungkinkan attacker melakukan **Arbitrary File Write** (menulis file di sembarang tempat) menggunakan teknik **Path Traversal** pada nama file.

## Resource

`routes/routes.js` dan script eksploit `solver.py`.

## Step-by-Step Solution

1.  **Analisis Eksploit:**
    Membuka file `solver.py` untuk melihat bagaimana serangan dilakukan. Terlihat bahwa attacker memanipulasi nama file dengan karakter `../` (dot-dot-slash) untuk keluar dari direktori yang seharusnya:

    ```python
    # Attacker mencoba menaruh file di folder static agar bisa diakses publik
    response = upload_resource(f"../static/js/{filename}", content)
    ```

2.  **Analisis Vulnerability:**
    Memeriksa file `routes/routes.js`. Masalah utama terletak pada bagaimana aplikasi menangani nama file yang diunggah. Aplikasi mempercayai `originalFilename` dari user secara mentah-mentah:

    ```javascript
    // VULNERABLE CODE
    const targetFilename = file.originalFilename; // Input: "../static/js/hacked.txt"

    // path.join akan memproses ".." sehingga jalur keluar dari 'resources'
    const targetPath = path.join(__dirname, '../resources', targetFilename);
    ```

    Hal ini menyebabkan file tersimpan di luar folder `resources`, yang bisa menimpa file sistem atau source code aplikasi.

3.  **Perbaikan (Patching):**
    Untuk mencegah Path Traversal, nama file harus disanitasi. Cara paling efektif di Node.js adalah menggunakan fungsi bawaan `path.basename()`. Fungsi ini akan membuang seluruh komponen direktori (seperti `/`, `\`, dan `..`) dan hanya mengambil nama filenya saja.

    Selain itu, ditemukan bug pada pengambilan objek file dari library `formidable`, sehingga perlu ditambahkan fallback logic agar server tidak crash (Error 400).

4.  **Implementasi Kode:**
    Update `routes/routes.js` dengan kode berikut:

    ```javascript
    // ... (kode pengambilan file yang lebih robust)

    try {
      // --- SECURITY PATCH ---
      // Gunakan path.basename() untuk mencegah Path Traversal
      // Input "../static/js/hacked.txt" akan diubah menjadi "hacked.txt"
      const targetFilename = path.basename(file.originalFilename);
      // --- END PATCH ---

      const targetPath = path.join(__dirname, '../resources', targetFilename);

      fs.renameSync(file.filepath, targetPath);

      // ...
    }
    ```

5.  **Verifikasi:**
    Setelah patch diterapkan dan server di-restart:

    - Attacker mengirim: `../static/js/hacked.txt`
    - Server memproses menjadi: `hacked.txt`
    - File disimpan aman di: `.../resources/hacked.txt`

    Script `solver.py` gagal menemukan file di folder `/static/js/`, yang menandakan celah keamanan telah tertutup rapat.
    ![](images/step3-flag.png)
    ![](images/step3-solved.png)

## Reflection

- **Status:** ✅ Berhasil
- **Root Cause:** Input Validation Failure. Aplikasi menggunakan input filename dari user secara langsung dalam operasi filesystem tanpa sanitasi.
- **Attack Vector:** Mengirimkan file dengan nama yang mengandung karakter traversal (`../`) untuk menulis file di luar direktori upload yang ditentukan.
- **Key Insight:** - Jangan pernah mempercayai `originalFilename` dari client. - Gunakan `path.basename()` untuk mengambil nama file murni. - Untuk keamanan maksimal, pertimbangkan untuk mengabaikan nama file dari user sepenuhnya dan menggantinya dengan _random generated string_ (seperti UUID) untuk menghindari konflik nama dan karakter berbahaya.
