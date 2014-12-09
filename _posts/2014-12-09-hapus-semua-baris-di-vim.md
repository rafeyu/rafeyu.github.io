---
layout: post
title: "Hapus Semua Baris di Vim"
modified: 2014-12-09 23:44:31 +0700
tags: [vim,editor]
category: linux
image:
  feature: 
  credit: 
  creditlink: 
comments: true
share: true
---

Setelah mencari beberapa cara bagaimana menghapus semua baris di editor Vim, akhirnya dapat juga. Itupun tak hanya satu cara sehingga saya bisa mencoba beberapa cara yang ditawarkan oleh Bram lewat si editor cantik ini.

Cara pertama adalah menggunakan perintah `gg dG`. `gg` berarti memindahkan kursor ke awal baris file. `dG` berarti menghapus semua baris.

Jika diuraikan: pindahkan kursor saya ke awal baris file (`gg`). Kemudian hapus (`d`) sampai akhir baris file (`G`).

Cara lain yang jitu adalah menggunakan perintah `:%d`. Asik to..
