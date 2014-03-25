---
layout: post
title: "Algoritma Backward Chaining"
modified: 2014-03-25 17:04:25 +0700
tags: [algoritma,sistem pakar,runut balik,inferensi]
category: kuliah
image:
  feature: 
  credit: 
  creditlink: 
comments: true
share: true
---

Algoritma *backward chaining* atau runut balik adalah salah satu algoritma pada sistem pakar.

Menurut saya cukup membingungkan apabila dibandingkan dengan algoritma *forward chaining* / runut maju, namun saya yakin dengan memperhatikan pola pengerjaan, algoritma ini cukup mudah dipahami.

Kunci algoritma ini adalah pada premis dan konklusi, layaknya *forward chaining*, perbedaanya adalah algoritma *backward chaining* memeriksa konklusi (`THEN ..`) terlebih dahulu, yang kemudian dicari premisnya (`IF ..`). Dan tidak lupa, algoritma *backward chaining* menggunakan *stack* untuk penyimpanan memori sementara.

# Contoh Kasus
## Aturan/rules basis pengetahuannya

<table>
<tr><td>R1	: IF ( A AND B) THEN C</td></tr>
<tr><td>R2	: IF C THEN D</td></tr>
<tr><td>R3	: IF (A AND E) THEN F</td></tr>
<tr><td>R4	: IF A THEN G</td></tr>
<tr><td>R5	: IF (F AND G) THEN D</td></tr>
<tr><td>R6	: IF (G AND E) THEN H</td></tr>
<tr><td>R7	: IF (C AND H) THEN I</td></tr>
<tr><td>R8	: IF (I AND A) THEN J</td></tr>
<tr><td>R9	: IF  G THEN J</td></tr>
<tr><td>R10	: IF J THEN K</td></tr></table>

Fakta awal yang diberikan adalah A dan F, buktikan apakah K bernilai benar apabila proses inferensi dilakukan dengan cara *backward chaining*?

## Jawab

## Langkah 1
<table>
	<tr><td>**Database**:</td>
	<td>A F</td>
	</tr>
</table>

<table>
	<tr><td>**Stack**:</td>
		<td>K</td>
	</tr>
</table>

**Goal**: K (sebagai isi awal dari stack). J tidak ada di database, simpan di stack.

## Langkah 2
<table>
R1	: IF ( A AND B) THEN C

R2	: IF C THEN D

R3	: IF (A AND E) THEN F

R4	: IF A THEN G

R5	: IF (F AND G) THEN D

R6	: IF (G AND E) THEN H

R7	: IF (C AND H) THEN I

R8	: IF (I AND A) THEN **J**

R9	: IF  G THEN J

R10	: IF J THEN K</table>

<table>
	<tr><td>**Database**:</td>
	<td>A F</td>
	</tr>
</table>

<table>
	<tr><td>**Stack**:</td>
		<td>K J</td>
	</tr>
</table>

**Sub goal**: J

A ada di Database. I tidak ada di Database (simpan di stack).

## Langkah 3
<table>
R1	: IF ( A AND B) THEN C

R2	: IF C THEN D

R3	: IF (A AND E) THEN F

R4	: IF A THEN G

R5	: IF (F AND G) THEN D

R6	: IF (G AND E) THEN H

R7	: IF (C AND H) THEN **I**

R8	: IF (I AND A) THEN J

R9	: IF  G THEN J

R10	: IF J THEN K</table>

<table>
	<tr><td>**Database**:</td>
	<td>A F</td>
	</tr>
</table>

<table>
	<tr><td>**Stack**:</td>
		<td>K J I</td>
	</tr>
</table>

**Sub goal**: I

C tidak ada di database (simpan di stack). H tidak ada di database (simpan di stack).

## Langkah 4
<table>
R1	: IF ( A AND B) THEN C

R2	: IF C THEN D

R3	: IF (A AND E) THEN F

R4	: IF A THEN G

R5	: IF (F AND G) THEN D

R6	: IF (G AND E) THEN **H**

R7	: IF (C AND H) THEN I

R8	: IF (I AND A) THEN J

R9	: IF  G THEN J

R10	: IF J THEN K</table>

<table>
	<tr><td>**Database**:</td>
	<td>A F</td>
	</tr>
</table>

<table>
	<tr><td>**Stack**:</td>
		<td>K J I C H</td>
	</tr>
</table>

**Sub goal**: H

G tidak ada di database (simpan di stack). E tidak ada di database (simpan di stack).

## Langkah 5
<table>
R1	: IF ( A AND B) THEN C

R2	: IF C THEN D

R3	: IF (A AND E) THEN F

R4	: IF A THEN G

R5	: IF (F AND G) THEN D

R6	: IF (G AND E) THEN H

R7	: IF (C AND H) THEN I

R8	: IF (I AND A) THEN J

R9	: IF  G THEN J

R10	: IF J THEN K</table>

<table>
	<tr><td>**Database**:</td>
	<td>A F</td>
	</tr>
</table>

<table>
	<tr><td>**Stack**:</td>
		<td>K J I C H G E</td>
	</tr>
</table>

**Sub goal**: E

Aturan dengan konklusi E tidak ada (yang berarti gagal), maka kembali ke langkah ke 2 (namun kita akan memakai R9 dari pada R8).

## Langkah 6 (perulangan dari langkah 2)
<table>
R1	: IF ( A AND B) THEN C

R2	: IF C THEN D

R3	: IF (A AND E) THEN F

R4	: IF A THEN G

R5	: IF (F AND G) THEN D

R6	: IF (G AND E) THEN H

R7	: IF (C AND H) THEN I

R8	: IF (I AND A) THEN J

R9	: IF  G THEN **J**

R10	: IF J THEN K</table>

<table>
	<tr><td>**Database**:</td>
	<td>A F</td>
	</tr>
</table>

<table>
	<tr><td>**Stack**:</td>
		<td>K J</td>
	</tr>
</table>

**Sub goal**: J

G tidak ada di database (simpan di stack).

## Langkah 7
<table>
R1	: IF ( A AND B) THEN C

R2	: IF C THEN D

R3	: IF (A AND E) THEN F

R4	: IF A THEN **G**

R5	: IF (F AND G) THEN D

R6	: IF (G AND E) THEN H

R7	: IF (C AND H) THEN I

R8	: IF (I AND A) THEN J

R9	: IF  G THEN J

R10	: IF J THEN K</table>

<table>
	<tr><td>**Database**:</td>
	<td>A F</td>
	</tr>
</table>

<table>
	<tr><td>**Stack**:</td>
		<td>K J G</td>
	</tr>
</table>

**Sub goal**: G

A ada di database. G masukkan ke database.

## Langkah 8
<table>
R1	: IF ( A AND B) THEN C

R2	: IF C THEN D

R3	: IF (A AND E) THEN F

<s>R4	: IF A THEN G</s>

R5	: IF (F AND G) THEN D

R6	: IF (G AND E) THEN H

R7	: IF (C AND H) THEN I

R8	: IF (I AND A) THEN J

R9	: IF  G THEN **J**

R10	: IF J THEN K</table>

<table>
	<tr><td>**Database**:</td>
	<td>A F G</td>
	</tr>
</table>

<table>
	<tr><td>**Stack**:</td>
		<td>K J</td>
	</tr>
</table>

**Sub goal**: J

G ada di database. J masukkan ke database.

## Langkah 9
<table>
R1	: IF ( A AND B) THEN C

R2	: IF C THEN D

R3	: IF (A AND E) THEN F

<s>R4	: IF A THEN G</s>

R5	: IF (F AND G) THEN D

R6	: IF (G AND E) THEN H

R7	: IF (C AND H) THEN I

R8	: IF (I AND A) THEN J

<s>R9	: IF  G THEN J</s>

R10	: IF J THEN **K**</table>

<table>
	<tr><td>**Database**:</td>
	<td>A F G J</td>
	</tr>
</table>

<table>
	<tr><td>**Stack**:</td>
		<td>K</td>
	</tr>
</table>

**Sub goal**: K

J ada di database. K masukkan ke database.

## Langkah 10
<table>
R1	: IF ( A AND B) THEN C

R2	: IF C THEN D

R3	: IF (A AND E) THEN F

<s>R4	: IF A THEN G</s>

R5	: IF (F AND G) THEN D

R6	: IF (G AND E) THEN H

R7	: IF (C AND H) THEN I

R8	: IF (I AND A) THEN J

<s>R9	: IF  G THEN J</s>

<s>R10	: IF J THEN K</s></table>

<table>
	<tr><td>**Database**:</td>
	<td>A F G J K</td>
	</tr>
</table>

<table>
	<tr><td>**Stack**:</td>
		<td>(kosong)</td>
	</tr>
</table>

Karena Goal K ditemukan di database, maka proses pencarian dihentikan. Disini terbukti bahwa K bernilai benar