---
layout: post
title: "Cara Install Valgrind di Gentoo"
modified: 2016-01-27 10:21++07:00
tags: [gentoo,valgrind]
category: linux
image:
  feature: 
  credit: 
  creditlink: 
comments: true
share: true
---
Bagi yang sering berjibaku denga bahasa pemrograman C pasti sudah tidak asing dengan Valgrind.

Melalui [situsnya][0], Valgrind adalah *framework* instrumentasi untuk membangun *tool-tool* analisis dinamis. Bahasa simpelnya, Valgrind bisa dipakai untuk mendeteksi bug dan *profiling* program C.

Umumunya Valgrind sudah disediakan secara *default* di repositori, termasuk Gentoo, tapi butuh sedikit trik supaya Valgrind berjalan lancar. Tanpa trik, Anda bisa mendapatkan error seperti di bawah ini.

Buat skrip Hello World dengan C,

    #include <stdio.h>
    int main(){
      puts("Hello world!");
      return 0;
    }

Silakan kompil dengan `gcc` atau `make`, lalu jalankan dengan Valgrind.

*Output* akan menghasilkan seperti ini:

    ==17145== Memcheck, a memory error detector
    ==17145== Copyright (C) 2002-2013, and GNU GPL'd, by Julian Seward et al.
    ==17145== Using Valgrind-3.10.1 and LibVEX; rerun with -h for copyright info
    ==17145== Command: ./hello-world
    ==17145== 
    
    valgrind:  Fatal error at startup: a function redirection
    valgrind:  which is mandatory for this platform-tool combination
    valgrind:  cannot be set up.  Details of the redirection are:
    valgrind:  
    valgrind:  A must-be-redirected function
    valgrind:  whose name matches the pattern:      strlen
    valgrind:  in an object with soname matching:   ld-linux-x86-64.so.2
    valgrind:  was not found whilst processing
    valgrind:  symbols from the object with soname: ld-linux-x86-64.so.2
    valgrind:  
    valgrind:  Possible fixes: (1, short term): install glibc's debuginfo
    valgrind:  package on this machine.  (2, longer term): ask the packagers
    valgrind:  for your Linux distribution to please in future ship a non-
    valgrind:  stripped ld.so (or whatever the dynamic linker .so is called)
    valgrind:  that exports the above-named function using the standard
    valgrind:  calling conventions for this platform.  The package you need
    valgrind:  to install for fix (1) is called
    valgrind:  
    valgrind:    On Debian, Ubuntu:                 libc6-dbg
    valgrind:    On SuSE, openSuSE, Fedora, RHEL:   glibc-debuginfo
    valgrind:  
    valgrind:  Cannot continue -- exiting now.  Sorry.

## Instalasi di Gentoo

Garis besar untuk mengatasi error seperti di atas.

1. Tambahkan USE flag `debug` di glibc.

2. Tambahkan `FEATURES="splitdebug"` di make.conf.

3. Kompil glibc dan valgrind lagi.

Langkah kedua bukanlah cara terbaik. Dengan menambahkan variabel baru di make.conf, berarti Gentoo akan mengaktifkannya secara global, semua paket akan dikompil ulang, dan itu butuh waktu yang laaamaaaa. Oleh karena itu lakukan hal di bawah ini.

Buatlah berkas debug.conf di /etc/portage/env/ dengan isi:

    CFLAGS="${CFLAGS} -ggdb"
    CXXFLAGS="${CXXFLAGS} -ggdb"
    FEATURES="splitdebug"

Lalu buat berkas baru (terserah namanya) di /etc/portage/package.env/ dengan isi:

    sys-libs/glibc debug.conf
    dev-util/valgrind debug.conf

Taaraa, tinggal jalankan langkah ketiga, Valgrind Anda berjalan sukses!

[0]: http://valgrind.org/
