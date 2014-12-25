---
published: true
title: Setelah Update Pack 6 Manjaro
layout: post
comments: "true"
share: "true"
description: "Ada poin-poin perbedaan setelah mengupdate Manjaro"
redirect_from: "/2014/05/setelah-update-pack-6-manjaro/"
category: 
  - linux
---

Setelah melakukan *update-pack 6* Manjaro kemarin, ada beberapa update yang cukup berarti bagi pengguna Manjaro edisi XFCE.

Salah satunya adalah shortcut pada **xfce4-popup-whiskermenu**. Pada versi sebelumnya ketika menekan tombol **super**, menu akan muncul ketika tombol baru ditekan (*on press*). Sedangkan pada versi baru, **xfce4-popup-whiskermenu** baru muncul ketika tombol terlepas dari tekanan jari (*on release*).

![gedit](/images/gedit-3.12.1.png)

Beberapa aplikasi resmi mengimplementasikan GTK 3, ini terlihat dari tampilan title bar dan kotak dialognya, salah satunya adalah Gedit (3.12.1).

![dialog box gtk3](/images/dialog-box-gtk3.png)

Firefox versi 29 juga terpasang di *Update Pack 6*.

Untuk mengupdate Manjaro kali ini, kita tidak disarankan untuk menggunakan `pamac` karena ada satu package (saya lupa namanya) yang di downgrade, sehingga kita harus melakukan *force downgrade* menggunakan perintah `pacman -Suu`.
