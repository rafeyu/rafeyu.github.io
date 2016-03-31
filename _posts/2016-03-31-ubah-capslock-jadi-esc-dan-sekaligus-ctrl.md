---
layout: post
title: "Ubah CapsLock Jadi ESC dan Sekaligus Ctrl"
modified: 2016-03-31 18:47++07:00
tags: [linux,vim]
category: vim
image:
  feature: 
  credit: 
  creditlink: 
comments: true
share: true
---

Saya adalah pengguna Vim. Bila ada perang sipil antara Vim dan Emacs, secara otomatis saya akan jadi salah satu tentara Vim, anak buah om Tiago atau om Bram. .. intermezzo

Sudah lama saya menggunakan `jk` sebagai pengganti tombol `ESC`. Di DVORAK, ini menyakitkan. Tombol `jk` di DVORAK sama dengan `cv` di QWERTY. Posisi yang sangat tidak nyaman untuk disentuh berulang-ulang kali. Sedangkan posisi jari `jk` di QWERTY sama dengan `ht` di DVORAK. Ini juga sangat menyakitkan. Sangat banyak karakter `ht` di bahasa Inggris dan untuk mengetiknya di DVORAK saya harus memencet tombol `h` dan `t` dengan selisih waktu hampir 1 detik.

Nah, saya baru dapat tips bagus dari grup Vim Indonesia: mengganti `ESC` dengan tombol `CapsLock` bila ditekan normal dan menggantinya menjadi `Ctrl` bila ditekan sedikit agak lama (*hold*).

Untuk melakukannya tuliskan dua perintah di bawah di berkas .xinitrc.

    setxkbmap -option 'caps:ctrl_modifier'
    xcape -e 'Caps_Lock=Escape'

Lalu *restart* Xorg. Selamat!
