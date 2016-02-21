---
layout: post
title: "Sinkronkan Repositori Gentoo dengan Git"
modified: 2016-02-21 21:13++07:00
tags: [gentoo,git]
category: linux
image:
  feature: 
  credit: 
  creditlink: 
comments: true
share: true
---
Bulan Agustus 2015, [Gentoo mengalihkan repositorinya ke teknologi git][0]. Keputusan ini tidak hanya menguntungkan pengembang, tapi juga penggunanya.

Sebelum menggunakan git, satu-satunya jalan untuk menyinkronkan repositori adalah menggunakan rsync, yang paling tidak membutuhkan waktu 5-10 menit. Dengan git, kita bisa memangkasnya menjadi kurang dari 1 menit saja.

Beneran? Benar! Silakan ubah berkas konfigurasi repositori Gentoo terlebih dahulu, tambahkan dengan,

    [gentoo]
    location = /usr/portage
    sync-type = git
    sync-uri = https://github.com/gentoo-mirror/gentoo.git
    auto-sync = yes

Sekarang, paling tidak berkas konfigurasi Anda seperti ini.

    $ cat /etc/portage/repos.conf/gentoo.conf
    [DEFAULT]
    main-repo = gentoo
     
    [gentoo]
    location = /usr/portage
    sync-type = git
    sync-uri = https://github.com/gentoo-mirror/gentoo.git
    auto-sync = yes

Pada bagian sync-uri, Anda bisa menggunakan *mirror* Github nya (seperti di atas), atau ke alamat resminya **https://anongit.gentoo.org/git/repo/gentoo.git**.

Kemudian hapus semua berkas di /usr/portage/.

    # rm -rf /usr/portage/

Loh mengapa dihapus? Yap, karena berkas git memiliki strukturnya sendiri yang bisa dilihat di direktori .git.

Taaraaa, silakan jalankan `emerge --sync`.

Pertama kali, emerge akan meng-clone repositori, selanjutnya ia akan melakukan pull. Buktikan kecepatannya! :D

[0]: http://kabarlinux.web.id/2015/gentoo-alihkan-ke-teknologi-git/
