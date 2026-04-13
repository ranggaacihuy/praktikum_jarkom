# LAPORAN PRAKTIKUM JARINGAN KOMPUTER - MODUL 3
## HTTP

### Identitas Mahasiswa
**Nama:** Rangga Dani Prasetya  
**NIM:** 103072400057  
**Kelas:** IF - 04 - 01  

---

## A. Tujuan Praktikum
1. Mampu menganalisis dan memahami cara kerja protokol HTTP dengan menggunakan Wireshark.

---

## B. Pengantar
Pada modul ini dibahas berbagai konsep dasar dari protokol HTTP, antara lain interaksi dasar GET/response, format pesan HTTP, proses pengambilan file HTML berukuran besar, pengambilan HTML yang memiliki objek tambahan (embedded objects), serta aspek autentikasi dan keamanan dalam HTTP.

---

## C. Hasil dan Pembahasan

### 1. Basic HTTP GET/response interaction
**Langkah-langkah:**
- Melakukan capture menggunakan Wireshark.
- Mengakses URL http://gaia.cs.umass.edu/wireshark-labs/HTTP-wireshark-file1.html.
- Menghentikan proses capture dan menerapkan filter HTTP.

Hasil yang diperoleh menunjukkan adanya request GET dari browser ke server (gaia.cs.umass.edu) dan response dari server berupa status **200 OK** yang menandakan permintaan berhasil diproses.

![](../week3/assets/image/week3(g1).png)

---

### 2. HTTP CONDITIONAL GET/response interaction
**Langkah-langkah:**
- Menghapus cache pada browser.
- Melakukan capture menggunakan Wireshark.
- Mengakses URL http://gaia.cs.umass.edu/wireshark-labs/HTTP-wireshark-file2.html.
- Mengakses kembali URL yang sama atau melakukan refresh dengan cepat.
- Menghentikan capture dan memfilter HTTP.

Pada percobaan ini, saat akses pertama server mengirimkan seluruh konten dengan status **200 OK**. Namun pada akses berikutnya, jika tidak ada perubahan pada data, server hanya mengirimkan status **304 Not Modified**. Hal ini menunjukkan bahwa browser memanfaatkan cache sehingga tidak perlu mengunduh ulang seluruh data.

![](../week3/assets/image/week3(g2).png)

---

### 3. Retrieving Long Documents
**Langkah-langkah:**
- Menghapus cache browser.
- Melakukan capture dengan Wireshark.
- Mengakses URL http://gaia.cs.umass.edu/wireshark-labs/HTTP-wireshark-file3.html.
- Menghentikan capture dan memfilter HTTP.

Pada kasus ini, ukuran file HTML cukup besar (sekitar 4500 byte), sehingga tidak dapat dikirim dalam satu paket TCP. Data tersebut dibagi menjadi beberapa segmen TCP. Di Wireshark terlihat sebagai **“TCP segment of a reassembled PDU”**, yang menunjukkan bahwa data dipecah saat dikirim dan digabung kembali di sisi penerima.

![](../week3/assets/image/week3(g3).png)

---

### 4. HTML Documents dengan Embedded Objects
**Langkah-langkah:**
- Menghapus cache browser.
- Melakukan capture dengan Wireshark.
- Mengakses URL http://gaia.cs.umass.edu/wireshark-labs/HTTP-wireshark-file4.html.
- Menghentikan capture dan memfilter HTTP.

Pada percobaan ini, halaman HTML utama berisi referensi ke dua gambar. Gambar tersebut tidak tersimpan langsung dalam file HTML, melainkan diambil melalui URL terpisah. Oleh karena itu, browser melakukan beberapa request tambahan untuk mengambil objek-objek tersebut dari server.

![](../week3/assets/image/week3(g4).png)

---

### 5. HTTP Authentication
**Langkah-langkah:**
- Menghapus cache browser.
- Melakukan capture menggunakan Wireshark.
- Mengakses URL http://gaia.cs.umass.edu/wireshark-labs/protected_pages/HTTP-wireshark-file5.html dan memasukkan username **wiresharkstudents** serta password **network**.
- Menghentikan capture dan memfilter HTTP.

Hasil pengamatan menunjukkan bahwa username dan password dapat terlihat pada bagian *Credentials* di Wireshark. Meskipun tampak seperti terenkripsi, sebenarnya data tersebut hanya dikodekan menggunakan **Base64**. Hal ini berarti informasi login masih dapat dengan mudah didekode dan tidak aman jika digunakan pada jaringan publik.

![](../week3/assets/image/week3(g5).png)

---

## D. Kesimpulan
Dari praktikum yang telah dilakukan, dapat disimpulkan bahwa:
1. HTTP bekerja dengan mekanisme *request-response*, di mana client mengirim permintaan (GET) dan server memberikan respons berupa status serta data yang diminta.
2. HTTP mendukung mekanisme caching yang memungkinkan penghematan bandwidth dan mempercepat proses loading, ditandai dengan respons **304 Not Modified** jika data tidak berubah.
3. File HTML berukuran besar akan dikirim dalam beberapa segmen TCP dan kemudian disusun kembali oleh penerima.
4. Sistem autentikasi pada HTTP tidak aman karena data login hanya dikodekan (Base64), bukan dienkripsi, sehingga rentan terhadap penyadapan.