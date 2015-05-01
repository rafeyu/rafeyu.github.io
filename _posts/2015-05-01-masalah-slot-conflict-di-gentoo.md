---
layout: post
title: "Masalah Slot Conflict di Gentoo"
modified: 2015-05-01 11:31:37 +0700
tags: [gentoo,conflict,emerge]
category: gentoo
image:
  feature: 
  credit: 
  creditlink: 
comments: true
share: true
---

*Slot conflict* biasanya hadir ketika pengguna akan memasang paket yang mempunyai CFLAGS berlawanan dengan paket yang lain. 

Hal yang baru saja saya alami adalah ketika ingin memasang paket `unbound`. `Unbound` membutuhkan paket `openssl` dengan CFLAGS **tanpa** `bindist`, padahal `openssh` memerlukan `openssl` **dengan** `bindist`. Galat yang muncul akan seperti ini:

```
# emerge -av unbound

Calculating dependencies... done!
[ebuild  N     ] net-dns/dnssec-root-20110630::gentoo  USE="{-test}" 2 KiB
[ebuild   R    ] dev-libs/openssl-1.0.1l-r1::gentoo  USE="tls-heartbeat zlib -bindist* -gmp -kerberos -rfc3779 -static-libs {-test} -vanilla" ABI_X86="(64) -32 (-x32)" CPU_FLAGS_X86="(sse2)" 0 KiB
[ebuild  N     ] net-dns/unbound-1.5.1-r2::gentoo  USE="ecdsa -debug -dnstap -gost -python (-selinux) -static-libs {-test} -threads" ABI_X86="(64) -32 (-x32)" PYTHON_TARGETS="python2_7" 4,693 KiB

Total: 3 packages (2 new, 1 reinstall), Size of downloads: 4,694 KiB

# required by media-video/openshot-1.4.3[ffmpeg]
!!! Multiple package instances within a single package slot have been pulled
!!! into the dependency graph, resulting in a slot conflict:

dev-libs/openssl:0

  (dev-libs/openssl-1.0.1l-r1:0/0::gentoo, ebuild scheduled for merge) pulled in 
  by dev-libs/openssl:0[-bindist] required by
      (net-dns/unbound-1.5.1-r2:0/0::gentoo, ebuild scheduled for merge)
                             ^^^^^^^^                                                                                                                    
                             
      (dev-libs/openssl-1.0.1l-r1:0/0::gentoo, installed) pulled in by
         >=dev-libs/openssl-0.9.6d:0[bindist=] required by
         (net-misc/openssh-6.7_p1:0/0::gentoo, installed)
```

Sebenarnya, galat (*error*) muncul akibat paket `openssh` yang menggunakan CFLAGS `bindist`. Segala paket dengan CFLAGS `bindist` akan menjadi dependensi paket yang ber-CFLAGS `bindist` pula, dan sebaliknya. Dalam kasus ini:

```
openssh (bindist) -> openssl (bindist) -> unbound (-bindist)
```

`openssl` akan selalu konflik dengan `unbound`, selama kebutuhan CFLAGS `bindist` berbeda (`unbound` tidak memerlukan `bindist`, `openssl` masih memakai `bindist`, `openssh` pakai `bindist`).

Untuk menyelesaikan konflik ini, saya hanya perlu meng-`emerge` ulang `openssh` dan `openssl` tanpa CFLAGS `bindist`.

```
openssh (-bindist) -> openssl (-bindist) -> unbound (-bindist)
```
