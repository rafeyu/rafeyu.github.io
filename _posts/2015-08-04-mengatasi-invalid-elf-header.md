---
layout: post
title: "Mengatasi Invalid ElF Header"
modified: 2015-08-04 08:15+07:00
tags: [ruby,elf,compile]
category: linux
image:
  feature: abstract-10.jpg
  credit:
  creditlink:
comments: true
share: true
---
Berawal ketika saya memperbarui ruby ke `ruby-2.1.6` dan racc ke `racc-1.4.11`, saya mendapati error saat saya membangun `racc` dengan target `RUBY_TARGETS="ruby21"` berupa `invalid ELF header` pada libz.so.1.

    # cat /var/tmp/portage/dev-ruby/racc-1.4.11/work/ruby21/racc-1.4.11/ext/racc/mkmf.log

    "x86_64-pc-linux-gnu-gcc -o conftest -I/usr/include/ruby-2.1.0/x86_64-linux -I/usr/include/ruby-2.1.0/ruby/backward -I/usr/include/ruby-2.1.0 -I.    -march=native -O2 -fno-strict-aliasing -fPIC conftest.c  -L. -L/usr/lib64 -L. -Wl,-O1 -Wl,--as-needed -fstack-protector -rdynamic -Wl,-export-dynamic -Wl,--no-undefined     -lruby21  -lpthread -lgmp -ldl -lcrypt -lm   -lc"
    /usr/libexec/gcc/x86_64-pc-linux-gnu/4.8.4/cc1: error while loading shared libraries: /usr/lib64/libz.so.1: invalid ELF header
    checked program was:
    /* begin */
    1: #include "ruby.h"
    2:
    3: int main(int argc, char **argv)
    4: {
    5:   return 0;
    6: }
    /* end */

Ok, apa yang terjadi?

    $ ls -l /usr/lib64/libz*
    -rwxr-xr-x 1 root root 88456 Mar 17 22:13 /usr/lib64/libz.so
    lrwxrwxrwx 1 root root     7 Mar 17 22:13 /usr/lib64/libz.so.1 -> libz.so

    $ readelf -h /usr/lib64/libz.so
    readelf: Error: Unable to read in 0x7964 bytes of section headers
    readelf: Error: Not an ELF file - it has the wrong magic bytes at the start

libz.so di /usr/lib64/libz.so tidak dianggap sebagai berkas ELF. Berbeda dengan libz.so yang ada di /lib64.

    $ ls -l /lib64/libz*
    lrwxrwxrwx 1 root root    13 Mar 17 23:29 /lib64/libz.so.1 -> libz.so.1.2.8
    -rwxr-xr-x 1 root root 88456 Mar 17 23:29 /lib64/libz.so.1.2.8

    $ readelf -h /lib64/libz.so.1.2.8
    ELF Header:
      Magic:   7f 45 4c 46 02 01 01 00 00 00 00 00 00 00 00 00
      Class:                             ELF64
      Data:                              2's complement, little endian
      Version:                           1 (current)
      OS/ABI:                            UNIX - System V
      ABI Version:                       0
      Type:                              DYN (Shared object file)
      Machine:                           Advanced Micro Devices X86-64
      Version:                           0x1
      Entry point address:               0x2360
      Start of program headers:          64 (bytes into file)
      Start of section headers:          86792 (bytes into file)
      Flags:                             0x0
      Size of this header:               64 (bytes)
      Size of program headers:           56 (bytes)
      Number of program headers:         7
      Size of section headers:           64 (bytes)
      Number of section headers:         26
      Section header string table index: 25

Masalah sudah jelas! Saatnya copy-kan /lib64/libz.so.1.2.8 ke /usr/lib64/libz.so.

    # cp /lib64/libz.so.1.2.8 /usr/lib64/libz.so

Taraa! Kompilasi lancar dan bisa dilanjutkan.
