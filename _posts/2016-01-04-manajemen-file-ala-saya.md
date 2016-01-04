---
layout: post
title: "Manajemen File Ala Saya"
modified: 2016-01-04 22:40++07:00
tags: [file,manajemen]
category: komputer
image:
  feature: abstract-6.jpg
  credit: 
  creditlink: 
comments: true
share: true
---
Menyimpan file yang sangat banyak menjadi merepotkan apabila kita tidak me-*memanage* nya dengan baik.

Ada dua metode manajemen file yang saya pakai,

**Pertama**, menggunakan folder/direktori. Cara ini sangat konvensional, tapi efektif. Setiap file yang berhubungan dengan kegiatan atau topik tertentu dimasukkan dalam direktori tersendiri.

Misalnya, **direktori "Pramuka"** berisi file-file yang berhubungan Pramuka. Di direktori ini dibagi lagi menjadi sub-sub direktori berdasarkan tipe file; gambar, dokumen, video, dll sesuai kebutuhan. Setiap sub direktori bisa dibagi menjadi sub-sub direktori lagi, bisa berdasarkan jenis file (presentasi, sheet, pdf, dokumen biasa), atau sesuai kegiatan (kemah, rapat).

Contohnya (di Windows ganti `ls` dengan `dir`, abaikan `$`):

    $ ls Pramuka
    Gambar    Video   Dokumen
    
    $ ls Pramuka/Gambar
    ke-pantai.jpg   rapat-kemah.png
    
    $ ls Pramuka/Video
    pelantikan.mp4
    
    $ ls Pramuka/Dokumen
    rapat-1Januari16.odt    daftar-peralatan-kemah.ods

Metode pertama ini cocok dipakai untuk file-file yang disimpan dalam jangka panjang, yang ingin disimpan selamanya.

**Kedua**, menyimpan file dengan sufiks sebelum ekstensi file. Sufiks yang dimaksud berupa kata kunci atau *keyword* sesuai isi file.

Misalnya, file yang berhubungan dengan pramuka disimpan dengan sufiks "pramuka" atau disingkat menjadi "pmk"; jadwal-kemah-pmk.ods, rapat-2016-pmk.jpg, catatan-singkat-pmk.odt, dsb.

Metode kedua ini cocok dipakai untuk file yang tidak bertahan lama, yang dalam waktu dekat akan dihapus.

Mengapa cocok dipakai untuk file berumur pendek? Karena saya bisa menghapusnya dengan sangat mudah tanpa harus lewat *file manager* atau *explorer*.

Contoh (di Windows ganti `rm` dengan `del`, abaikan `$`):

Untuk menghapus semua file yang berhubungan dengan pramuka.

$ rm *-pmk.*

Untuk menghapus file gambar yang behubungan dengan pramuka.

$ rm *-pmk.jpg
$ rm *-pmk.png

&nbsp;
&nbsp;
Begitulah cara saya me-*manage* banyak file. Bagaimana dengan Anda? Ada masukkan?
