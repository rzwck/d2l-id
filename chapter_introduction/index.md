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

### Model

Sebagian besar *machine learning* melibatkan suatu transformasi data dalam arti tertentu.
Kita mungkin ingin membangun sistem yang menerima input foto dan memprediksi kadar senyuman.
Dalam hal lain, kita mungkin ingin menrima input satu set pembacaan sensor
dan memprediksi seberapa normal vs anomali pembacaan tersebut.
Dengan menyebut model, yang kami maksudkan adalah mesin komputasi untuk menyerap data
dari satu jenis,
dan mengeluarkan prediksi dari jenis yang mungkin berbeda.
Secara khusus, kita tertarik pada model statistik
yang dapat diperkirakan dari data.
Walaupun model sederhana sangat mampu menangani
masalah yang cukup sederhana,
masalah
yang kami fokuskan dalam buku ini di luar kapasitas metode klasik.
*Deep learning* dibedakan dari pendekatan klasik
terutama oleh serangkaian model yang kuat yang menjadi fokusnya.
Model ini terdiri dari banyak transformasi data yang berurutan
yang dirangkai dari atas ke bawah, itulah kenapa dinamakan *deep learning*.
Dalam perjalanan kita untuk mendiskusikan *deep model*,
kami juga akan membahas beberapa metode tradisional.

### Fungsi Objektif

Di awal, kami memperkenalkan *machine learning* sebagai pembelajaran dari pengalaman.
Dengan menyebut *learning* di sini, yang kami maksud adalah meningkatkan beberapa tugas seiring waktu.
Tetapi siapa yang bisa mengatakan bahwa itu adalah kemajuan atau perbaikan?
Anda mungkin membayangkan bahwa kita dapat mengusulkan untuk memperbarui model kita,
dan beberapa orang mungkin tidak setuju bahwa usulan perbaruan itu adalah kemajuan atau kemunduran.

Untuk mengembangkan sistem matematika formal dari mesin pembelajaran,
kita perlu memiliki ukuran formal tentang seberapa baik (atau buruk) model kita.
Dalam pembelajaran mesin, dan pengoptimalan secara umum,
kami menyebutnya *fungsi objektif*.
Secara konvensi, kami biasanya mendefinisikan fungsi objektif sehingga nilai yang lebih rendah adalah lebih baik.
Ini hanyalah konvensi.
Anda dapat mengambil fungsi apa pun
yang nilainya lebih tinggi adalah yang lebih baik, dan mengubahnya menjadi fungsi baru
yang identik secara kualitatif tetapi nilai yang lebih rendah adalah lebih baik
dengan cara membalik tanda positif dan negatifnya.
Karena lebih rendah lebih baik, fungsi-fungsi ini terkadang disebut
*fungsi kerugian*.

Saat mencoba memprediksi nilai numerik,
fungsi kerugian yang paling umum adalah *kesalahan kuadrat*,
yaitu, kuadrat dari perbedaan antara prediksi dan nilai yang sebenarnya.
Untuk klasifikasi, fungsi objektif paling umum adalah meminimalkan tingkat kesalahan,
yaitu, bagian dari contoh yang
prediksinya tidak sesuai dengan nilai sebenarnya.
Beberapa objektif (misalnya, kesalahan kuadrat) mudah untuk dioptimalkan.
Lainnya (misalnya, tingkat kesalahan) sulit untuk dioptimalkan secara langsung,
karena tidak dapat dibedakan atau komplikasi lainnya.
Dalam kasus ini, biasanya mengoptimalkan fungsi objektif *surrogate*.

Biasanya, fungsi kerugian ditentukan
sehubungan dengan parameter model
dan bergantung pada *dataset*.
Kita belajar
nilai terbaik dari parameter model kita
dengan meminimalkan kerugian yang terjadi pada satu set
terdiri dari sejumlah contoh yang dikumpulkan untuk pelatihan.
Namun, melakukannya dengan baik pada data pelatihan
tidak menjamin bahwa model kita akan bekerja dengan baik pada data yang baru.
Jadi kita biasanya ingin membagi data yang tersedia menjadi dua partisi:
*dataset* pelatihan (atau *set* pelatihan, untuk menyesuaikan parameter model)
dan *dataset* pengujian (atau *set* pengujian, yang diadakan untuk evaluasi),
dan kita akan menghitung kinerja model pada keduanya.
Anda bisa membayangkan performa latihan seperti itu
nilai siswa pada latihan ujian 
digunakan untuk mempersiapkan beberapa ujian akhir yang sebenarnya.
Meskipun hasil latihan ujian bagus,
itu tidak menjamin kesuksesan pada ujian akhir.
Dengan kata lain,
kinerja tes dapat menyimpang secara signifikan dari kinerja pelatihan.
Saat model berperforma baik di set pelatihan
tetapi gagal menggeneralisasi ke data yang baru,
kita mengatakan itu sebagai *overfitting*.
Dalam istilah kehidupan nyata, ini seperti gagal dalam ujian yang sebenarnya
meski mengerjakan latihan ujian dengan baik.

### Algoritma Optimasi

Setelah kita mendapatkan beberapa sumber data dan representasi,
model, dan fungsi objektif yang terdefinisi dengan baik,
kita membutuhkan algoritma yang mampu melakukan pencarian
untuk mendapatkan parameter terbaik untuk meminimalkan fungsi kerugian.
Algoritma optimasi populer untuk *deep learning*
didasarkan pada pendekatan yang disebut *gradient descent*.
Singkatnya, di setiap langkah, metode ini
akan mencari, untuk setiap parameter,
ke arah mana kerugian set pelatihan akan bergerak
jika Anda mengubah parameter itu sedikit saja.
Kemudian dia akan memperbarui
parameter ke arah yang dapat mengurangi kerugian.

## Jenis Masalah *Machine Learning*

Masalah kata panggilan dalam contoh motivasi di awal 
hanyalah satu di antara
banyak masalah yang dapat ditangani oleh *machine learning*.
Untuk memotivasi pembaca lebih lanjut
dan memberi kita beberapa bahasa umum saat kita membicarakan lebih banyak masalah di seluruh buku,
berikut kami
buat daftar contoh masalah *machine learning*.
Kami akan terus merujuk
konsep kami yang disebutkan di atas
seperti data, model, dan teknik pelatihan.

### Pembelajaran Terbimbing *Supervised Learning*

*Pembelajaran terbimbing* membahas tugas
memprediksi label dari masukan berupa fitur.
Setiap pasangan fitur - label disebut sebagai contoh.
Terkadang, jika konteksnya jelas, kami mungkin menggunakan istilah *contoh*
untuk merujuk pada kumpulan masukan,
bahkan ketika label terkait tidak diketahui.
Tujuan kita adalah menghasilkan model
yang memetakan input apa pun ke prediksi label.

Untuk mendasari deskripsi ini dalam contoh konkret,
jika kita bekerja di bidang kesehatan,
maka kita mungkin ingin memprediksi apakah atau tidak
seorang pasien akan mengalami serangan jantung.
Pengamatan ini, "serangan jantung" atau "tidak ada serangan jantung",
akan menjadi label kita.
Fitur masukan mungkin merupakan tanda vital
seperti detak jantung, tekanan darah diastolik,
dan tekanan darah sistolik.

Pengawasan berperan karena untuk memilih parameter, kita (sebagai pengawas) menyediakan model dengan *dataset*
terdiri dari contoh berlabel,
di mana setiap contoh dicocokkan dengan label yang benar.
Dalam istilah probabilistik, kami biasanya tertarik untuk memperkirakan
probabilitas bersyarat dari label yang diberi fitur masukan.
Meskipun ini hanyalah salah satu dari beberapa paradigma dalam pembelajaran mesin,
pembelajaran yang diawasi termasuk sebagai mayoritas dalam hal kesuksesan penerapan pembelajaran mesin di industri.
Sebagian karena banyak tugas penting
dapat digambarkan secara jelas sebagai memperkirakan probabilitas
dari sesuatu yang tidak diketahui berdasarkan kumpulan data tertentu yang tersedia:

* Memprediksi kanker vs. bukan kanker berdasarkan masukan gambar tomografi komputer.
* Memprediksi terjemahan yang benar dalam bahasa Prancis berdasarkan masukan kalimat dalam bahasa Inggris.
* Memprediksi harga suatu saham bulan depan berdasarkan data pelaporan keuangan bulan ini.

Bahkan dengan deskripsi yang sederhana
"memprediksi label berdasarkan fitur masukan"
pembelajaran yang diawasi dapat berupa banyak bentuk
dan membutuhkan banyak keputusan pemodelan,
tergantung pada (di antara pertimbangan lain) jenis, ukuran,
dan jumlah input dan output.
Misalnya, kita menggunakan model yang berbeda untuk memproses urutan masukan dengan panjang bervariasi 
dan untuk memproses representasi vektor dengan panjang tetap.
Kita akan membahas banyak dari masalah ini secara mendalam
sepanjang buku ini.

Secara informal, proses pembelajaran terlihat seperti berikut ini.
Pertama, ambil banyak koleksi contoh yang fitur-fiturnya diketahui
dan pilih sebagian di antaranya secara acak,
dapatkan label yang sebenarnya untuk masing-masing contoh tersebut.
Terkadang label ini mungkin data yang telah dikumpulkan
(misalnya, apakah pasien meninggal dalam tahun berikutnya?)
dan di lain waktu kita mungkin perlu menggunakan usaha manusia untuk memberi label pada data,
(mis., menetapkan gambar ke kategori).
Bersama-sama, masukan dan label yang sesuai membentuk suatu set pelatihan.
Kita kemudian memasukkan set data pelatihan ke dalam algoritma pembelajaran yang diawasi,
sebuah fungsi yang mengambil masukan dari sebuah dataset
dan menghasilkan fungsi lain: yaitu model yang dipelajari dari data.
Akhirnya, kita dapat memasukkan masukan yang sebelumnya tidak pernah dilihat ke model yang dipelajari,
menggunakan keluarannya sebagai prediksi dari label yang sesuai.
Proses lengkapnya diambil dalam :numref:`fig_supervised_learning`.

#### Regresi

Mungkin tugas belajar terbimbing yang paling sederhana
dan mudah untuk dipahami *regresi*.
Pertimbangkan, misalnya, sekumpulan data yang diambil
dari basis data penjualan rumah.
Kita mungkin membuat tabel,
di mana setiap baris sesuai dengan rumah yang berbeda,
dan setiap kolom sesuai dengan beberapa atribut yang relevan,
seperti ukuran luas sebuah rumah,
jumlah kamar tidur, jumlah kamar mandi, dan jumlah menit (berjalan kaki) ke pusat kota.
Dalam kumpulan data ini, setiap contoh adalah suatu rumah tertentu,
dan vektor fitur yang sesuai akan menjadi satu baris dalam tabel.
Jika Anda tinggal di New York atau San Francisco,
dan Anda bukan CEO Amazon, Google, Microsoft, atau Facebook,
(luas, jumlah kamar tidur, jumlah kamar mandi, jarak berjalan kaki)
vektor fitur untuk rumah Anda mungkin terlihat seperti ini: $[600, 1, 1, 60]$.
Namun, jika Anda tinggal di Pittsburgh, mungkin terlihat seperti $[3000, 4, 3, 10]$.
Vektor fitur seperti ini sangat penting
untuk sebagian besar algoritma pembelajaran mesin klasik.

Apa yang membuat suatu masalah menjadi tergolong masalah regresi sebenarnya adalah keluarannya.
Katakanlah Anda sedang mencari rumah baru.
Anda mungkin ingin memperkirakan nilai pasar wajar sebuah rumah,
berdasarkan masukan beberapa fitur seperti di atas.
Label, harga jual, adalah nilai numerik.
Ketika label mengambil nilai numerik bebas,
kita menyebutnya sebagai masalah *regresi*.
Tujuan kita adalah menghasilkan model yang prediksinya 
mendekati nilai label sebenarnya.

Banyak masalah praktis merupakan masalah regresi yang dijelaskan dengan baik.
Memprediksi rating yang akan diberikan pengguna untuk sebuah film
dapat dianggap sebagai masalah regresi
dan jika Anda merancang algoritma hebat untuk mencapai prestasi ini di tahun 2009,
Anda mungkin telah memenangkan [hadiah Netflix senilai 1 juta dolar](https://en.wikipedia.org/wiki/Netflix_Prize).
Memprediksi lamanya pasien dirawat di rumah sakit
juga merupakan masalah regresi.
Aturan praktis yang baik untuk mengenali regresi adalah masalah yang menjawab *berapa?* sebaiknya menggunakan regresi,
seperti:

* Berapa jam operasi ini berlangsung?
* Berapa curah hujan yang akan terjadi di kota ini dalam enam jam ke depan?

Meskipun mungkin Anda belum pernah menggunakan pembelajaran mesin sebelumnya,
Anda mungkin telah menyelesaikan masalah regresi secara informal.
Bayangkan, misalnya, saluran air Anda diperbaiki
dan kontraktor Anda menghabiskan waktu 3 jam
menghilangkan kotoran dari pipa limbah Anda.
Kemudian dia mengirimi Anda tagihan sebesar 350 dolar.
Sekarang bayangkan teman Anda menyewa kontraktor yang sama selama 2 jam
dan bahwa dia menerima tagihan 250 dolar.
Jika seseorang kemudian bertanya kepada Anda berapa banyak tagihan 
untuk penghapusan kotoran mereka yang akan datang
Anda mungkin membuat beberapa asumsi yang masuk akal,
seperti lebih banyak jam kerja membutuhkan lebih banyak dolar.
Anda mungkin juga berasumsi bahwa ada beberapa tagihan dasar
dan kemudian kontraktor menagih per jam di luar itu.
Jika asumsi ini benar, maka jika diberikan dua contoh data ini,
Anda sudah dapat mengidentifikasi struktur harga kontraktor:
100 dolar per jam ditambah 50 dolar untuk muncul di rumah Anda.
Jika Anda mengerti sampai di sini maka Anda sudah mengerti
ide garis besar di balik regresi linier.

Dalam hal ini, kita dapat menghitung parameter model 
yang sama persis dengan harga kontraktor.
Terkadang hal ini tidak memungkinkan,
mis., jika beberapa dari
variansnya disebabkan oleh beberapa faktor
selain kedua fitur Anda.
Dalam kasus ini, kami akan mencoba mempelajari model
yang meminimalkan jarak antara prediksi kita dan nilai yang diamati.
Di sebagian besar bab, kita akan fokus pada
meminimalkan fungsi kerugian kesalahan kuadrat.
Seperti yang akan kita lihat nanti, kerugian ini sesuai dengan asumsi
bahwa data telah dirusak dengan gangguan Gaussian.

#### Klasifikasi

Meskipun model regresi bagus untuk menjawab pertanyaan *berapa?*,
banyak masalah yang tidak cocok dengan pola ini.
Sebagai contoh,
bank ingin menambahkan pemindaian cek ke aplikasi selulernya.
Ini akan melibatkan pelanggan yang mengambil foto cek
dengan kamera ponsel pintar mereka
dan aplikasi tersebut harus mampu
untuk secara otomatis memahami teks yang terlihat pada gambar.
Secara khusus,
aplikasi itu juga harus bisa memahami teks tulisan tangan agar lebih aman,
seperti memetakan karakter tulisan tangan
ke salah satu karakter yang dikenal.
Jenis masalah *yang mana?* ini disebut *klasifikasi*.
Masalah ini diselesaikan dengan sekumpulan algoritma yang berbeda
daripada yang digunakan untuk regresi meskipun banyak teknik akan sama. 

Dalam *klasifikasi*, kita ingin model kita melihat fitur,
mis., nilai piksel dalam gambar,
dan kemudian memprediksi *kategori* mana (secara formal disebut *kelas*),
Di antara beberapa pilihan yang berbeda, ada contohnya.
Untuk angka tulisan tangan, kita mungkin memiliki sepuluh kelas,
sesuai dengan angka 0 sampai 9.
Bentuk klasifikasi yang paling sederhana adalah jika hanya ada dua kelas,
masalah yang kami sebut *klasifikasi biner*.
Misalnya, kumpulan data kami dapat terdiri dari gambar hewan
dan label kita mungkin kelas $\mathrm{\{kucing, anjing\}}$.
Saat dalam regresi, kita mencari regressor untuk menghasilkan nilai numerik,
dalam klasifikasi, kami mencari pengklasifikasi, yang keluarannya adalah kelas yang diprediksi.

Untuk alasan yang akan kita bahas nanti di bagian yang lebih teknis,
mungkin sulit untuk mengoptimalkan model yang hanya dapat menghasilkan
tugas pengklasifikasian kategoris keras,
mis., "kucing" atau "anjing".
Dalam kasus ini, biasanya lebih mudah untuk diungkapkan
model kita dalam bahasa probabilitas.
Diberikan fitur contoh,
model kita memberikan probabilitas
untuk setiap kelas yang memungkinkan.
Kembali ke contoh klasifikasi hewan kita
dengan kelas $\mathrm{\{kucing, anjing\}}$,
pengklasifikasi mungkin melihat gambar dan mengeluarkan probabilitas
bahwa gambar kucing sebagai 0,9.
Kita dapat menafsirkan angka ini dengan mengatakan bahwa pengklasifikasi
90\% yakin bahwa gambar tersebut menggambarkan seekor kucing.
Besarnya probabilitas untuk kelas yang diprediksi
menyampaikan satu konsep tentang ketidakpastian.
Ini bukan satu-satunya konsep tentang ketidakpastian
dan kita akan membahas konsep lain di bab-bab mendatang.

Jika kita memiliki lebih dari dua kelas yang memungkinkan,
kita menyebutnya sebagai masalah *klasifikasi multikelas*.
Contoh umum termasuk pengenalan karakter tulisan tangan
$\mathrm{\{0, 1, 2, ... 9, a, b, c, ...\}}$.
Sementara kita menyelesaikan masalah regresi dengan mencoba
untuk meminimalkan fungsi kerugian kesalahan kuadrat,
fungsi kerugian umum untuk masalah klasifikasi disebut *entropi-silang*,
yang namanya akan dijelaskan
melalui pengantar teori informasi di bab-bab selanjutnya.

Perhatikan bahwa kelas yang paling mungkin belum tentu menjadi 
salah satu yang akan Anda gunakan dalam keputusan Anda.
Bayangkan bahwa Anda menemukan jamur yang indah di halaman belakang rumah Anda
seperti yang ditunjukkan di :numref:`fig_death_cap`.

![Death cap---jangan dimakan!](../img/death-cap.jpg)
:width:`200px`
:label:`fig_death_cap`

Sekarang, asumsikan bahwa Anda membuat pengklasifikasi dan melatihnya
untuk memprediksi apakah suatu jamur beracun berdasarkan foto.
Katakanlah pengklasifikasi pendeteksi racun kita menghasilkan keluaran 
bahwa kemungkinan foto 
:numref:`fig_death_cap` mengandung jamur *death cap* sebesar 0.2.
Dengan kata lain, pengklasifikasi 80\% yakin
bahwa jamur kita bukanlah jamur *death cap*.
Meskipun begitu tetap saja, hanya orang bodoh yang akan memakannya.
Hal itu karena manfaat pasti dari makan malam yang enak
tidak sebanding dengan risiko 20% mati karenanya.
Dengan kata lain, risiko efek dari ketidakpastian jauh
melebihi manfaatnya.
Jadi, kita perlu menghitung risiko yang diharapkan yang kita tanggung sebagai fungsi kerugian,
yaitu, kita perlu mengalikan probabilitas hasilnya
dengan manfaat (atau bahaya) yang terkait dengannya.
Pada kasus ini,
kerugian yang timbul karena memakan jamur
adalah $0.2 \times \infty + 0.8 \times 0 = \infty$,
padahal kerugian membuangnya
$0.2 \times 0 + 0.8 \times 1 = 0.8$.
Kekhawatiran kita ternyata benar:
seperti yang dikatakan ahli mikologi mana pun kepada kami,
jamur di :numref:`fig_death_cap` sebenarnya
adalah benar jamur *death cap*.

Klasifikasi bisa menjadi jauh lebih rumit dari sekedar
klasifikasi biner, multikelas, atau bahkan multi-label.
Misalnya, ada beberapa varian klasifikasi
untuk menangani hierarki.
Hierarki mengasumsikan bahwa terdapat beberapa hubungan di antara beberapa kelas.
Jadi tidak semua kesalahan bernilai sama --- jika kita terpaksa harus salah, kita lebih suka
untuk salah mengklasifikasikan ke kelas yang masih berhubungan daripada ke kelas yang jauh.
Biasanya, ini disebut sebagai *klasifikasi hierarkis*.
Salah satu contoh awal adalah [Linnaeus](https://en.wikipedia.org/wiki/Carl_Linnaeus), yang mengatur hewan dalam hierarki.

Dalam kasus klasifikasi hewan,
mungkin tidak terlalu buruk untuk salah mengira pudel (jenis anjing) sebagai *schnauzer* (jenis anjing lain),
tetapi model kita akan membayar hukuman yang besar
jika salah memprediksi anjing pudel sebagai dinosaurus.
Hierarki mana yang relevan mungkin bergantung
tentang bagaimana Anda berencana menggunakan model tersebut.
Misalnya ular berbisa dan ular *garter*
mungkin memang dekat dalam hubungan filogenetik,
tapi salah mengira ular derik sebagai garter bisa mematikan.

#### Pemberian Tag

Beberapa masalah klasifikasi sangat cocok 
ke dalam konfigurasi klasifikasi biner atau multikelas.
Misalnya, kita bisa melatih pengklasifikasi biner normal
untuk membedakan kucing dari anjing.
Mengingat kemajuan visi komputer saat ini,
kita dapat melakukannya dengan mudah, dengan alat siap pakai.
Meskipun demikian, tidak peduli seberapa akurat model kita,
kita mungkin menemukan diri kita dalam masalah saat pengklasifikasi
menemukan gambar *Town Musicians of Bremen*,
dongeng Jerman populer yang menampilkan empat hewan
di :numref:`fig_stackedanimals`.


![Keledai, anjing, kucing, dan ayam jantan.](../img/stackedanimals.png)
:width:`300px`
:label:`fig_stackedanimals`

Seperti yang Anda lihat, ada kucing di :numref:`fig_stackedanimals`,
dan ayam jantan, anjing, dan keledai,
dengan beberapa pohon di latar belakang.
Tergantung apa yang ingin kita lakukan dengan model kita
akhirnya, memperlakukan ini sebagai masalah klasifikasi biner
mungkin tidak masuk akal.
Sebagai gantinya, kita mungkin ingin memberi model opsi
untuk bisa mengatakan gambar itu menggambarkan seekor kucing, seekor anjing, seekor keledai,
*dan* ayam jantan.

Masalah belajar memprediksi kelas yang 
tidak saling eksklusif disebut *klasifikasi multi-label*.
Masalah pemberian tag otomatis biasanya paling baik dijelaskan
sebagai masalah klasifikasi multi-label.
Pikirkan tentang tag yang mungkin diterapkan orang pada tulisan di blog teknis,
mis., "pembelajaran mesin", "teknologi", "gadget",
"bahasa pemrograman", "Linux", "komputasi awan", "AWS".
Artikel biasa mungkin menerapkan 5--10 tag
karena konsep-konsep ini saling berhubungan.
Postingan tentang "komputasi awan" cenderung menyebut "AWS"
dan postingan tentang "pembelajaran mesin" juga bisa berhubungan
dengan "bahasa pemrograman".

Kami juga harus menghadapi masalah seperti ini saat menangani 
literatur biomedis, di mana pemberian tag artikel dengan benar itu penting
karena memungkinkan peneliti untuk melakukan tinjauan pustaka yang mendalam.
Di *National Library of Medicine*, sejumlah anotator profesional
membahas setiap artikel yang diindeks di *PubMed*
untuk mengaitkannya dengan istilah yang relevan dari *MeSH*,
kumpulan sekitar 28.000 tag.
Ini adalah proses yang memakan waktu dan
annotator biasanya memiliki jeda satu tahun antara pengarsipan dan pemberian tag.
Pembelajaran mesin dapat digunakan di sini untuk memberikan tag sementara
hingga setiap artikel memiliki tinjauan manual yang tepat.
Memang, selama beberapa tahun, organisasi *BioASQ*
telah [menyelenggarakan kompetisi](http://bioasq.org/) untuk melakukan hal ini dengan tepat.

#### Pencarian 

Terkadang kita tidak hanya ingin menetapkan setiap contoh ke sebuah keranjang
atau ke angka riel. Di bidang pencarian informasi,
kita ingin menerapkan pemeringkatan pada suatu kumpulan objek.
Ambil contoh penelusuran web.
Tujuannya bukan untuk menentukan apakah
halaman tertentu relevan untuk suatu kueri, tetapi,
halaman mana di antara hasil pencarian yang sangat banyak, 
yang paling relevan
untuk seorang pengguna tertentu.
Kita sangat memperhatikan urutan hasil pencarian yang relevan
dan algoritma pembelajaran kita perlu menghasilkan sub himpunan yang terurut 
dari himpunan yang lebih besar.
Dengan kata lain, jika kita diminta untuk menghasilkan 5 huruf pertama dari alfabet, ada perbedaan
antara mengembalikan "A B C D E" dan "C A B E D".
Meskipun hasil setnya sama,
urutan objek di dalamnya penting.

Salah satu solusi yang mungkin untuk masalah ini adalah pertama menetapkan 
untuk setiap elemen dalam himpunan suatu skor relevansi yang sesuai
dan kemudian untuk mengambil elemen peringkat teratas.
[PageRank](https://en.wikipedia.org/wiki/PageRank),
saus rahasia orisinil di balik mesin pencari Google
adalah contoh awal dari sistem penilaian seperti itu yang 
tidak biasa karena tidak bergantung pada kueri yang sebenarnya.
Di sini, cara ini mengandalkan filter relevansi sederhana
untuk mengidentifikasi kumpulan item yang relevan
dan kemudian menggunakan PageRank untuk mengurutkan hasil 
yang mengandung kueri.
Saat ini, mesin pencari menggunakan pembelajaran mesin dan model perilaku
untuk mendapatkan skor relevansi yang bergantung pada kueri.
Ada banyak konferensi akademik yang seluruhnya ditujukan untuk subjek ini.

#### Sistem Pemberi Rekomendasi
:label:`subsec_recommender_systems`

Sistem pemberi rekomendasi adalah masalah lainnya
yang terkait dengan penelusuran dan pemeringkatan.
Masalah ini memiliki kemiripan tujuan, yaitu menampilkan sekumpulan item yang relevan kepada pengguna.
Perbedaan utamanya adalah penekanannya pada *personalisasi*
kepada pengguna tertentu dalam konteks sistem pemberi rekomendasi.
Misalnya, untuk rekomendasi film,
halaman hasil untuk penggemar fiksi ilmiah
dan halaman hasil
bagi penikmat komedi *Peter Sellers* mungkin sangat berbeda.
Masalah serupa muncul di pengaturan rekomendasi lainnya,
mis., untuk produk ritel, musik, dan rekomendasi berita.

Dalam beberapa kasus, pelanggan memberikan umpan balik secara eksplisit untuk melaporkan 
seberapa besar mereka menyukai produk tertentu
(misalnya, peringkat dan ulasan produk di Amazon, IMDb, dan GoodReads).
Dalam beberapa kasus lain, mereka memberikan umpan balik implisit,
mis., dengan mengabaikan judul-judul lagu tertentu pada daftar putar,
yang mungkin menunjukkan ketidaksukaan tetapi mungkin juga hanya menunjukkan
bahwa lagu tersebut tidak sesuai konteksnya.
Dalam formulasi paling sederhana, sistem ini dilatih
untuk memperkirakan beberapa skor,
seperti perkiraan peringkat
atau kemungkinan terjadinya pembelian 
berdasarkan pengguna dan barang.

Dengan model seperti itu,
untuk seorang pengguna,
kita dapat mengambil kumpulan objek dengan skor terbesar,
yang kemudian dapat direkomendasikan kepada pengguna tersebut.
Sistem produksi jauh lebih maju dan cepat
dalam merekam aktivitas pengguna secara rinci dan karakteristik barang ke dalam akun
saat menghitung skor tersebut. :numref:`fig_deeplearning_amazon` adalah sebuah contoh
buku *deep learning* yang direkomendasikan oleh Amazon berdasarkan algoritma personalisasi yang disesuaikan untuk menangkap preferensi seseorang.

![Deep learning books recommended by Amazon.](../img/deeplearning-amazon.jpg)
:label:`fig_deeplearning_amazon`

Meskipun memiliki nilai ekonomi yang luar biasa,
sistem rekomendasi yang 
dibangun secara naif di atas model prediktif
menderita beberapa kekurangan konseptual yang serius.
Untuk memulai, kita hanya mengamati *umpan balik yang disensor*:
pengguna biasanya hanya akan memberi penilaian pada film yang mereka memiliki perasaan kuat tentangnya.
Sebagai contoh,
dalam skala lima poin,
Anda mungkin memperhatikan bahwa suatu barang menerima banyak peringkat lima dan satu bintang
tetapi hanya ada sedikit peringkat bintang tiga yang mencolok.
Selain itu, kebiasaan membeli saat ini sering kali terjadi
akibat dari efek algoritma rekomendasi yang saat ini diterapkan,
tetapi algoritma pembelajaran tidak selalu memperhitungkan detail ini.
Dengan demikian, dapat terbentuk siklus umpan balik 
di mana sistem pemberi rekomendasi mendorong barang
yang kemudian dianggap lebih baik (karena pembelian lebih banyak)
dan hal ini mendorong barang ini menjadi lebih sering lagi direkomendasikan.
Banyak dari masalah tentang cara menangani penyensoran,
insentif, dan siklus umpan balik, adalah pertanyaan penelitian terbuka yang penting.

