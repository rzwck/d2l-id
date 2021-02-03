# Kata Pengantar

Hanya beberapa tahun yang lalu, tidak ada banyak ilmuwan yang belajar secara mendalam
mengembangkan produk dan layanan cerdas di perusahaan dan rintisan besar.
Ketika yang termuda di antara kita (penulis) masuk ke bidang ini,
*machine learning* tidak menjadi berita utama di surat kabar harian.
Orang tua kami tidak tahu apa itu *machine learning*,
apalagi mengapa kita lebih memilih bidang ini dibanding karir di bidang kedokteran atau hukum.
*Machine learning* adalah disiplin akademis berwawasan ke depan
dengan terapan dunia nyata yang sempit.
Dan terapan-terapan tersebut, misalnya, pengenalan ucapan dan visi komputer,
membutuhkan begitu banyak pengetahuan spesifik sehingga sering dianggap
sebagai area yang sepenuhnya terpisah di mana *machine learning* merupakan salah satu komponen kecilnya.
Pada saat itu, *neural network*, pendahulu dari model *deep learning*
yang kami fokuskan di buku ini, dianggap sebagai alat yang ketinggalan zaman.


Hanya dalam lima tahun terakhir, *deep learning* telah mengejutkan dunia,
mendorong kemajuan pesat dalam berbagai bidang seperti visi komputer,
pemrosesan bahasa alami, pengenalan ucapan otomatis,
*reinforcement learning*, dan pemodelan statistik.
Dengan kemajuan-kemajuan ini dalam genggaman, kita sekarang dapat membuat mobil swa-kemudi
dengan lebih banyak otonomi daripada sebelumnya (dan lebih sedikit otonomi
dari yang diyakini beberapa perusahaan),
sistem penjawab cerdas yang secara otomatis membuat jawaban email yang paling umum,
membantu orang-orang dari masalah *inbox* *email* yang sangat besar,
dan agen perangkat lunak yang mendominasi pemain terbaik dunia
di permainan papan seperti Go, suatu prestasi yang pernah dianggap akan datang beberapa dekade lagi.
Alat-alat ini telah memberikan dampak yang lebih luas pada industri dan masyarakat,
mengubah cara pembuatan film, diagnosis penyakit,
dan memainkan peran yang terus berkembang dalam ilmu pengetahuan dasar --- dari astrofisika hingga biologi.

## Tentang Buku Ini

Buku ini adalah upaya kami untuk membuat *deep learning* mudah didekati,
mengajari Anda konsep, konteks, dan kode.

### Satu Media Menggabungkan Kode, Matematika, dan HTML

Agar teknologi komputasi apa pun mencapai dampak penuhnya,
teknologi tersebut harus dipahami dengan baik, didokumentasikan dengan baik, dan didukung oleh
alat yang matang dan terawat dengan baik.
Ide-ide utama harus disaring dengan jelas,
meminimalkan waktu orientasi yang diperlukan untuk memutakhirkan praktisi baru.
Kode pustaka yang matang harus mengotomatiskan tugas-tugas umum,
dan kode contoh harus memudahkan praktisi
untuk mengubah, menerapkan, dan mengembangkan aplikasi umum agar sesuai dengan kebutuhan mereka.
Lihat aplikasi web dinamis sebagai contoh.
Meskipun ada banyak perusahaan, seperti Amazon,
mengembangkan aplikasi web berbasis database yang sukses di tahun 1990-an,
potensi teknologi ini untuk membantu wirausahawan kreatif
telah direalisasikan ke tingkat yang jauh lebih besar dalam sepuluh tahun terakhir,
sebagian karena pengembangan *framework* yang kuat dan terdokumentasi dengan baik.


Menguji potensi *deep learning* menghadirkan tantangan unik
karena setiap aplikasi menyatukan berbagai disiplin ilmu.
Menerapkan *deep learning* membutuhkan pemahaman semua hal berikut
(i) motivasi untuk memodelkan masalah dengan cara tertentu;
(ii) matematika dari suatu pendekatan pemodelan;
(iii) algoritma optimasi untuk menyesuaikan model dengan data;
dan (iv) teknik yang diperlukan untuk melatih model secara efisien,
menghindari masalah komputasi numerik
dan memaksimalkan perangkat keras yang tersedia.
Mengajar keterampilan berpikir kritis yang dibutuhkan untuk merumuskan masalah,
matematika untuk menyelesaikannya, dan perangkat lunak untuk mengimplementasi
solusinya, semua di satu tempat, menghadirkan tantangan yang berat.
Tujuan kami dalam buku ini adalah untuk menyajikan sumber daya lengkap 
untuk membekali para calon praktisi.

Pada saat kami memulai proyek buku ini,
tidak ada sumber daya yang 
(i) mutakhir; (ii) membahas lengkap 
*machine learning* modern dengan kedalaman teknis yang mendalam;
dan (iii) menyajikan jalinan buku teks menarik dan berkualitas dengan kode yang bersih dan dapat dijalankan yang biasanya ditemukan dalam kelas *hands-on*.
Kami menemukan banyak contoh kode untuk
bagaimana menggunakan kerangka *deep learning* yang diberikan
(misalnya, cara melakukan komputasi numerik dasar dengan matriks di TensorFlow)
atau untuk menerapkan teknik tertentu
(misalnya, cuplikan kode untuk LeNet, AlexNet, ResNets, dll)
tersebar di berbagai tulisan blog dan repositori GitHub.
Namun, contoh ini biasanya difokuskan pada
bagaimana mengimplementasikan suatu pendekatan tertentu,
tetapi tidak menjelaskan mengapa keputusan algoritmik tertentu dibuat.
Sementara beberapa sumber daya interaktif muncul secara sporadis
untuk membahas topik tertentu, misalnya, tulisan blog yang menarik
dipublikasikan di situs web [Distill](http://distill.pub), atau blog pribadi,
mereka hanya membahas topik terpilih dalam *deep learning*,
dan sering kali tidak mengandung kode.
Di sisi lain, sementara beberapa buku teks telah muncul,
terutama :cite:`Goodfellow.Bengio.Courville.2016`,
yang menawarkan survei komprehensif tentang konsep di balik *deep learning*,
namun sumber daya ini tidak menjembatani konsep dan implementasi konsep dalam kode,
terkadang membuat pembaca tidak mengerti bagaimana menerapkan konsep-konsep tersebut.
Selain itu, terlalu banyak sumber daya yang tersembunyi di balik tembok 
penyedia kursus komersial.

Kami mulai untuk membuat sumber daya yang bisa
(i) tersedia secara bebas untuk semua orang;
(ii) menawarkan kedalaman teknis yang cukup untuk memberikan titik awal dari perjalanan
untuk benar-benar menjadi ilmuwan *machine learning* terapan;
(iii) menyertakan kode yang dapat dijalankan, menunjukkan kepada pembaca *bagaimana* memecahkan masalah dalam praktik;
(iv) memungkinkan pemutakhiran cepat, baik oleh kami dan juga oleh komunitas pada umumnya;
dan (v) dilengkapi dengan [forum](http://discuss.d2l.ai)
diskusi interaktif tentang detail teknis dan forum untuk menjawab pertanyaan.

Tujuan-tujuan ini seringkali bertentangan.
Persamaan, teorema, dan kutipan paling baik dikelola dan dijabarkan di LaTeX.
Kode paling baik dijelaskan dengan Python.
Dan halaman web secara bawaan adalah HTML dan JavaScript.
Selanjutnya, kami ingin kontennya
dapat diakses baik sebagai kode yang dapat dieksekusi, sebagai buku fisik,
sebagai PDF yang dapat diunduh, dan di Internet sebagai situs web.
Saat ini tidak ada alat dan alur kerja yang cocok untuk tuntutan ini, jadi kami harus mengumpulkannya sendiri.
Kami menjelaskan pendekatan kami secara detail di :numref:`sec_how_to_contribute`.
Kami memilih GitHub untuk membagikan sumber dan mengizinkan pengeditan,
Notebook Jupyter untuk menuliskan kode, persamaan dan teks,
*Sphinx* sebagai mesin *rendering* untuk menghasilkan banyak keluaran,
dan *Discourse* untuk forum.
Meskipun sistem kami belum sempurna,
pilihan ini memberikan kompromi yang baik di antara pilihan-pilihan ini.
Kami yakin ini mungkin buku pertama yang diterbitkan
menggunakan alur kerja terintegrasi seperti ini.

### Belajar dengan Berbuat

Banyak buku teks mengajarkan serangkaian topik, masing-masing dengan detail yang lengkap.
Misalnya, buku teks Chris Bishop yang sangat bagus: cite:`Bishop.2006`,
mengajarkan setiap topik dengan sangat detail, sehingga membacanya sampai ke bab
pada regresi linier pun membutuhkan usaha yang tidak sedikit.
Walaupun para ahli menyukai buku ini karena kedetailannya,
namun untuk pemula, hal ini malah akan membatasi kegunaannya sebagai teks pengantar.

Dalam buku ini, kami akan mengajarkan sebagian besar konsep pada saatnya (*just in time*).
Dengan kata lain, Anda akan mempelajari suatu konsep tepat di saat konsep tersebut memang dibutuhkan untuk mencapai tujuan praktis tertentu.
Sementara kami meluangkan waktu di awal untuk mengajar
pendahuluan dasar, seperti aljabar linier dan probabilitas,
kami ingin Anda merasakan kepuasan melatih model pertama Anda
sebelum memikirkan tentang distribusi probabilitas yang lebih rumit.

Selain dari beberapa *notebook* awal yang menyediakan kursus kilat
tentang matematika dasar,
setiap bab berikutnya memperkenalkan konsep-konsep baru 
dan menyediakan satu contoh mandiri --- menggunakan *dataset* asli.
Hal ini menghadirkan tantangan dalam penyusunan.
Beberapa model mungkin secara logis dikelompokkan dalam satu *notebook*.
Dan beberapa ide mungkin paling baik diajarkan dengan menjalankan beberapa model secara berurutan.
Di sisi lain, ada keuntungan besar dari mengikuti
aturan *satu contoh untuk satu notebook*:
Ini membuat semudah mungkin bagi Anda untuk 
memulai proyek penelitian Anda sendiri dengan memanfaatkan kode kami.
Cukup salin sebuah *notebook* dan mulailah memodifikasinya.

Kami akan memadukan kode yang dapat dijalankan berselang dengan materi latar belakang sesuai kebutuhan.
Pada umumnya kita akan cenderung membuat perkakas tersedia dulu sebelum menjelaskannya sepenuhnya (dan kami akan menindaklanjutinya hingga menjelaskan latar belakangnya nanti).
Misalnya, kita mungkin menggunakan *stochastic gradient descent*
sebelum menjelaskan sepenuhnya mengapa ini berguna atau mengapa itu berhasil.
Ini membantu memberi praktisi amunisi untuk menyelesaikan masalah dengan cepat,
namun memerlukan kepercayaan pembaca atas beberapa pilihan solusi.

Buku ini akan mengajarkan konsep *deep learning* dari awal.
Terkadang, kami ingin masuk ke dalam detail model
yang biasanya disembunyikan dari pengguna
oleh abstraksi rumit dari *framework* *deep learning*.
Ini muncul terutama di tutorial dasar,
di mana kami ingin Anda memahami semua
yang terjadi di suatu lapis atau *optimizer*.
Dalam hal ini, kami akan sering menyajikan dua versi contoh:
di satu versi kami mengimplementasi semuanya dari awal,
hanya mengandalkan antarmuka NumPy dan *automatic differentiation*,
dan versi contoh lain yang lebih praktis,
tempat kami menulis kode ringkas menggunakan API tingkat tinggi dari *framework* *deep learning*.
Setelah kami mengajari Anda cara kerja beberapa komponen,
kita bisa menggunakan API tingkat tinggi di tutorial berikutnya.

### Konten dan Struktur

Buku ini secara umum dapat dibagi menjadi tiga bagian,
yang disajikan dengan warna berbeda di :numref:`fig_book_org`:

![Book structure](../img/book-org.svg)
:label:`fig_book_org`

* Bagian pertama mencakup dasar-dasar dan pendahuluan.
:numref:`chap_introduction` menawarkan pengantar *deep learning*.
Kemudian, di :numref:`chap_preliminaries`,
kami akan membekali Anda dengan prasyarat dasar yang diperlukan
untuk mempraktikkan *deep learning*, seperti cara menyimpan dan memanipulasi data,
dan bagaimana menerapkan berbagai operasi numerik berdasarkan konsep dasar
dari aljabar linier, kalkulus, dan probabilitas.
: numref: `chap_linear` dan: numref:` chap_perceptrons`
mencakup konsep dan teknik paling dasar dari *deep learning*,
seperti regresi linier, perceptron multi lapis dan regularisasi.

* Lima bab berikutnya berfokus pada teknik *deep learning* modern.
:numref:`chap_computation` menjelaskan berbagai komponen kunci dari kalkulasi *deep learning* 
dan meletakkan dasar bagi kami untuk kemudian mengimplementasikan model yang lebih kompleks.
Selanjutnya, di :numref:`chap_cnn` dan :numref:`chap_modern_cnn`,
kami memperkenalkan *convolutional neural network* (CNN), alat canggih
yang menjadi tulang punggung sistem visi komputer modern.
Selanjutnya, di :numref:`chap_rnn` dan :numref:`chap_modern_rnn`, kami perkenalkan
*recurrent neural network* (RNN), model yang mengeksploitasi
struktur temporal atau sekuensial dalam data, dan biasanya digunakan
untuk pemrosesan bahasa alami dan prediksi deret waktu.
Dalam :numref:`chap_attention`, kami memperkenalkan kelas model baru
yang menggunakan teknik yang disebut mekanisme perhatian (*attention mechanism*)
yang baru-baru ini mulai menggantikan RNN dalam pemrosesan bahasa alami.
Bagian ini akan membekali Anda dengan alat dasar 
di balik sebagian besar aplikasi modern dari *deep learning*.

* Bagian tiga membahas tentang skalabilitas, efisiensi, dan aplikasi.
Pertama, dalam :numref:`chap_optimization`,
kami membahas beberapa algoritma optimasi yang umum
digunakan untuk melatih model *deep learning*.
Bab berikutnya, :numref:`chap_performance` membahas beberapa faktor kunci
yang memengaruhi performa komputasi kode *deep learning* Anda.
Dalam :numref:`chap_cv`,
kami menggambarkan
aplikasi utama *deep learning* dalam visi komputer.
Dalam :numref:`chap_nlp_pretrain` dan :numref:`chap_nlp_app`,
kami menunjukkan bagaimana melakukan *pre-training* untuk model representasi bahasa dan menerapkannya
untuk tugas pemrosesan bahasa alami.

### Kode
:label:`sec_code`

Sebagian besar bagian buku ini menampilkan kode yang dapat dieksekusi karena keyakinan kami
akan pentingnya pengalaman belajar interaktif dalam *deep learning*.
Saat ini, intuisi tertentu hanya dapat dikembangkan melalui *trial and error*,
mengubah kode dan mengamati hasilnya.
Idealnya, teori matematika yang elegan mungkin bisa memberi tahu kita
tepatnya cara mengubah kode kita untuk mencapai hasil yang diinginkan.
Sayangnya, saat ini, teori-teori elegan seperti itu luput dari perhatian kita.
Meskipun dengan upaya terbaik kami, penjelasan formal untuk berbagai teknik
masih kurang, baik karena matematika dari model tersebut
bisa sangat sulit dan juga karena penelitian serius tentang topik-topik ini
mengalami percepatan tinggi.
Kami berharap seiring dengan berkembangnya teori *deep learning*,
edisi mendatang dari buku ini akan dapat memberikan penjelasan mengenai hal-hal yang
belum bisa dijelaskan di edisi sekarang.

Terkadang, untuk menghindari pengulangan yang tidak perlu, kami merangkum
fungsi yang sering diimpor dan dirujuk, kelas dalam buku ini di paket `d2l`.
Untuk blok apa pun seperti fungsi, kelas, atau beberapa *import*
yang akan disimpan dalam paket tersebut, kami akan menandainya dengan
`#@save`. Kami menawarkan ikhtisar mendetail dari fungsi dan kelas ini di: numref: `sec_d2l`.
Paket `d2l` ringan dan hanya membutuhkan
paket dan modul berikut sebagai dependensi:

```{.python .input}
#@tab all
#@save
import collections
from collections import defaultdict
from IPython import display
import math
from matplotlib import pyplot as plt
import os
import pandas as pd
import random
import re
import shutil
import sys
import tarfile
import time
import requests
import zipfile
import hashlib
d2l = sys.modules[__name__]
```

:begin_tab:`mxnet`
Sebagian besar kode dalam buku ini didasarkan pada Apache MXNet.
MXNet adalah *framework* sumber terbuka untuk *deep learning*
dan *framework* favorit untuk AWS (Amazon Web Services),
serta banyak perguruan tinggi dan perusahaan.
Semua kode dalam buku ini telah lulus tes pada versi MXNet terbaru.
Namun, karena perkembangan pesat dalam *deep learning*, beberapa kode
*dalam edisi cetak* mungkin tidak berfungsi dengan baik di versi MXNet mendatang.
Namun, kami berencana untuk terus memutakhirkan edisi online buku ini.
Jika Anda mengalami masalah seperti itu,
silakan lihat :ref:`chap_installation`
untuk memutakhirkan kode dan *runtime environment* Anda.

Berikut adalah cara kami mengimpor modul dari MXNet.
: end_tab:

:begin_tab:`pytorch`
Sebagian besar kode dalam buku ini didasarkan pada PyTorch.
PyTorch adalah *framework* sumber terbuka untuk *deep learning* yang sangat
populer di komunitas peneliti.
Semua kode di buku ini telah lulus tes pada versi PyTorch terbaru.
Namun, karena perkembangan pesat dalam *deep learning*, beberapa kode
*dalam edisi cetak* mungkin tidak berfungsi dengan baik di versi PyTorch mendatang.
Namun, kami berencana untuk terus memutakhirkan edisi online buku ini.
Jika Anda mengalami masalah seperti itu,
silakan lihat :ref:`chap_installation`
untuk memutakhirkan kode dan *runtime environment* Anda.

Berikut adalah cara kami mengimpor modul dari PyTorch.
:end_tab:

:begin_tab:`tensorflow`
Sebagian besar kode dalam buku ini didasarkan pada TensorFlow.
TensorFlow adalah *framework* sumber terbuka untuk *deep learning* yang sangat
populer di komunitas peneliti dan dunia industri.
Semua kode di buku ini telah lulus tes pada versi TensorFlow terbaru.
Namun, karena perkembangan pesat dalam *deep learning*, beberapa kode
*dalam edisi cetak* mungkin tidak berfungsi dengan baik di versi TensorFlow mendatang.
Namun, kami berencana untuk terus memutakhirkan edisi online buku ini.
Jika Anda mengalami masalah seperti itu,
silakan lihat :ref:`chap_installation`
untuk memutakhirkan kode dan *runtime environment* Anda.

Berikut adalah cara kami mengimpor modul dari TensorFlow.
:end_tab:

```{.python .input}
#@save
from mxnet import autograd, context, gluon, image, init, np, npx
from mxnet.gluon import nn, rnn
```

```{.python .input}
#@tab pytorch
#@save
import numpy as np
import torch
import torchvision
from torch import nn
from torch.nn import functional as F
from torch.utils import data
from torchvision import transforms
from PIL import Image
```

```{.python .input}
#@tab tensorflow
#@save
import numpy as np
import tensorflow as tf
```

### Sasaran Pembaca

Buku ini ditujukan untuk mahasiswa (sarjana atau pascasarjana),
insinyur, dan peneliti, yang mencari pemahaman yang solid
akan teknik praktis *deep learning*.
Karena kami menjelaskan setiap konsep dari awal,
latar belakang *deep learning* atau *machine learning* sebelumnya tidak diperlukan.
Menjelaskan sepenuhnya metode *deep learning*
membutuhkan beberapa matematika dan pemrograman,
tetapi kami hanya akan berasumsi bahwa Anda menguasai beberapa pengetahuan dasar,
termasuk (dasar-dasar) aljabar linier, kalkulus, probabilitas,
dan pemrograman Python.
Selain itu, dalam Lampiran, kami memberikan penyegar
tentang sebagian besar matematika yang tercakup dalam buku ini.
Seringkali, kami akan lebih mengutamakan intuisi dan ide dari detail matematis.
Ada banyak buku bagus yang dapat membawa pembaca yang tertarik lebih jauh.
Misalnya, *Linear Analysis* oleh Bela Bollobas :cite:`Bollobas.1999`
membahas aljabar linier dan analisis fungsional secara mendalam.
"All of Statistics" :cite:`Wasserman.2013` adalah panduan statistik yang sangat bagus.
Dan jika Anda belum pernah menggunakan Python sebelumnya,
Anda mungkin ingin membaca [tutorial Python](http://learnpython.org/) ini.

### Forum

Bersama buku ini, kami meluncurkan forum diskusi, berlokasi di [diskusikan.d2l.ai](https://discuss.d2l.ai/).
Jika Anda memiliki pertanyaan tentang bagian mana pun dari buku ini,
Anda dapat menemukan tautan halaman diskusi terkait di akhir setiap bab.

### Sanwacana

Kami berhutang budi kepada ratusan kontributor untuk kedua edisi buku ini, bahasa Inggris dan bahasa China.
Mereka membantu meningkatkan konten dan menawarkan umpan balik yang berharga.
Secara khusus, kami berterima kasih kepada setiap kontributor draf bahasa Inggris ini
untuk menjadikannya lebih baik bagi semua orang.
ID atau nama GitHub mereka (tanpa urutan tertentu):
alxnorden, avinashingit, bowen0701, brettkoonce, Chaitanya Prakash Bapat,
cryptonaut, Davide Fiocco, edgarroman, gkutiel, John Mitro, Liang Pu,
Rahul Agarwal, Mohamed Ali Jamaoui, Michael (Stu) Stewart, Mike Müller,
NRauschmayr, Prakhar Srivastav, sad-, sfermigier, Sheng Zha, sundeepteki,
topecongiro, tpdi, vermicelli, Vishaal Kapoor, Vishwesh Ravi Shrimali, YaYaB, Yuhong Chen,
Evgeniy Smirnov, lgov, Simon Corston-Oliver, Igor Dzreyev, Ha Nguyen, pmuens,
Andrei Lukovenko, senorcinco, vfdev-5, dsweet, Mohammad Mahdi Rahimi, Abhishek Gupta,
uwsd, DomKM, Lisa Oakley, Bowen Li, Aarush Ahuja, Prasanth Buddareddygari, brianhendee,
mani2106, mtn, lkevinzc, caojilin, Lakshya, Fiete Lüer, Surbhi Vijayvargeeya,
Muhyun Kim, dennismalmgren, adursun, Anirudh Dagar, liqingnz, Pedro Larroy,
lgov, ati-ozgur, Jun Wu, Matthias Blume, Lin Yuan, geogunow, Josh Gardner,
Maximilian Böther, Rakib Islam, Leonard Lausen, Abhinav Upadhyay, rongruosong,
Steve Sedlmeyer, Ruslan Baratov, Rafael Schlatter, liusy182, Giannis Pappas,
ati-ozgur, qbaza, dchoi77, Adam Gerson, Phuc Le, Mark Atwood, christabella, vn09,
Haibin Lin, jjangga0214, RichyChen, noelo, hansent, Giel Dops, dvincent1337, WhiteD3vil,
Peter Kulits, codypenta, joseppinilla, ahmaurya, karolszk, heytitle, Peter Goetz, rigtorp,
Tiep Vu, sfilip, mlxd, Kale-ab Tessera, Sanjar Adilov, MatteoFerrara, hsneto,
Katarzyna Biesialska, Gregory Bruss, Duy–Thanh Doan, paulaurel, graytowne, Duc Pham,
sl7423, Jaedong Hwang, Yida Wang, cys4, clhm, Jean Kaddour, austinmw, trebeljahr, tbaums,
Cuong V. Nguyen, pavelkomarov, vzlamal, NotAnotherSystem, J-Arun-Mani, jancio, eldarkurtic,
the-great-shazbot, doctorcolossus, gducharme, cclauss, Daniel-Mietchen, hoonose, biagiom,
abhinavsp0730, jonathanhrandall, ysraell, Nodar Okroshiashvili, UgurKap, Jiyang Kang,
StevenJokes, Tomer Kaftan, liweiwp, netyster, ypandya, NishantTharani, heiligerl, SportsTHU,
Hoa Nguyen, manuel-arno-korfmann-webentwicklung, aterzis-personal, nxby, Xiaoting He, Josiah Yoder,
mathresearch, mzz2017, jroberayalas, iluu, ghejc, BSharmi, vkramdev, simonwardjones, LakshKD,
TalNeoran, djliden, Nikhil95, Oren Barkan, guoweis, haozhu233, pratikhack, 315930399, tayfununal,
steinsag, charleybeller, Andrew Lumsdaine, Jiekui Zhang, Deepak Pathak, Florian Donhauser, Tim Gates,
Adriaan Tijsseling, Ron Medina, Gaurav Saha, Murat Semerci, Lei Mao, Levi McClenny, Joshua Broyde.

Kami berterima kasih kepada Amazon Web Services, khususnya Swami Sivasubramanian,
Raju Gulabani, Charlie Bell, and Andrew Jassy atas dukungan mereka yang murah hati dalam menulis buku ini. Tanpa waktu yang tersedia, sumber daya, diskusi dengan rekan kerja, dan dorongan terus menerus, buku ini tidak akan terwujud.

## Ringkasan

* *Deep learning* telah merevolusi pengenalan pola, memperkenalkan teknologi yang sekarang memberdayakan berbagai teknologi, termasuk visi komputer, pemrosesan bahasa alami, pengenalan ucapan otomatis.
* Untuk berhasil menerapkan *deep learning*, Anda harus memahami cara memodelkan masalah, matematika pemodelan, algoritma untuk menyesuaikan model Anda dengan data, dan teknik rekayasa untuk mengimplementasikan semuanya.
* Buku ini menyajikan sumber daya yang lengkap, termasuk prosa, gambar, matematika, dan kode, semuanya di satu tempat.
* Untuk menjawab pertanyaan terkait buku ini, kunjungi forum kami di https://discuss.d2l.ai/.
* Semua *notebook* tersedia untuk diunduh di GitHub.

## Latihan

1. Daftarkan akun di forum diskusi buku ini [diskusikan.d2l.ai](https://discuss.d2l.ai/).
1. Instal Python di komputer Anda.
1. Ikuti tautan ke forum di bagian bawah, di mana Anda akan dapat mencari bantuan dan mendiskusikan buku dan menemukan jawaban atas pertanyaan Anda dengan melibatkan penulis dan komunitas yang lebih luas.

:begin_tab:`mxnet`
[Discussions](https://discuss.d2l.ai/t/18)
:end_tab:

:begin_tab:`pytorch`
[Discussions](https://discuss.d2l.ai/t/20)
:end_tab:

:begin_tab:`tensorflow`
[Discussions](https://discuss.d2l.ai/t/186)
:end_tab:
