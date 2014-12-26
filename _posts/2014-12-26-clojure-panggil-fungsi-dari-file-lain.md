---
layout: post
title: "Clojure: Panggil Fungsi dari File Lain"
modified: 2014-12-26 19:28:11 +0700
tags: [clojure]
category: coding
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

Fitur pemanggilan fungsi dari file atau berkas lain sangat diperlukan oleh para coder. Bakal repot kalau semua script yang dibuat hanya di dalam satu file *main* saja.

Ok, mungkin saja tak merepotkan kalau program yang kita buat sangat simpel nan sederhana. Beda jika program yang dibuat sudah sangat kompleks.

Nah, kalau di Coljure untuk memecah file dan kemudian dipanggil kita bisa menggunakan `require`.

Di direktori `src/`

    ; core.clj
    (ns program.core
      (:require [program.subprogram :refer [fungsi-subprogram]])
      (:gen-class))

    (defn -main []
      (fungsi-subprogram))

    ; subprogram.clj
    (ns program.subprogram)

    (defn fungsi-subprogram []
      (println "Ini fungsi di subprogram"))

Perhatikan di bagian namespace (`ns`). Nama namespace akan selalu mengikuti lokasi dan nama dari file clj.

Misalnya kalau file subprogram.clj ada di folder `src/lib/`, maka penamaan namespace akan menjadi `program.lib.subprogram`.

Bagi yang sudah familiar dengan package pada Java pasti tidak bingung dengan hal ini.
