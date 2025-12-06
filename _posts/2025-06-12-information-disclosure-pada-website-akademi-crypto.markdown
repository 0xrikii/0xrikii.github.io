---
layout: post
title:  "Information Disclosure Pada Website Akademi Crypto"
date:   2025-12-06 19:02:31 +0700
categories: bug-bounty
---

Baru-baru ini, saya menemukan celah keamanan yang cukup serius pada website Akademi Crypto milik Timothy Ronald. Website tersebut dibangun menggunakan CMS WordPress dan menggunakan salah satu plugin dengan versi yang memiliki kerentanan Information Disclosure. Celah keamanan ini memungkinkan penyerang (attacker) untuk mengakses dan melihat informasi news yang seharusnya hanya dapat diakses oleh member Akademi Crypto yang telah membayar keanggotaan. Temuan ini cukup mengkhawatirkan karena dapat merugikan baik pihak pengelola platform maupun para member yang membayar untuk mendapatkan akses eksklusif tersebut.

Vulnerability ini terjadi karena plugin WordPress yang digunakan memiliki konfigurasi keamanan yang lemah atau bug pada versi tertentu, yang memungkinkan siapa saja untuk melakukan bypass terhadap mekanisme autentikasi dan otorisasi yang seharusnya melindungi konten berbayar. Dengan memanfaatkan celah ini, seseorang yang tidak memiliki akses resmi dapat membaca informasi news yang merupakan salah satu nilai jual dari membership Akademi Crypto.

## **Apa itu information disclosure?**

Information disclosure adalah kondisi ketika informasi sensitif terbocorkan kepada pihak yang tidak berwenang, baik melalui kesalahan konfigurasi, bug aplikasi, celah keamanan, atau kelalaian manusia. Dampaknya berupa data yang seharusnya tidak bisa diakses menjadi dapat diakses oleh pihak yang tidak berwenang yang menyebabkan kebocoran informasi. Pada website Akademi Crypto saya menemukan bug information disclosure yang membuat saya dapat melihat news yang seharusnya hanya bisa diakses oleh member internal.

## Walkthrough!

Pertama-tama, saya melakukan inspeksi terhadap source code website Academy Crypto untuk memahami teknologi dan komponen yang digunakan. Dalam proses penelusuran ini, saya menemukan bahwa website tersebut menggunakan beberapa plugin WordPress, dan salah satu di antaranya menggunakan versi yang sudah lawas atau outdated. Plugin dengan versi lama ini menarik perhatian saya karena seringkali versi-versi usang memiliki kerentanan keamanan yang sudah terdokumentasi secara publik. Setelah mencatat nama dan versi plugin tersebut, saya kemudian melakukan pencarian lebih lanjut pada database kerentanan (vulnerability database) untuk mengonfirmasi apakah versi plugin ini memang memiliki celah keamanan yang dapat dieksploitasi.

![image.png](/assets/image/2025-06-12-information-disclosure-pada-website-akademi-crypto-1.png)

Setelah saya melakukan pencarian lebih mendalam mengenai plugin tersebut, saya menemukan bahwa versi yang digunakan memang terdaftar memiliki kerentanan Information Disclosure yang cukup kritis. Kerentanan ini memungkinkan penyerang untuk mengakses informasi sensitif tanpa melalui proses autentikasi yang seharusnya. Berdasarkan dokumentasi kerentanan yang saya temukan, celah ini dapat dieksploitasi dengan memanfaatkan parameter tertentu pada URL, yang akan membypass mekanisme kontrol akses yang telah diterapkan oleh sistem. Hal ini tentu saja sangat berbahaya karena membuka peluang bagi siapa saja untuk mengakses konten premium yang seharusnya hanya tersedia bagi member berbayar.

![image.png](/assets/image/2025-06-12-information-disclosure-pada-website-akademi-crypto-2.png)

Kemudian saya mencoba menambahkan parameter `s` pada URL [akademicrypto.com](http://akademicrypto.com) untuk menguji kerentanan yang telah saya temukan sebelumnya. Dengan menambahkan parameter ini pada URL, saya berharap dapat memicu respons sistem yang akan mengekspos data-data yang seharusnya tidak dapat diakses oleh pengguna yang tidak terautentikasi.

![image.png](/assets/image/2025-06-12-information-disclosure-pada-website-akademi-crypto-3.png)

Dan informasi yang seharusnya saya tidak bisa akses muncul di hasil pencarian. Ini membuktikan bahwa kerentanan tersebut benar-benar dapat dieksploitasi. Saya kemudian mencoba mengunjungi salah satu halaman news yang muncul di hasil pencarian tersebut, dan ternyata seluruh konten informasi lengkapnya dapat saya baca tanpa harus login atau memiliki keanggotaan berbayar. Ini merupakan bukti nyata bahwa plugin tersebut memiliki celah keamanan yang serius, karena konten premium yang seharusnya hanya dapat diakses oleh member yang telah membayar, kini dapat diakses secara gratis oleh siapa saja yang mengetahui cara mengeksploitasi kerentanan ini. 

![image.png](/assets/image/2025-06-12-information-disclosure-pada-website-akademi-crypto-4.png)

## Kesimpulan

Temuan ini sudah saya laporkan kepada pihak Akademi Crypto. Saya menghubungi tim teknis mereka dan memberikan detail lengkap mengenai kerentanan yang ditemukan, termasuk langkah-langkah reproduksi dan bukti-bukti screenshot yang menunjukkan bahwa celah keamanan ini benar-benar dapat dieksploitasi. Dalam laporan tersebut, saya juga menyertakan rekomendasi perbaikan, seperti melakukan update plugin ke versi terbaru yang sudah tidak memiliki kerentanan ini, dan menonaktifkan fitur search nya jika memang tidak diperlukan agar menjadi lebih aman. Saya berharap pihak Akademi Crypto dapat segera mengambil tindakan untuk menutup celah keamanan ini demi melindungi konten premium mereka dan menjaga kepercayaan para member yang telah membayar untuk mendapatkan akses eksklusif.

![image.png](/assets/image/2025-06-12-information-disclosure-pada-website-akademi-crypto-5.png)