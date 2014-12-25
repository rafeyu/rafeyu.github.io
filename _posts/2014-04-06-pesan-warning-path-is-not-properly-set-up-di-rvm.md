---
layout: post
title: "Pesan 'Warning! PATH is not properly set up' di RVM"
modified: 2014-04-06 20:40:21 +0700
tags: [rvm,ruby,error]
category: coding
image:
  feature: 
  credit: 
  creditlink: 
comments: true
redirect_from: "/2014/04/pesan-warning-path-is-not-properly-set-up-di-rvm/"
share: true
---

Setelah saya memutuskan mengganti shell (dari bash menjadi zshell), pesan berupa warning muncul ketika saya menjalankan perintah `rvm` pada shell.

    Warning! PATH is not properly set up, '/home/ramdzi/.rvm/gems/ruby-2.1.0/bin' is not at first place, usually this is caused by shell initialization files - check them for ' PATH=...' entries, it might also help to re-add RVM to your dotfiles: 'rvm get stable --auto-dotfiles', to fix temporarily in this shell session run: 'rvm use ruby-2.1.0'

Padahal sebelumnya dengan bash pesan ini tidak muncul, hanya ketika setelah memakai zshell pesan ini kemudian muncul (walaupun saya jalankan lagi bash, pesan ini tetap muncul).

Saya sudah mencoba menggunakan perintah `rvm get head --auto-dotfiles` tetap tidak bisa. Saya tidak mencoba perintah `rvm get stable --auto-dotfiles` karena RVM saya sudah versi terbaru, *ogah* download-download lagi :v.

Ya sudah, saya coba pindahkan direktori `/home/ramdzi/.rvm/gems/ruby-2.1.0/bin` dari tengah ke awal pada variable PATH. Akhirnya pun sukses, cukup simple ternyata -_- .
