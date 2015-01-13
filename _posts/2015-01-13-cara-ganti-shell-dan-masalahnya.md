---
layout: post
title: "Cara Ganti Shell dan Masalahnya"
modified: 2015-01-13 22:14:18 +0700
tags: [zsh,shell]
category: linux
image:
  feature: 
  credit: 
  creditlink: 
comments: true
share: true
---

![zshell](/images/zsh.png)
Mengganti default shell pada sebuah distro adalah hal yang sangat mudah. Yaitu dengan perintah `chsh`.

Misalnya ketika kita akan mengganti `bash` dengan `zsh`.

    $ chsh -s $(which zsh)
    Changing shell for ramdzi.
    Password: 
    Shell changed.

## Masalah pertama
Penggantian default shell ini tak selalu lancar, seperti munculnya masalah

    chsh: Shell not change

Masalah ini bisa diatasi dengan mengedit `/etc/passwd`.

    $ sudo vipw

Edit pada bagian 

    ramdzi:x:1000:100:Ramdziana F Y:/home/ramdzi:/bin/zsh

Perhatikan pada `/bin/zsh`.

Setelah itu silakan jalankan perintah `chsh`.

## Masalah kedua

Ada kalanya setelah menginstall `zsh`, di `/etc/passwd` sudah terdapat `/usr/bin/zsh`.

    ramdzi:x:1000:100:Ramdziana F Y:/home/ramdzi:/usr/bin/zsh

Silakan ubah `/usr/bin/zsh` ke `/bin/zsh`. Jangan paksa menjadi

    ramdzi:x:1000:100:Ramdziana F Y:/home/ramdzi:/usr/bin/zsh:/bin/zsh

karena akan muncul masalah baru,

    your shell is not in /etc/shells, shell change denied: Permission denied
