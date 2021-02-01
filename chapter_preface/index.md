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
Dengan kemajuan-kemajuan ini, kita sekarang dapat membuat mobil swa-kemudi
dengan lebih banyak otonomi daripada sebelumnya (dan lebih sedikit otonomi
dari beberapa perusahaan yang mungkin Anda percayai),
sistem penjawab cerdas yang secara otomatis membuat draf email paling umum,
membantu orang-orang dari masalah *inbox* email yang sangat besar,
dan agen perangkat lunak yang mendominasi pemain terbaik dunia
di permainan papan seperti Go, suatu prestasi yang pernah dianggap akan datang beberapa dekade lagi.
Alat-alat ini telah memberikan dampak yang lebih luas pada industri dan masyarakat,
mengubah cara pembuatan film, diagnosis penyakit,
dan memainkan peran yang lebih luas dalam ilmu pengetahuan dasar --- dari astrofisika hingga biologi.

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
untuk mengubah, menerapkan, dan memperluas aplikasi umum agar sesuai dengan kebutuhan mereka.
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
untuk mempercepat proses belajar para calon praktisi.

Pada saat kami memulai proyek buku ini,
tidak ada sumber daya yang 
(i) mutakhir; (ii) membahas lengkap 
*machine learning* moderen dengan kedalaman teknis yang mendalam;
dan (iii) sajian bergantian antara buku teks menarik dan berkualitas dan kode yang bersih dan dapat dijalankan yang biasanya ditemukan dalam kelas *hands-on*.
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
dipublikasikan di situs web [Distill] (http://distill.pub), atau blog pribadi,
mereka hanya membahas topik terpilih dalam *deep learning*,
dan sering kali tidak mengandung kode.
Di sisi lain, sementara beberapa buku teks telah muncul,
terutama: cite: `Goodfellow.Bengio.Courville.2016`,
yang menawarkan survei komprehensif tentang konsep di balik *deep learning*,
namun sumber daya ini tidak menjembatani konsep dan implementasi konsep dalam kode,
terkadang membuat pembaca tidak mengerti bagaimana menerapkan konsep-konsep tersebut.
Selain itu, terlalu banyak sumber daya yang tersembunyi di balik tembok 
penyedia kursus komersial.

