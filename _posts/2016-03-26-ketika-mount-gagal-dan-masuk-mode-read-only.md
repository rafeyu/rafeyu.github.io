---
layout: post
title: "Ketika Mount Gagal dan Masuk Mode Read-Only"
modified: 2016-03-26 23:08++07:00
tags: [mount,freebsd]
category: freebsd
image:
  feature: 
  credit: 
  creditlink: 
comments: true
share: true
---
Di GNU/Linux, kita biasa me-*recovery* sistem melalui fitur chroot. Sedangkan di FreeBSD, kita bisa memakai sistem *single user boot*.

Saya baru saja melakukan kesalahan pengaturan di /etc/rc.conf, yang membuat kotak FreeBSD saya kernel *panic*.

Untuk mengatasinya, saya hanya perlu boot ke FreeBSD sebagai *single user*. Lalu *edit* /etc/rc.conf dengan Vi.

Namun, ada kalanya sistem berkas sistem FreeBSD saya terkunci, hanya dalam mode baca-saja (*read-only*).

Nah untuk mengatasinya, saya perlu me-*mount* sistem supaya menjadi mode baca-tulis (*read-write*), dengan perintah:

    # mount -a -t ufs

Jika aksi mount gagal, saya menjalankan `fsck -y` terlebih dahulu. Lalu jalankan perintah mount seperti tadi.
