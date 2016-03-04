---
layout: post
title: "Cara Install Gentoo"
modified: 2016-03-04 21:12++07:00
tags: [gentoo]
category: linux
image:
  feature: 
  credit: abstract-4.jpg
  creditlink: 
comments: true
share: true
---
Berawal dari pesan yang saya terima melalui Telegram, apakah di blog ini mengulas cara instal Gentoo. Yang kemudian saya jawab, tidak ada, karena Anda bisa mencarinya di laman [wiki][0]. Saya cukup merasa bersalah, mengapa saya tak menuliskannya sendiri?

Oleh karena itu, saya menyediakan tulisan mengenai cara instal Gentoo berikut ini.

> Saya asumsikan Anda akan menginstal Gentoo untuk arsitektur **x86\_64 (amd64)** dengan sistem init **OpenRC**, memakai editor *vim* dan menggunakan *MBR non-UEFI*.

# Gentoo?

Gentoo Linux (dulu Enoch Linux) adalah sistem operasi GNU/Linux berbasis *source code*, atau sering disebut *source based distro*. Istilah ini mengacu pada instalasi paket yang di*compile* sendiri di komputer pengguna.

Gentoo sangat populer dengan filosofi "*it's all about choice*". Pengguna dibebaskan untuk mengolah sistem operasi ini sesuai dengan keinginan mereka.

Penjelasan lengkap mengenai sistem operasi ini bisa dibaca di [Wikipedia][1].

# Instalasi Gentoo

## Persiapan

### 1. Media instalasi

Media instalasi adalah sebuah sebutan untuk media atau tempat menginstal Gentoo. Media atau tempat ini bukan berarti perangkat komputernya lho yaa.

Media instalasi Gentoo bisa apa saja, dalam arti Anda tidak perlu menggunakan Live CD (Minimal Installation CD) Gentoo. Anda bisa mengunduh Live CD apapun.

Jika Anda *kebelet* ingin memakai distro berasa Gentoo, Anda bisa menggunakan [SystemRescueCd][2], distro Linux berbasis Gentoo, sebagai media instalasinya.

Tidak seperti GNU/Linux berbasis GUI (Ubuntu, Fedora, openSUSE, dsb), instalasi Gentoo menggunakan fitur `chroot`, sehingga calon pengguna Gentoo perlu memakai Live CD (atau sistem operasi GNU/Linux yang sudah terinstal sebelumnya di perangkat yang sama) untuk menginstal distro berlogo "Magatama" ini.

### 2. Unduh berkas stage3

Berkas stage merupakan berkas pondasi penginstalan Gentoo. Berkas ini berisi *compiler* (gcc), berkas-berkas utama GNU/Linux (/bin,/usr, dsb), dan berkas lainnya.

Jika Anda sudah membaca laman [Wikipedia tentang Gentoo][1], Anda seharusnya sudah tahu mengapa yang disediakan adalah berkas stage3, bukan stage 1 atau stage 2.

Anda bisa mengunduh berkas stage3 di [tautan ini][3].

## Mulai instal

### 1. Partisi

Jika media instalasi Anda membawa aplikasi [Gparted][4], pakai saja. Jika tidak ada, Anda bisa memakai `cfdisk`.

Rekomendasi partisi dari saya:

/dev/sda1 (2MB) sebagai *bootloader*

/dev/sda2 (128MB) ext2 atau ext4, sebagai partisi /boot, **tidak wajib**

/dev/sda3 (sesuai kebutuhan) sebagai partisi swap

/dev/sda4 (50GB) ext4, sebagai partisi root

/dev/sda5 (sisanya) ext4, sebagai partisi /home, **tidak wajib**

Format /dev/sda2, /dev/sda4, dan /dev/sda5 dengan:

    # mkfs.ext4 /dev/sda2
    # mkfs.ext4 /dev/sda4
    # mkfs.ext4 /dev/sda5

Aktifkan partisi swap dengan:

    # mkswap /dev/sda3
    # swapon /dev/sda3

### 2. Mount partisi Gentoo

Silakan mount partisi root Gentoo ke /mnt.

    # mkdir -p /mnt/gentoo
    # mount /dev/sda4 /mnt/gentoo

Langkah mount ini dipakai untuk melakukan `chroot` nanti.

### 3. Ekstrak stage3 ke /mnt/gentoo

*Copy*kan berkas stage3 ke /mnt/gentoo.

    # cp ~/Dowloads/stage3-amd64-20160225.tar.bz2 /mnt/gentoo

Ekstrak stage3.

    # cd /mnt/gentoo
    # tar -xf stage3-amd64-20160225.tar.bz2

### 4. Edit berkas /mnt/gentoo/etc/portage/make.conf

Tambahkan konfigurasi di bawah ini ke berkas tersebut.

    CFLAGS="-march=native -O2"
    CXXFLAGS="${CFLAGS}"
    MAKEOPTS="-j2"

Biarkan CFLAGS dan CXXFLAGS seperti itu. Khusus untuk opsi -j dalam MAKEOPTS, Anda harus memperhatikan *core* prosesor yang Anda miliki. Rumus untuk mengisi opsi -j di MAKEOPTS adalah *core*+1. Misalnya, saya mempunyai 1 *core* maka nilai -j adalah 2, dan bila saya mempunyai 4 *core*, nilai -j adalah 5.

MAKEOPTS digunakan untuk menentukan banyaknya komputasi paralel yang akan dilakukan saat mengompil paket.

### 5. Copy konfigurasi repositori Gentoo

    # mkdir -p /mnt/gentoo/etc/portage/repos.conf
    # cp /mnt/gentoo/usr/share/portage/config/repos.conf /mnt/gentoo/etc/portage/gentoo.conf

### 6. Copy konfigurasi DNS ke /mnt/gentoo

    # cp -L /etc/resolv.conf /mnt/gentoo/etc/

Cara ini dipakai untuk meng-*online*-kan lingkungan *chroot*.

### 7. Masuk ke lingkungan chroot Gentoo

Sebelum ke lingkungan chroot Gentoo, mount terlebih dahulu beberapa *device file* yang penting.

    # mount -t proc proc /mnt/gentoo/proc
    # mount --rbind /sys /mnt/gentoo/sys
    # mount --rbind /dev /mnt/gentoo/dev

Chroot ke Gentoo.

    # chroot /mnt/gentoo /bin/bash
    # source /etc/profile

### 8. Perbarui *snapshot* Portage

Snapshot Portage merupakan kumpulan berkas yang memberitahu Portage, perangkat lunak apa yang bisa diinstal, profil mana yang bisa dipilih, dan lainnya.

Jalankan,

    # emerge-webrsync

Perintah ini akan mengambil snapshot Portage terbaru dari repositori Gentoo, dan menginstalnya ke sistem.

### 9. Pilih profil Gentoo

Profil akan menentukan bagaimana sistem operasi Gentoo Anda berjalan. Profil akan menentukan konfigurasi *default*, dan akan mengaturnya sedemikian rupa sesuai dengan profil yang dipilih.

Misalnya, profil default/linux/amd64/13.0/desktop akan mengaktifkan USE Flag "X" secara *default*, berbeda dengan profil default/linux/amd64/13.0 yang hanya membawa konfigurasi *basic* sebuah distro Linux.

Untuk melihat semua profil:

    # eselect profile list
    Available profile symlink targets:
      [1]   default/linux/amd64/13.0
      [2]   default/linux/amd64/13.0/selinux
      [3]   default/linux/amd64/13.0/desktop *
      [4]   default/linux/amd64/13.0/desktop/gnome
      [5]   default/linux/amd64/13.0/desktop/gnome/systemd
      [6]   default/linux/amd64/13.0/desktop/kde
      [7]   default/linux/amd64/13.0/desktop/kde/systemd
      [8]   default/linux/amd64/13.0/desktop/plasma
      [9]   default/linux/amd64/13.0/desktop/plasma/systemd
      [10]  default/linux/amd64/13.0/developer
      [11]  default/linux/amd64/13.0/no-multilib
      [12]  default/linux/amd64/13.0/systemd
      [13]  default/linux/amd64/13.0/x32
      [14]  hardened/linux/amd64
      [15]  hardened/linux/amd64/selinux
      [16]  hardened/linux/amd64/no-multilib
      [17]  hardened/linux/amd64/no-multilib/selinux
      [18]  hardened/linux/amd64/x32
      [19]  hardened/linux/musl/amd64
      [20]  hardened/linux/musl/amd64/x32
      [21]  default/linux/uclibc/amd64
      [22]  hardened/linux/uclibc/amd64

Karena kita akan membangun Gentoo desktop, saya akan memilih profil 3.

    # eselect profile set 3

### 10. Atur timezone

Lihat daftar timezone di /usr/share/zoneinfo/.

    # ls /usr/share/zoneinfo/

Untuk mengaturnya,

    # echo "Asia/Jakarta" > /etc/timezone

Konfigurasi ulang dengan perintah,

    # emerge --config sys-libs/timezone-data

### 11. Atur locales

    # vim /etc/locale.gen

Untuk pengaturan Indonesia, Anda hanya butuh:

    en_US ISO-8859-1
    en_US.UTF-8 UTF-8

Jalankan perintah di bawah untuk men-*generate* pengaturan locale.

    # eselect locale list
    Available targets for the LANG variable:
      [1]   C
      [2]   en_US
      [3]   en_US.iso88591
      [4]   en_US.utf8
      [5]   POSIX
      [ ]   (free form)
    # eselect locale set 4

Untuk mengaktifkan peraturan ini, Anda perlu memuat ulang lingkungan Gentoo dengan,

    # env-update && source /etc/profile

### 12. Atur dan instal Kernel Linux

Ada dua cara instal kernel di Gentoo; dengan cara manual dan cara otomatis (dengan genkernel).

Saya hanya akan mengulas cara manualnya, karena cara inilah yang sesuai dengan alasan mengapa Gentoo ini dibuat.

#### 12.1 Persiapan

Di sesi ini, Anda harus benar-benar jeli. Komponen mana yang perlu di aktifkan melalui menuconfig dan komponen mana yang dibiarkan tidak aktif. Oleh karena itu, Anda tentu butuh perkakas `lspci` untuk melihat jenis dan nama komponen PCI yang ada di komputer Anda.

    # emerge -av sys-apss/pciutils

#### 12.2 Mulai instal

Instal kernel Linux (gentoo-sources),

    # emerge -av sys-kernel/gentoo-sources
    # ls -l /usr/src/linux
    lrwxrwxrwx 1 root root 22 Jan 23 21:02 /usr/src/linux -> linux-4.1.15-gentoo-r1

Atur konfigurasi Gentoo dengan menuconfig,

    # cd /usr/src/linux
    # make menuconfig

Anda akan melihat tampilan konfigurasi kernel seperti di bawah ini,

![menuconfig](http://rmdzn.kilatstorage.com/image/menuconfig-lnx.png)

Saya akan menuliskan beberapa hal yang perlu diaktifkan, yang saya ambil dari Handbook Gentoo.

Untuk mengaktifkan devtmpfs,

    Device Drivers --->
      Generic Driver Options --->
        [*] Maintain a devtmpfs filesystem to mount at /dev
        [ ]   Automount devtmpfs at /dev, after the kernel mounted the rootfs

Untuk mengaktifkan sistem berkas /proc dan yang Anda pakai (contoh ext3, ext4, Raiser, JFS, XFS).

    File systems --->
    (Select one or more of the following options as needed by your system)
      <*> Second extended fs support
      <*> Ext3 journalling file system support
      <*> The Extended 4 (ext4) filesystem
      <*> Reiserfs support
      <*> JFS filesystem support
      <*> XFS filesystem support
      ...
      Pseudo Filesystems --->
        [*] /proc file system support
        [*] Virtual memory file system support (former shm fs)

Untuk mengaktifkan *driver* untuk koneksi PPP (menjalankan modem USB).

    Device Drivers --->
      Network device support --->
        <*> PPP (point-to-point protocol) support
        <*>   PPP support for async serial ports
        <*>   PPP support for sync tty ports

Untuk mengaktifkan SMP.

    Processor type and features  --->
      [*] Symmetric multi-processing support

Untuk mengaktifkan USB.

    Device Drivers --->
      [*] HID Devices  --->
        <*>   USB Human Interface Device (full HID) support

Setelah yakin, Anda bisa menyimpan konfigurasi dan keluar. Lalu *compile* dengan perintah,

    # make && make modules_install

Salin hasil kompilasi ke /boot dengan perintah,

    # make install

Beberapa perangkat butuh firmware, Anda bisa menginstal paket linux-firmware.

    # emerge -av sys-kernel/linux-firmware

### 13. Atur /etc/fstab

Misalnya seperti ini,

    /dev/sda2		/boot		ext4		noauto,noatime	1 2
    /dev/sda3		none		swap		sw		0 0
    /dev/sda4		/		    ext4		noatime		0 1
    /dev/sda5		/home		ext4		noatime		0 2

### 14. Atur /etc/conf.d/hostname

Berkas ini dipakai untuk mengatur nama host perangkat, misalnya

    hostname="localhost"

### 15. Atur koneksi internet

Edit berkas /etc/conf.d/net

Untuk koneksi internet kabel, tambahkan

    config_eth0="DHCP"

Untuk koneksi Wi-Fi, tambahkan

    config_wlan0="DHCP"

Saya rekomendasikan Anda pakai wpa\_supplicant untuk membangun koneksi dengan Wi-Fi.

    # emerge -av wpa_supplicant

Jika Anda ingin membangun koneksi internet dengan modem USB, Anda bisa membaca tulisan saya di Kabar Linux: [Cara Menggunakan Modem USB di Gentoo][5].

### 16. Atur jam di /etc/conf.d/hwclock

Ada dua pengaturan di sini. Yang pertama UTC, yang kedua lokal. Di Indonesia, Anda harus menggunakan lokal.

    clock="local"

### 17. Pasang *system logger*

    # emerge -av syslog-ng

Tambahkan ke runlevel default,

    # rc-update add syslog-ng default

### 18. Pasang cron

    # emerge -av sys-proccess/cronie

Tambahkan ke runlevel default,

    # rc-update add cronie default

### 19. Instal klien DHCP

    # emerge -av net-misc/dhcpcd

### 20. Atur bootloader dengan GRUB2

Instal GRUB2,

    # emerge -av sys-boot/grub:2

Pasang berkas-berkas GRUB2 ke /boot/grub/,

    # grub2-install /dev/sda

*Generate* menu-menu grub,

    # grub2-mkconfig -o /boot/grub/grub.cfg

### 21. Keluar dari chroot dan unmount /mnt/gentoo

    # exit

    # cd
    # umount -l /mnt/gentoo/dev{/shm,/pts,}
    # umount /mnt/gentoo{/boot,/sys,/proc,}

&nbsp;
&nbsp;
&nbsp;

Saatnya reboot dan masuk ke Gentoo Anda. Selamat!

[0]: https://wiki.gentoo.org/wiki/Handbook:Main_Page
[1]: https://en.wikipedia.org/wiki/Gentoo_Linux
[2]: http://www.sysresccd.org/
[3]: https://www.gentoo.org/downloads/
[4]: http://gparted.sourceforge.net/
[5]: http://kabarlinux.web.id/2015/cara-menggunakan-modem-usb-di-gentoo/

# Gentoo?

Ge¡
