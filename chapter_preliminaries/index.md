# Pendahuluan
:label:`chap_preliminaries`

Untuk memulai pembelajaran mendalam,
kita perlu mengembangkan beberapa keterampilan dasar.
Semua pembelajaran mesin terkait
dengan mengekstraksi informasi dari data.
Jadi kita akan mulai dengan mempelajari keterampilan praktis
untuk menyimpan, memanipulasi, dan memproses data sebelumnya.

Selain itu, pembelajaran mesin biasanya membutuhkan
bekerja dengan kumpulan data besar, yang dapat kita anggap sebagai tabel,
dimana baris sesuai dengan sampel 
dan kolom sesuai dengan atribut.
Aljabar linier memberi kita seperangkat teknik yang ampuh
untuk bekerja dengan data tabel.
Kita tidak akan terlalu jauh membahasnya tetapi lebih fokus pada yang dasar
operasi matriks dan implementasinya.

Selain itu, pembelajaran mendalam adalah tentang optimasi.
Kita memiliki sebuah model dengan beberapa parameter dan
kita ingin menemukan parameter yang paling sesuai dengan data kami.
Menentukan ke arah mana harus memindahkan setiap parameter pada setiap langkah algoritma
membutuhkan sedikit kalkulus, yang akan diperkenalkan secara singkat.
Untungnya, paket `autograd` secara otomatis menghitung diferensiasi untuk kita,
dan kami akan membahasnya selanjutnya.

Selanjutnya, pembelajaran mesin berkaitan dengan membuat prediksi:
apa kemungkinan nilai dari beberapa atribut yang tidak diketahui,
berdasarkan informasi yang kita amati?
Untuk bernalar secara mendalam di bawah ketidakpastian
kita perlu menggunakan bahasa probabilitas.

Pada akhirnya, dokumentasi resmi menyediakan
banyak deskripsi dan contoh yang berada di luar buku ini.
Sebagai penutup bab ini, kami akan menunjukkan kepada Anda bagaimana mencari dokumentasi
informasi yang dibutuhkan.

Buku ini matematika dituliskan seminimal mungkin sekadar 
untuk mendapatkan pemahaman yang tepat tentang pembelajaran mendalam.
Namun, bukan berarti buku ini bebas matematika.
Dengan demikian, bab ini memberikan pengantar singkat tentang
matematika dasar dan matematika yang sering dipakai agar siapa pun bisa memahami
setidaknya *sebagian besar* dari konten matematika buku ini.
Jika Anda ingin memahami *semua* konten matematika di buku ini,
meninjau lebih lanjut [lampiran online tentang matematika](https://d2l.ai/chapter_appendix-mathematics-for-deep-learning/index.html) sudah cukup.

```toc
:maxdepth: 2

ndarray
pandas
linear-algebra
calculus
autograd
probability
lookup-api
```