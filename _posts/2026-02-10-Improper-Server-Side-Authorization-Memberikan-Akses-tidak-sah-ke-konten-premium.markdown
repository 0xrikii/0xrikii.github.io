---
layout: post
title:  "Improper Server-Side Authorization Pada Website Malaka"
date:   2025-12-06 19:02:31 +0700
categories: bug-bounty
---
Hai, saya baru-baru ini menemukan celah keamanan pada website malaka milik bang ferry irwandi. Celah yang saya temukan membuat saya dapat mengakses buku premium meskipun saya adalah user biasa yang tidak membayar.

## Improper Server Side Authorization

Improper server-side authorization adalah kondisi ketika server tidak melakukan atau tidak menerapkan pemeriksaan hak akses secara benar sebelum mengizinkan pengguna mengakses suatu resource atau menerima data tertentu. Dalam hal ini saya dapat mengakses buku premium meskipun saya adalah user biasa yang tidak membayar.

## Step to reproduce

Pertama saya mendaftarkan akun bisa dilihat saya adalah free user yang tidak berlangganan dan membayar apapun.

![Picture1.png](/assets/image/2026-02-10-Improper-Server-Side-Authorization-Memberikan-Akses-tidak-sah-ke-konten-premium-1.png)

Sebagai contoh saya gunakan buku ini. Buku ini adalah buku premium dan jelas saya tidak memiliki hak dan akses untuk membaca buku ini.

![Screenshot 2026-02-10 161648.png](/assets/image/2026-02-10-Improper-Server-Side-Authorization-Memberikan-Akses-tidak-sah-ke-konten-premium-2.png)

Disini letak saya mulai mengakali sistem nya, tepatnya pada endpoint /book/ff728dc5-c784-4d4d-a0eb-9b6239d84941?chapter=e76bfde4-1dbc-43ae-a35b-0c3fde3b11d2

saya ambil id buku dan chapter kemudian saya akses langsung, tapi saat akses langsung saya temper menggunakan burpsuite.

![](/assets/image/2026-02-10-Improper-Server-Side-Authorization-Memberikan-Akses-tidak-sah-ke-konten-premium-3.png)

Di endpoint /api/frontsite/books/ff728dc5-c784-4d4d-a0eb-9b6239d84941/access, saya memodifikasi response yang diterima.

![Screenshot 2026-02-10 161825.png](/assets/image/2026-02-10-Improper-Server-Side-Authorization-Memberikan-Akses-tidak-sah-ke-konten-premium-4.png)

Saya ubah nilai hasAccess yang sebelumnya false menjadi true dan reasons menjadi free_book.

![Screenshot 2026-02-10 162006.png](/assets/image/2026-02-10-Improper-Server-Side-Authorization-Memberikan-Akses-tidak-sah-ke-konten-premium-5.png)

Dan hasilnya saya dapat mengakses buku tersebut meskipun tidak membayar.

![Screenshot 2026-02-10 162130.png](/assets/image/2026-02-10-Improper-Server-Side-Authorization-Memberikan-Akses-tidak-sah-ke-konten-premium-6.png)

bahkan bukunya terdaftar di library saya dan bisa dilihat juga status saya disitu masih free user.

![Screenshot 2026-02-10 162234.png](/assets/image/2026-02-10-Improper-Server-Side-Authorization-Memberikan-Akses-tidak-sah-ke-konten-premium-7.png)

## Kesimpulan

Bug yang saya temukan ini sudah saya laporkan kepada pihak malaka dan saat ini sudah diperbaiki.

![Screenshot 2026-02-10 162442.png](/assets/image/2026-02-10-Improper-Server-Side-Authorization-Memberikan-Akses-tidak-sah-ke-konten-premium-8.png)