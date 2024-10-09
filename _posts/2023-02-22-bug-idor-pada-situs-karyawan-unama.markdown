---
layout: post
title:  "Bug IDOR Pada Situs Karyawan Universitas Dinamika Bangsa"
date:   2023-02-22 22:51:31 +0700
categories: bug-bounty
---

IDOR (Insecure Direct Object Reference) adalah salah satu kerentanan yang paling umum ditemukan dalam aplikasi, dan dapat menyebabkan kebocoran keamanan yang serius. Hari ini, saya akan menjelaskan bagaimana saya menemukan bug IDOR pada situs karyawan Universitas Dinamika Bangsa.

#### Apa itu IDOR?

Insecure Direct Object Reference (IDOR) adalah kerentanan keamanan yang terjadi ketika aplikasi memperlihatkan objek internalnya kepada pengguna tanpa perlindungan akses yang memadai.

Biasanya, aplikasi web atau API menggunakan referensi atau pengenal untuk mengakses dan menampilkan data dari sistem penyimpanan. Jika referensi ini tidak dilindungi dengan baik, penyerang dapat memanipulasinya untuk mendapatkan akses tidak sah ke data sensitif.

Kerentanan ini muncul ketika aplikasi tidak melakukan pengecekan otorisasi yang benar sebelum memberikan sumber daya yang diminta. Penyerang dapat memanfaatkan ini dengan mengubah parameter atau pengenal dalam permintaan, sehingga memungkinkan mereka mengakses data atau fitur yang seharusnya tidak mereka akses.

Contohnya, penyerang dapat mengubah parameter dalam URL untuk mengakses file pribadi pengguna lain atau memodifikasi data orang lain. Hal ini bisa mengakibatkan kebocoran data, manipulasi informasi, atau bahkan pengambilalihan akun jika penyerang mendapatkan akses ke fitur dengan hak istimewa.

#### Walkthrough!

Saat mengakses halaman karyawan.unama.ac.id, saya menemukan sebuah fitur pendaftaran (register) yang memungkinkan saya untuk membuat akun baru. Sebagai uji coba, saya membuat akun dengan username "test." Setelah akun berhasil dibuat, saya mulai menganalisis lebih dalam terkait struktur dan keamanan aplikasi tersebut.

![2023-02-22-bug-idor-pada-situs-karyawan-unama-1.png](/assets/image/2023-02-22-bug-idor-pada-situs-karyawan-unama-1.png)

Dalam proses eksplorasi, saya menemukan bahwa setiap akun yang terdaftar memiliki ID pengguna unik yang disematkan pada URL tertentu. Akun yang saya buat diberi ID pengguna "0125." Saya kemudian mencoba melakukan hash pada ID pengguna tersebut dengan menggantinya menjadi "0004." Setelah itu, ID baru ini saya enkripsi menggunakan algoritma hash MD5 dan saya letakkan dalam URL bagian "detail."

![2023-02-22-bug-idor-pada-situs-karyawan-unama-2.png](/assets/image/2023-02-22-bug-idor-pada-situs-karyawan-unama-2.png)

Hasil dari modifikasi ini cukup mengejutkan, karena saya berhasil mendapatkan akses ke data milik pengguna lain yang memiliki ID "0004." Data pribadi pengguna tersebut dapat saya lihat tanpa adanya hambatan keamanan yang memadai. Tidak hanya itu, saya juga menemukan bahwa saya dapat melakukan perubahan pada data-data pengguna tersebut, seperti mengedit informasi penting, tanpa perlu masuk ke akun pengguna yang bersangkutan atau melakukan proses otentikasi lebih lanjut.

![2023-02-22-bug-idor-pada-situs-karyawan-unama-3.png](/assets/image/2023-02-22-bug-idor-pada-situs-karyawan-unama-3.png)

Temuan ini mengindikasikan adanya kelemahan serius pada mekanisme kontrol akses dan otorisasi dalam aplikasi tersebut. Kelemahan ini memungkinkan pengguna yang tidak sah untuk tidak hanya melihat, tetapi juga memodifikasi informasi milik pengguna lain dengan cara yang relatif sederhana.
