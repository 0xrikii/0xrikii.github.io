---
layout: post
title:  "Kebocoran Data Melalui Celah Keamanan IDOR Pada Platform Edukasi Konten"
date:   2025-12-06 19:02:31 +0700
categories: bug-bounty
---

Hai, saya baru-baru ini menemukan celah keamanan pada website sebuah platform edukasi pembuatan konten. Saya tidak dapat menyebutkan nama brand-nya di sini. Celah yang saya temukan adalah IDOR kerentanan yang memungkinkan saya melihat invoice pengguna yang telah melakukan pembayaran pada platform tersebut.

## Walkthrough

Pertama-tama, saya memulai proses dengan melakukan pendaftaran akun baru pada platform tersebut . 

![image.png](/assets/image/2025-11-12-kebocoran-data-melalui-celah-keamanan-IDOR-pada-platform-edukasi-konten-1.png)

Setelah proses pendaftaran berhasil, saya melanjutkan ke tahap pembayaran, di mana sistem mengarahkan saya ke halaman pembayaran Midtrans seperti yang ditampilkan pada gambar di bawah ini. Pada tahap ini, semuanya masih berjalan normal tanpa indikasi adanya kejanggalan.

![image.png](/assets/image/2025-11-12-kebocoran-data-melalui-celah-keamanan-IDOR-pada-platform-edukasi-konten-2.png)

Setelah berada di halaman pembayaran, saya kembali membuka halaman utama dashboard platform tersebut pada tab browser yang berbeda. Tujuannya adalah untuk melihat apakah terdapat perubahan pada data invoice maupun riwayat transaksi. Di dalam dashboard, saya mengakses bagian **Payments** dan kemudian memilih tab **Invoice** untuk melihat daftar invoice yang terkait dengan akun saya.

![image.png](/assets/image/2025-11-12-kebocoran-data-melalui-celah-keamanan-IDOR-pada-platform-edukasi-konten-3.png)

Selanjutnya, saya mencoba membuka salah satu invoice dengan mengklik tombol **PDF**. 

![image.png](/assets/image/2025-11-12-kebocoran-data-melalui-celah-keamanan-IDOR-pada-platform-edukasi-konten-4.png)

Tindakan ini secara normal menampilkan invoice resmi yang memang milik akun saya sendiri, lengkap dengan rincian transaksi dan informasi pembayaran yang sesuai.

![image.png](/assets/image/2025-11-12-kebocoran-data-melalui-celah-keamanan-IDOR-pada-platform-edukasi-konten-5.png)

Namun, saat saya meninjau URL yang digunakan untuk memproses permintaan file PDF tersebut, saya menemukan adanya parameter penting, yaitu `txn`, yang tampaknya merujuk pada ID transaksi. URL awalnya terlihat seperti berikut:

```jsx
/wp-admin/admin-ajax.php?action=mepr_download_invoice&**txn=35050**&mepr_invoices_nonce=
```

Karena parameter `txn` ini dapat dimodifikasi dari sisi klien, saya mencoba mengganti angkanya menjadi nilai lain, misalnya:

```jsx
/wp-admin/admin-ajax.php?action=mepr_download_invoice&**txn=35049**&mepr_invoices_nonce=
```

Setelah melakukan perubahan nilai tersebut, file PDF yang muncul bukan lagi milik akun saya, melainkan milik **pengguna lain** yang sama sekali tidak terkait dengan saya. Invoice tersebut menampilkan informasi lengkap, mulai dari nama, alamat email, hingga detail status pembayaran beserta jumlah nominal yang telah dibayarkan. Hal ini menunjukkan bahwa endpoint tersebut tidak melakukan verifikasi otorisasi, sehingga siapa pun dapat membuka invoice siapa pun hanya dengan mengganti angka `txn`.
Temuan ini memperlihatkan secara jelas bahwa sistem memiliki kerentanan **Insecure Direct Object Reference (IDOR)** yang sangat serius, karena memungkinkan akses langsung terhadap data sensitif pengguna lain hanya dengan memodifikasi parameter di URL.

![image.png](/assets/image/2025-11-12-kebocoran-data-melalui-celah-keamanan-IDOR-pada-platform-edukasi-konten-6.png)

## Kesimpulan

Dari temuan yang saya dapatkan, data-data tersebut memiliki potensi besar untuk disalahgunakan oleh pihak yang tidak bertanggung jawab. Informasi sensitif seperti email customer hingga kode voucher yang muncul pada beberapa invoice dapat menjadi celah keamanan serius yang berpotensi dimanfaatkan untuk berbagai tindakan berbahaya. Misalnya, penyerang dapat melakukan phishing yang menyasar customer, penyalahgunaan voucher, hingga upaya impersonasi yang dapat merugikan pengguna maupun pihak pengelola platform. Jika tidak segera ditangani, kerentanan ini berisiko menimbulkan insiden kebocoran data dengan dampak luas, termasuk kerugian finansial, pelanggaran privasi, serta merusak reputasi layanan secara keseluruhan.

Namun demikian, setelah saya melaporkan temuan ini kepada pihak owner atau pengelola platform, saya menerima respon yang sangat baik dan profesional. Mereka menunjukkan komitmen yang kuat terhadap keamanan dengan langsung menindaklanjuti laporan tersebut. Celah yang saya temukan segera diperbaiki melalui penerapan validasi akses yang lebih ketat serta penyesuaian pada sistem pengelolaan invoice. Saat ini, kerentanan tersebut telah dinyatakan tertutup dan tidak lagi dapat dieksploitasi, sehingga akses tidak sah terhadap data sensitif pengguna tidak dapat dilakukan kembali.