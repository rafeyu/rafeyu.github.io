---
published: true
layout: post
title: "Hapus Semua Baris di Vim"
tags: [vim,editor]
category: linux
image:
  feature: 
  credit: 
  creditlink: 
comments: true
redirect_from: "/2014/12/hapus-semua-baris-di-vim/"
share: true
---

Setelah mencari beberapa cara bagaimana menghapus semua baris di editor Vim, akhirnya dapat juga. Itupun tak hanya satu cara sehingga saya bisa mencoba beberapa cara yang ditawarkan oleh Bram lewat si editor cantik ini.

Cara pertama adalah menggunakan perintah `gg dG`. `gg` berarti memindahkan kursor ke awal baris file. `dG` berarti menghapus semua baris.

Jika diuraikan: pindahkan kursor saya ke awal baris file (`gg`). Kemudian hapus (`d`) sampai akhir baris file (`G`).

Cara lain yang jitu adalah menggunakan perintah `:%d`. Asik to..
