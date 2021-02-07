# Pengantar
:label:`chap_introduction`

Hingga saat ini, hampir setiap program komputer yang berinteraksi dengan kita setiap hari diprogrem oleh pengembang perangkat lunak dari prinsip pertama. Misalnya kita ingin membuat aplikasi untuk mengelola platform *e-commerce*. Setelah berdiri di sekitar papan tulis selama beberapa jam untuk merenungkan masalah tersebut, kita akan menemukan garis besar solusi yang mungkin terlihat seperti ini: (i) pengguna berinteraksi dengan aplikasi melalui antarmuka yang berjalan di browser web atau aplikasi seluler; (ii) aplikasi kita berinteraksi dengan database kelas komersial untuk melacak setiap status pengguna dan menyimpan catatan transaksi historis; dan (iii) inti dari aplikasi kita, logika bisnis (bisa dikatakan, otak) aplikasi kita menjabarkan secara metodis tindakan yang tepat yang harus dilakukan oleh program kita dalam setiap kemungkinan situasi.

Untuk membangun otak aplikasi kita,
kita harus memikirkan setiap kasus yang kita pikir memungkinkan untuk membuat aturan yang sesuai.
Setiap kali pelanggan mengklik untuk menambahkan barang ke keranjang belanja mereka,
kita menambahkan data ke tabel database keranjang belanja,
mengaitkan ID pengguna tersebut dengan ID produk yang diminta.
Sementara beberapa pengembang pernah melakukannya dengan benar pada kali pertama
(mungkin perlu beberapa uji coba untuk mengatasi masalah),
Pada umumnya, kita dapat menulis program seperti itu dari prinsip-prinsip pertama
dan meluncurkannya dengan percaya diri
bahkan *sebelum* kita melihat pelanggan sungguhan.
Kemampuan kita untuk merancang sistem otomatis dari prinsip pertama
yang menjalankan produk dan sistem,
sering dalam situasi baru,
adalah prestasi kognitif yang luar biasa.
Dan saat Anda dapat menemukan solusi yang berhasil $100\%$ setiap saat,
Anda tidak perlu menggunakan *machine learning*.

Untungnya bagi komunitas ilmuwan *machine learning* yang terus bertambah, banyak tugas yang ingin kami otomatisasi namun tidak mudah ditundukkan dengan kecerdikan manusia. Bayangkan berdiri di sekitar papan tulis dengan orang terpintar yang Anda ketahui, tetapi kali ini Anda mencoba menyelesaikan salah satu masalah berikut:

* Membuat program yang meramal cuaca besok berdasarkan informasi geografis, citra satelit, dan jendela jejak cuaca masa lalu.
* Membuat program yang memuat pertanyaan, dalam bentuk teks bebas, dan dapat menjawabnya dengan benar.
* Membuat program yang ketika diberi gambar dapat mengenali semua orang di dalam gambar, dan menggambar *outline* di sekelilingnya.
* Membuat program yang menyarankan produk yang kemungkinan besar akan disukai pengguna, namun jarang ditemui secara alami ketika *browsing* di web.

Dalam setiap contoh masalah ini, bahkan programmer elit
tidak mampu membuat kode dari nol.
Alasannya bisa bermacam-macam. Terkadang program
yang dikerjakan mengikuti pola yang terus berubah seiring waktu,
dan kita membutuhkan program yang adapat beradaptasi.
Dalam kasus lain, relasi (misalnya antara piksel,
dan kategori abstrak) mungkin terlalu rumit,
membutuhkan ribuan atau jutaan perhitungan
yang berada di luar pemahaman kita 
walaupun mata kita bisa mengerjakan tugas itu dengan mudah.
*machine learning* adalah studi tentang teknik efektif yang mampu belajar dari pengalaman.
Saat algoritma *machine learning* mengumpulkan lebih banyak pengalaman,
biasanya dalam bentuk data observasi atau
interaksi dengan lingkungan, kinerjanya akan meningkat.
Bandingkan ini dengan platform e-commerce deterministik di awal tadi,
yang bekerja sesuai dengan logika bisnis yang tetap sama,
tidak peduli berapa banyak pengalaman yang diperoleh,
sampai pemrogram belajar dan memutuskan sendiri
bahwa sudah waktunya untuk memutakhirkan perangkat lunaknya.
Dalam buku ini, kami akan mengajari Anda dasar-dasar *machine learning*,
dan fokus khususnya pada *deep learning*,
kumpulan teknik yang kuat yang 
mendorong inovasi di berbagai bidang seperti visi komputer,
pemrosesan bahasa alami, perawatan kesehatan, dan genomik.


## Contoh yang Memotivasi

Sebelum mulai menulis, penulis buku ini,
seperti banyak tenaga kerja, harus berkafein.
Kami melompat ke dalam mobil dan mulai mengemudi.
Menggunakan iPhone, Alex memanggil "Hai Siri",
membangunkan sistem pengenalan suara telepon.
Kemudian Mu memerintahkan "arah ke kedai kopi Blue Bottle".
Telepon dengan cepat menampilkan transkripsi perintahnya.
Ia juga menyadari bahwa kami menanyakan arah
dan meluncurkan aplikasi Maps (aplikasi)
untuk memenuhi permintaan kami.
Setelah diluncurkan, aplikasi Maps mengidentifikasi sejumlah rute.
Di samping setiap rute, ponsel menampilkan perkiraan waktu transit.
Walaupun kami mengarang cerita ini untuk ilustrasi pedagogis,
cerita ini menunjukkan bahwa hanya dalam beberapa detik,
interaksi sehari-hari kita dengan ponsel pintar
dapat melibatkan beberapa model *machine learning*. 

Bayangkan membuat program yang menjawab kata panggilan
seperti "Alexa", "Ok Google", dan "Hai Siri".
Coba buat kode sendiri di ruangan
hanya dengan komputer dan editor kode,
seperti yang diilustrasikan dalam :numref:`fig_wake_word`.
Bagaimana Anda akan menulis program seperti itu dari prinsip pertama?
Pikirkanlah ... masalah ini sulit.
Setiap detik, mikrofon akan mengoleksi sekitar 
44000 sampel.
Setiap sampel merupakan pengukuran amplitudo gelombang suara.
Aturan apa yang dapat dipetakan dengan andal dari cuplikan audio mentah ke prediksi yang meyakinkan
$\{\text{yes}, \text{no}\}$
apakah cuplikan tersebut berisi kata panggilan?
Jika Anda bingung, jangan khawatir.
Kami juga tidak tahu bagaimana menulis program seperti itu dari nol.
Itulah mengapa kami menggunakan *machine learning*.

![Identify a wake word.](../img/wake-word.svg)
:label:`fig_wake_word`

Inilah triknya.
Seringkali, bahkan ketika kita tidak tahu bagaimana cara memberitahu komputer
secara eksplisit bagaimana memetakan dari input ke output,
kita tetap mampu melakukan prestasi kognitif itu sendiri.
Dengan kata lain, meskipun Anda tidak tahu
bagaimana memprogram komputer untuk mengenali kata "Alexa",
Anda sendiri bisa mengenalinya.
Berbekal kemampuan ini, kita dapat mengumpulkan *dataset* yang sangat besar
berisi contoh-contoh audio
dan beri label pada contoh audio tersebut yang mengandung kata panggilan atau tidak mengandung kata panggilan.
Dalam pendekatan *machine learning*,
kita tidak mencoba merancang sistem
*secara eksplisit* untuk mengenali kata panggilan.
Sebaliknya, kita mendefinisikan program fleksibel
yang perilakunya ditentukan oleh sejumlah *parameter*.
Kemudian kita menggunakan *dataset* untuk menentukan kumpulan parameter terbaik,
yang meningkatkan kinerja program kita menurut beberapa ukuran kinerja yang relevan.

Anda dapat menganggap parameter sebagai kenop yang dapat kita putar untuk 
memanipulasi perilaku program.
Bersama dengan parameter yang sudah ditentukan, kami menyebut program ini sebagai *model*.
Himpunan semua program yang berbeda (pemetaan input ke output)
yang dapat kami hasilkan hanya dengan memanipulasi parameter
disebut *keluarga* model.
Dan program meta yang menggunakan dataset kita 
untuk memilih parameter terbaik disebut algoritma *learning*.

Sebelum kita dapat melanjutkan dan menggunakan algoritma *learning*,
kita harus mendefinisikan masalahnya dengan tepat,
menjabarkan sifat input dan output yang tepat,
dan memilih keluarga model yang sesuai.
Pada kasus ini,
model kita menerima potongan audio sebagai *input*,
dan modelnya
menghasilkan pilihan di antara
$\{\text{yes}, \text{no}\}$ as *output*.
Jika semua berjalan sesuai rencana
tebakan model 
biasanya akan benar untuk
menjawab pertanyaan apakah cuplikan audio tersebut berisi kata panggilan.

Jika kita memilih keluarga model yang tepat,
seharusnya akan ada pengaturan kenop
sedemikian rupa sehingga model menjawab "ya" setiap kali mendengar kata "Alexa".
Karena pilihan dari kata panggilan itu bebas,
kita mungkin membutuhkan keluarga model yang cukup fleksibel, sehingga 
melalui pengaturan kenop yang lain, model itu bisa menjawab "ya"
hanya setelah mendengar kata "Aprikot".
Kami berharap keluarga model yang sama harus cocok
untuk pengenalan kata "Alexa" dan pengenalan kata "Aprikot"
karena mereka tampaknya, secara intuitif, adalah tugas yang mirip.
Namun, kita mungkin membutuhkan keluarga model yang sama sekali berbeda
jika kita ingin berurusan dengan input atau output yang berbeda secara mendasar,
katakanlah jika kita ingin memetakan dari gambar ke teks,
atau dari kalimat bahasa Inggris ke kalimat bahasa Mandarin.

Seperti yang bisa Anda duga, jika kita mengatur semua kenop secara acak,
kecil kemungkinan model kita akan mampu mengenali kata "Alexa",
"Apricot", atau kata bahasa Inggris lainnya.
Dalam *machine learning*,
*learning* adalah proses untuk menemukan pengaturan kenop yang tepat agar 
menghasilkan perilaku yang diinginkan dari model kita.
Dengan kata lain, kita *melatih* model kita dengan data.
Seperti yang ditunjukkan di :numref:`fig_ml_loop`, proses pelatihan biasanya terlihat seperti berikut:

1. Mulailah dengan model yang diinisialisasi secara acak yang tidak dapat melakukan sesuatu yang berguna.
1. Ambil beberapa data Anda (mis., Cuplikan audio dan label $\{\text{yes}, \text{no}\}$ yang sesuai).
1. Sesuaikan kenop sehingga model menjadi lebih baik terhadap contoh-contoh tersebut.
1. Ulangi Langkah 2 dan 3 sampai modelnya sangat baik.

![Proses pelatihan.](../img/ml-loop.svg)
:label:`fig_ml_loop`

Ringkasnya, daripada membuat kode pengenal kata panggilan,
kita membuat kode program yang dapat *belajar* untuk mengenali kata-kata panggilan 
jika kita berikan *dataset* besar berlabel.
Anda dapat menyebut cara ini, untuk menentukan perilaku program
dengan memberikan dengan *dataset* sebagai pemrograman dengan data.
Artinya,
kita bisa "memprogram" detektor kucing dengan memberikan banyak contoh kucing dan anjing ke sistem *machine learning* kita.
Dengan cara ini detektor pada akhirnya akan belajar mengeluarkan bilangan positif yang sangat besar jika itu adalah kucing, bilangan negatif yang sangat besar jika itu adalah seekor anjing,
dan mendekati nol jika tidak yakin,
dan contoh ini hanya sebagian kecil saja dari apa yang bisa dilakukan *machine learning*.
*Deep learning*,
yang akan kami jelaskan lebih detail nanti,
hanyalah satu di antara banyak metode populer
untuk memecahkan masalah *machine learning*.

## Komponen-komponen kunci

Dalam contoh kata panggilan tadi, kami menjelaskan kumpulan data
terdiri dari potongan audio dan label biner,
dan kami
menjelaskan secara garis besar tentang bagaimana kita bisa melatih
model untuk memperkirakan pemetaan dari cuplikan audio hingga klasifikasi.
Masalah seperti ini, memprediksi label yang tidak diketahui 
berdasarkan suatu masukan, 
berdasarkan *dataset* yang terdiri dari contoh
yang labelnya diketahui,
disebut *supervised* *machine learning*.
Ini hanyalah salah satu dari banyak jenis masalah *machine learning*.
Nanti kita akan mempelajari berbagai masalah *machine learning*.
Pertama, kami ingin menjelaskan lebih banyak tentang beberapa komponen inti
yang akan mengikuti kita, apa pun masalah *machine learning* yang kita hadapi:

1. *Data* yang dapat kita pelajari.
1. *Model* tentang bagaimana mentransformasikan data.
1. Fungsi objektif yang mengukur seberapa baik (atau buruk) kinerja sebuah model.
1. Algoritma untuk menyesuaikan parameter model untuk mengoptimalkan fungsi objektif.

### Data

Mungkin sudah jelas bahwa Anda tidak dapat melakukan pekerjaan ilmu data tanpa data.
Kami bisa kehilangan ratusan halaman memikirkan apa sebenarnya yang dimaksud dengan data,
tapi untuk saat ini, kita akan mengutamakan pada sisi praktisnya
dan fokus pada properti utama yang akan diperhatikan.
Secara umum, kita berbicara mengenai kumpulan contoh.
Untuk bekerja dengan data,
kita biasanya
perlu memikirkan representasi numerik yang sesuai dari data tersebut.
Setiap *contoh* (atau *titik data*, *contoh data*) biasanya terdiri dari satu set
atribut yang disebut *fitur* (atau *kovariat*),
yang darinya model harus membuat prediksi.
Dalam masalah *supervised learning* di atas,
hal yang akan diprediksi
adalah atribut khusus
yang ditetapkan sebagai
*label* (atau *target*).

Jika kita bekerja dengan data gambar,
setiap foto individu dapat menjadi contoh,
masing-masing diwakili oleh daftar nilai numerik yang diurutkan
sesuai dengan kecerahan setiap piksel.
Foto berwarna ukuran $200\times 200$ terdiri dari $200\times200\times3=120000$ nilai numerik
sesuai dengan tingkat kecerahan warna merah, hijau dan biru dari setiap lokasi spasial.
Dalam tugas tradisional lainnya, kita mungkin mencoba memprediksi
apakah pasien akan selamat atau tidak,
ketika diberi satu set fitur standar seperti
usia, tanda-tanda vital, dan diagnosis.

Ketika setiap contoh dicirikan oleh nilai numerik dalam jumlah yang sama,
kita katakan bahwa data terdiri dari vektor dengan panjang tetap
dan kami menjelaskan panjang konstan vektor
sebagai *dimensionalitas* data.
Seperti yang Anda bisa duga, vektor dengan panjang tetap bisa menjadi properti yang memudahkan.
Jika kita ingin melatih model untuk mengenali kanker dalam gambar mikroskop,
input dengan panjang tetap berarti kita tidak harus menghawatirkan hal ini.

Namun, tidak semua data dapat dengan mudah direpresentasikan sebagai
vektor dengan panjang tetap.
Meskipun kita mungkin mengharapkan gambar mikroskop berasal dari peralatan standar,
kita tidak dapat mengharapkan semua gambar yang diambil dari Internet
memiliki resolusi atau bentuk yang sama.
Untuk gambar, kita mungkin bisa memangkas semuanya ke ukuran standar,
tapi strategi itu hanya membantu kita sejauh ini.
Cara ini berisiko kehilangan informasi dalam bagian yang dipotong.
Selain itu, data berupa teks lebih jarang lagi untuk dijadikan dalam panjang yang tetap.
Perhatikan ulasan pelanggan yang ditinggalkan di situs *e-commerce*
seperti Amazon, IMDB, dan TripAdvisor.
Beberapa ada yang pendek: "bau!".
Beberapa yang lain menulis dalam banyak halaman.
Salah satu keuntungan utama dari *deep learning* dibandingkan metode tradisional
adalah kemampuannya dalam menangani data dengan panjang bervariasi.

Umumnya, semakin banyak data yang kita miliki, semakin mudah pekerjaan kita.
Saat kita memiliki lebih banyak data, kita dapat melatih model yang lebih canggih
dan tidak terlalu mengandalkan asumsi-asumsi dari model.
Perubahan rezim dari (secara komparatif) data kecil menjadi data besar (*big data*)
adalah kontributor utama bagi keberhasilan *deep learning* modern.
Poinnya adalah, banyak model paling menarik dalam *deep learning* tidak akan berfungsi tanpa *dataset* yang besar.
Beberapa model *deep learning* lainnya ada yang bisa berfungsi dalam rezim data kecil,
tetapi tidak lebih baik dari pendekatan tradisional.

Terakhir, tidak cukup hanya memiliki banyak data dan memprosesnya dengan cerdik.
Kami membutuhkan data *yang benar*.
Jika datanya penuh dengan kesalahan,
atau jika fitur yang dipilih tidak berguna untuk prediksi target yang dituju,
maka proses *learning* tidak akan berhasil.
Situasi ini digambarkan dengan baik oleh: *garbage in, garbage out*.
Selain itu, kinerja prediksi yang buruk bukan satu-satunya masalah yang akan timbul.
Dalam penerapan *machine learning* yang sensitif,
seperti kepolisian prediktif, penyaringan resume kandidat, dan model risiko yang digunakan untuk peminjaman,
kita harus sangat waspada terhadap konsekuensi data sampah.
Satu kesalahan yang umum terjadi di *dataset* di mana beberapa kelompok orang
tidak terwakili dalam data pelatihan.
Bayangkan menerapkan sistem pengenalan kanker kulit di alam liar
yang belum pernah melihat kulit hitam sebelumnya.
Kegagalan juga bisa terjadi saat data
tidak hanya kurang merepresentasikan beberapa kelompok
tetapi juga mencerminkan prasangka sosial.
Sebagai contoh,
jika keputusan perekrutan di masa lalu digunakan untuk melatih model prediktif
yang akan digunakan untuk menyaring resume kandidat,
maka model *machine learning* bisa secara tidak sengaja
memasukkan dan mengotomatiskan ketidakadilan historis.
Perhatikan bahwa ini semua dapat terjadi tanpa sang ilmuwan data secara aktif 
berniat melakukan itu, atau bahkan tanpa disadarinya.

