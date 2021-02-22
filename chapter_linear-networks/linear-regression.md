# Regresi Linier
:label:`sec_linear_regression`

*Regresi* mengacu pada sekumpulan metode untuk memodelkan 
hubungan antara satu atau lebih variabel independen
dan variabel dependen.
Dalam ilmu alam dan ilmu sosial,
tujuan regresi paling sering adalah
*mencirikan* hubungan antara masukan dan keluaran.
Pembelajaran mesin, di sisi lain,
paling sering berkaitan dengan *prediksi*.

Masalah regresi muncul setiap kali kita ingin memprediksi nilai numerik.
Contoh umum termasuk memprediksi harga (rumah, saham, dll.),
memprediksi lama tinggal (untuk pasien di rumah sakit),
peramalan permintaan (untuk penjualan eceran), di antara yang lainnya.
Tidak semua masalah prediksi merupakan masalah regresi klasik.
Pada bagian selanjutnya, kami akan memperkenalkan masalah klasifikasi,
di mana tujuannya adalah untuk memprediksi keanggotaan di antara sekumpulan kategori.

## Elemen-Elemen Dasar Regresi Linier

*Regresi linier* mungkin adalah metode yang paling sederhana
dan paling populer di antara alat-alat standar untuk regresi.
Dimulai sejak abad ke-19, regresi linier berangkat dari
beberapa asumsi sederhana.
Pertama, kita berasumsi bahwa hubungan antara
variabel independen $\mathbf{x}$ dan variabel dependen $y$ adalah linier,
yaitu, $y$ dapat diekspresikan sebagai jumlah tertimbang (*weighted sum*)
dari elemen-elemen dalam $\mathbf{x}$,
dengan beberapa derau (*noise*) pada pengamatan.
Kedua, kita berasumsi bahwa derau berperilaku baik 
(mengikuti distribusi Gaussian).

Untuk motivasi, mari kita mulai dengan contoh.
Misalkan kita ingin memperkirakan harga rumah (dalam dolar)
berdasarkan luas (dalam kaki persegi) dan usia (dalam tahun).
Untuk benar-benar mengembangkan model untuk memprediksi harga rumah,
kita perlu mendapatkan dataset penjualan yang mengandung data 
harga jual, luas, dan usia untuk setiap rumah.
Dalam terminologi pembelajaran mesin,
dataset tersebut disebut *dataset pelatihan* atau *set pelatihan*,
dan setiap baris (di sini data terkait dengan satu penjualan)
disebut *contoh* (atau *titik data*, *contoh data*, *sampel*).
Hal yang kita coba prediksi (harga)
disebut *label* (atau *target*).
Variabel independen (umur dan luas)
yang menjadi dasar prediksi
disebut *fitur* (atau *kovariat*).

Biasanya, kita memakai $n$ sebagai jumlah sampel dalam dataset.
Kita mengindeks sampel dengan $i$, menuliskan setiap masukan 
sebagai $\mathbf{x}^{(i)} = [x_1^{(i)}, x_2^{(i)}]^\top$
dan labelnya sebagai $y^{(i)}$.

### Model Linier
:label:`subsec_linear_model`

Asumsi linieritas hanya mengatakan bahwa target (harga)
dapat dinyatakan sebagai jumlah tertimbang fitur-fiturnya (luas dan usia):

$$\mathrm{price} = w_{\mathrm{area}} \cdot \mathrm{area} + w_{\mathrm{age}} \cdot \mathrm{age} + b.$$
:eqlabel:`eq_price-area`

Dalam :eqref:`eq_price-area`, $w_{\mathrm{area}}$ and $w_{\mathrm{age}}$
disebut *bobot*, dan $b$ disebut *bias*
(juga disebut *offset* atau *intercept*).
Bobot menentukan pengaruh setiap fitur
pada prediksi kita dan bias hanya mengatakan
berapa nilai prediksi ketika semua fitur bernilai 0.
Bahkan jika kita tidak akan pernah melihat rumah dengan luas nol,
atau rumah berusia nol tahun,
kita masih membutuhkan bias sebab bila tidak kita akan membatasi ekspresifitas
model kita.
Sebenarnya, :eqref:`eq_price-area` adalah transformasi *affine* fitur masukan,
yang ditandai dengan *transformasi linier* fitur melalui penjumlahan tertimbang, dikombinasikan dengan
*translasi* melalui bias yang ditambahkan.

Diberikan dataset, tujuan kita adalah memilih
bobot $\mathbf{w}$ dan bias $b$ sedemikian rupa sehingga secara rata-rata,
prediksi yang dibuat oleh model kita 
paling sesuai dengan harga sebenarnya dalam data.
Model yang prediksi keluarannya
ditentukan oleh transformasi *affine* dari fitur masukan
adalah *model linier*,
dimana transformasi *affine* ditentukan oleh bobot dan bias yang dipilih.

Dalam disiplin ilmu di mana adalah hal yang biasa untuk berfokus pada dataset dengan hanya beberapa fitur,
mengekspresikan model secara eksplisit dalam bentuk panjang seperti ini adalah hal biasa.
Namun dalam pembelajaran mesin, kita biasanya bekerja dengan dataset berdimensi tinggi,
jadi akan lebih mudah untuk menggunakan notasi aljabar linier.
Jika masukan kita terdiri dari $d$ fitur,
kita menyatakan prediksi kita $\hat{y}$ (secara umum simbol "topi" menunjukkan perkiraan) sebagai

$$\hat{y} = w_1  x_1 + ... + w_d  x_d + b.$$

Mengumpulkan semua fitur dalam vektor $\mathbf{x} \in \mathbb{R}^d$
dan semua bobot dalam vektor $\mathbf{w} \in \mathbb{R}^d$,
kita bisa mengekspresikan model kita secara ringkas dengan perkalian titik:

$$\hat{y} = \mathbf{w}^\top \mathbf{x} + b.$$
:eqlabel:`eq_linreg-y`

Dalam :eqref:`eq_linreg-y`, vektor $\mathbf{x}$ berhubungan dengan fitur-fitur dari satu sampel data.
Seringkali akan lebih mudah bagi kita untuk merujuk fitur-fitur dari seluruh dataset dengan $n$ sampel
melalui matriks desain (*design matrix*) $\mathbf{X} \in \mathbb{R}^{n \times d}$.
Di sini, $\mathbf{X}$ mengandung satu baris untuk setiap sampel dan satu kolom untuk setiap fitur.

Untuk kumpulan fitur $\mathbf{X}$,
prediksi $\hat{\mathbf{y}} \in \mathbb{R}^n$
dapat diekspresikan melalui perkalian matriks-vektor:

$${\hat{\mathbf{y}}} = \mathbf{X} \mathbf{w} + b,$$

di mana penyebaran (*broadcasting*) (lihat :numref:`subsec_broadcasting`) diterapkan dalam penjumlahan.
Diberikan fitur-fitur dataset pelatihan $\mathbf{X}$ beserta labelnya (yang diketahui) $\mathbf{y}$,
tujuan dari regresi linier adalah untuk menemukan
vektor bobot $\mathbf{w}$ dan suku bias $b$ sedemikian hingga ketika diberikan 
fitur-fitur dari sampel data baru yang diambil dari distribusi yang sama dengan $\mathbf{X}$,
prediksi label dari sampel data baru tersebut memiliki galat terkecil.

Bahkan jika kita percaya bahwa model terbaik untuk
memprediksi $y$ berdasarkan $\mathbf{x}$ adalah linier,
kita tidak akan berharap menemukan dataset dunia nyata dengan $n$ sampel di mana 
$y^{(i)}$ sama persis dengan $\mathbf{w}^\top \mathbf{x}^{(i)}+b$
untuk semua $1 \leq i \leq n$.
Misalnya instrumen apa saja yang kita gunakan untuk mengamati
fitur-fitur $\mathbf{X}$ dan label $\mathbf{y}$
mungkin mengalami sedikit galat pengukuran.
Jadi, bahkan saat kita percaya 
bahwa hubungan yang mendasarinya adalah linier,
kita akan memasukkan suku derau (*noise term*) untuk menjelaskan galat tersebut.

Sebelum kita bisa mencari *parameter* (atau *parameter model*) terbaik $\mathbf{w}$ dan $b$,
kita membutuhkan dua hal lagi:
(i) ukuran kualitas untuk suatu model tertentu;
dan (ii) prosedur untuk memutakhirkan model untuk meningkatkan kualitasnya.

### Fungsi Kerugian

Sebelum kita mulai memikirkan tentang cara *menyesuaikan* (*fitting*) data dengan model kita,
kita perlu menentukan ukuran *kesesuaian* (*fitness*).
*Fungsi kerugian* menghitung jarak
antara nilai sebenarnya dan *prediksi* target.
Kerugian biasanya berupa angka non-negatif
dimana nilai yang lebih kecil lebih baik
dan prediksi sempurna mengalami kerugian 0.
Fungsi kerugian paling populer dalam masalah regresi
adalah galat kuadrat.
Saat prediksi kita untuk sebuah sampel $i$ adalah $\hat{y}^{(i)}$
dan label sebenarnya adalah $y^{(i)}$,
galat kuadrat dihitung sebagai:

$$l^{(i)}(\mathbf{w}, b) = \frac{1}{2} \left(\hat{y}^{(i)} - y^{(i)}\right)^2.$$

Konstanta $\frac{1}{2}$ tidak membuat perbedaan nyata
tapi akan terbukti memudahkan notasi,
konstanta ini akan habis saat kita mengambil turunan dari kerugian.
Karena dataset pelatihan diberikan kepada kita, dan karenanya di luar kendali kita,
kesalahan empiris hanya merupakan fungsi dari parameter model.
Untuk membuatnya lebih konkret, perhatikan contoh di bawah ini
di mana kita memplot masalah regresi untuk kasus satu dimensi
seperti yang ditunjukkan di: numref: `fig_fit_linreg`.

![Menyesuaikan data dengan model linier.](../img/fit-linreg.svg)
:label:`fig_fit_linreg`

Perhatikan bahwa perbedaan besar antara
estimasi $\hat{y}^{(i)}$ dan pengamatan $y^{(i)}$
menyebabkan kontribusi kerugian yang lebih besar,
karena ketergantungan kuadrat.
Untuk mengukur kualitas model di seluruh dataset dengan $n$ sampel,
kita hanya merataratakan (atau boleh juga, menjumlahkan)
semua kerugian di dataset pelatihan.

$$L(\mathbf{w}, b) =\frac{1}{n}\sum_{i=1}^n l^{(i)}(\mathbf{w}, b) =\frac{1}{n} \sum_{i=1}^n \frac{1}{2}\left(\mathbf{w}^\top \mathbf{x}^{(i)} + b - y^{(i)}\right)^2.$$

Ketika melatih model, kita ingin menemukan parameter ($\mathbf{w}^*, b^*$)
yang meminimalkan total kerugian dari seluruh sampel pelatihan:

$$\mathbf{w}^*, b^* = \operatorname*{argmin}_{\mathbf{w}, b}\  L(\mathbf{w}, b).$$



### Solusi Analitik

Regresi linier adalah masalah optimasi yang sangat sederhana.
Tidak seperti kebanyakan model lain yang akan kita temui di buku ini,
regresi linier dapat diselesaikan secara analitik dengan menerapkan rumus sederhana.
Untuk memulai, kita bisa memasukkan bias $b$ ke dalam parameter $\mathbf{w}$
dengan menambahkan kolom yang semua berisi angka 1 ke matriks desain.
Kemudian soal prediksi kita adalah meminimalkan $\|\mathbf{y} - \mathbf{X}\mathbf{w}\|^2$.
Hanya ada satu titik kritis di fungsi kerugian 
dan titik itu adalah titik dengan kerugian minimum di seluruh domain.
Mengambil turunan dari fungsi kerugian terhadap $\mathbf{w}$
dan mengaturnya sama dengan nol menghasilkan solusi analitik (bentuk tertutup):

$$\mathbf{w}^* = (\mathbf X^\top \mathbf X)^{-1}\mathbf X^\top \mathbf{y}.$$


Meskipun masalah sederhana seperti regresi linier
dapat diselesaikan dengan solusi analitik,
jangan terbiasa dengan keberuntungan seperti itu.
Meskipun solusi analitik memungkinkan analisis matematika yang bagus,
persyaratan solusi analitik sangat membatasi 
sehingga tidak memungkinkan dalam pembelajaran mendalam.

### Penurunan Gradien Stokastik Minibatch (*Minibatch Stochastic Gradient Descent*)

Bahkan dalam kasus di mana kita tidak dapat menyelesaikan model secara analitis,
ternyata dalam praktiknya kita masih bisa melatihnya secara mangkus (efektif).
Selain itu, untuk banyak tugas, model-model yang sulit dioptimalkan tersebut
ternyata berkinerja jauh lebih baik sehingga usaha untuk mencari tahu cara melatihnya 
menjadi sangat sepadan.

Kunci teknik untuk mengoptimalkan hampir semua model pembelajaran mendalam,
termasuk model-model yang dicakup dalam buku ini,
adalah secara berulang-ulang mengurangi galat dengan memutakhirkan 
parameter-parameter ke arah yang mengurangi fungsi kerugian secara bertahap.
Algoritma ini disebut dengan penurunan gradien (*gradient descent*).

Di setiap iterasi, kita pertama mengambil satu *minibatch* sampel $\mathcal{B}$
terdiri dari sejumlah tetap contoh pelatihan.
Kita kemudian menghitung turunan (gradien) dari kerugian rata-rata terhadap parameter model 
dari *minibatch* tersebut.
Terakhir, kita mengalikan gradien dengan suatu nilai positif tertentu $\eta$ 
dan hasilnya akan mengurangi nilai parameter saat ini.

Kita dapat mengekspresikan pemutakhiran secara matematis sebagai berikut:
($\partial$ menunjukkan turunan parsial):

$$(\mathbf{w},b) \leftarrow (\mathbf{w},b) - \frac{\eta}{|\mathcal{B}|} \sum_{i \in \mathcal{B}} \partial_{(\mathbf{w},b)} l^{(i)}(\mathbf{w},b).$$

Ringkasnya, langkah-langkah dari algoritma ini adalah:
(i) kita menginisialisasi parameter model, biasanya secara acak;
(ii) secara berulang-ulang, mengambil *minibatch* sampel secara acak dari data,
memutakhirkan parameter ke arah negatif dari gradien.
Untuk kerugian kuadrat dan transformasi *affine* kita bisa 
menuliskan ini secara eksplisit sebagai berikut:

$$\begin{aligned} \mathbf{w} &\leftarrow \mathbf{w} -   \frac{\eta}{|\mathcal{B}|} \sum_{i \in \mathcal{B}} \partial_{\mathbf{w}} l^{(i)}(\mathbf{w}, b) = \mathbf{w} - \frac{\eta}{|\mathcal{B}|} \sum_{i \in \mathcal{B}} \mathbf{x}^{(i)} \left(\mathbf{w}^\top \mathbf{x}^{(i)} + b - y^{(i)}\right),\\ b &\leftarrow b -  \frac{\eta}{|\mathcal{B}|} \sum_{i \in \mathcal{B}} \partial_b l^{(i)}(\mathbf{w}, b)  = b - \frac{\eta}{|\mathcal{B}|} \sum_{i \in \mathcal{B}} \left(\mathbf{w}^\top \mathbf{x}^{(i)} + b - y^{(i)}\right). \end{aligned}$$
:eqlabel:`eq_linreg_batch_update`

Perhatikan bahwa $\mathbf{w}$ dan $\mathbf{x}$ adalah vektor dalam :eqref:`eq_linreg_batch_update`.
Di sini, notasi vektor yang lebih elegan membuat matematikanya menjadi lebih mudah dibaca
daripada mengekspresikannya dalah bentuk koefisien,
seperti $w_1, w_2, \ldots, w_d$.
Himpunan kardinalitas $|\mathcal{B}|$ merepresentasikan 
banyaknya jumlah sampel di setiap *minibatch* (ukuran *batch*)
dan $\eta$ menunjukkan laju pembelajaran (*learning rate*).
Kami menekankan bahwa nilai dari ukuran *batch* dan laju pembelajaran
ditentukan secara manual dan bukan dipelajari melalui pelatihan model.
Parameter-parameter ini yang bisa diubah namun tidak dimutakhirkan
dalam proses pelatihan disebut dengan hiperparameter.
Penyetelan hiperparameter adalah proses untuk menentukan hiperparameter,
dan biasanya memerlukan penyesuaian berdasarkan hasil dari pelatihan 
yang dinilai berdasarkan dataset terpisah yang disebut dataset validasi (*validation set*).

Setelah melalui beberapa iterasi pelatihan (atau memenuhi suatu kriteria perhentian tertentu),
kita mencatat estimasi parameter model,
ditunjukkan dengan $\hat{\mathbf{w}}, \hat{b}$.
Perhatikan bahkan bila fungsi kita benar-benar linier dan tanpa gangguan derau,
parameter-parameter ini tidak akan bisa menjadi benar-benar tepat sebagai 
*minimizer* kerugian, karena walaupun algoritma ini perlahan-lahan mengerucut ke 
*minimizer*, dia tidak bisa mencapainya dalam jumlah langkah yang terbatas.

Regresi linear adalah masalah pembelajaran di mana hanya ada satu titik minimum 
di antara seluruh domain.
Namun, untuk model yang lebih kompleks, seperti jaringan mendalam (*deep networks*), 
permukaan fungsi kerugian mengandung banyak titik *minima*.
Untungnya, untuk suatu alasan yang belum dimengerti saat ini, 
praktisi pembelajaran mendalam jarang mengalami masalah untuk menemukan parameter
yang meminimalkan kerugian pada dataset pelatihan.

Tantangan yang lebih sulit adalah untuk menemukan parameter yang menghasilkan
kerugian rendah pada data yang belum pernah dilihat sebelumnya, sebuah tantangan
yang disebut dengan generalisasi. Kita akan selalu menemukan topik ini di sepanjang buku ini.

### Membuat Prediksi dari Model yang Dipelajari

Diberikan model regresi liner yang dipelajari 
$\hat{\mathbf{w}}^\top \mathbf{x} + \hat{b}$,
kita sekarang bisa mengestimasi harga rumah baru 
(tidak ada dalam dataset pelatihan)
berdasarkan luasnya $x_1$ dan usianya $x_2$.
Memperkirakan target berdasarkan figur-fiturnya 
biasa disebut juga dengan membuat prediksi atau inferensi.

Kita akan menggunakan istilah *prediksi* karena 
menyebut langkah ini inferensi, walaupun mulai menjadi jargon
dalam pembelajaran mendalam, kurang tepat.

Dalam statitik, *inferensi* lebih sering menunjukkan
estimasi parameter berdasarkan dataset.
Penyalahgunaan terminologi seperti ini sering menjadi
sumber kebingungan ketika praktisi pembelajaran mendalam
berdiskusi dengan statistikawan.

## Vektorisasi untuk Kecepatan

Ketika melatih model, kita biasanya ingin memproses
seluruh *minibatch* contoh-contoh data sekaligus,
Melakukan ini secara efisien memerlukan (**kita**) (~~seharusnya~~) melakukan (**vektorisasi kalkulasi
dan memanfaatkan pustaka aljabar linier yang cepat daripada menulis *for-loop* sendiri di Python.**)

```{.python .input}
%matplotlib inline
from d2l import mxnet as d2l
import math
from mxnet import np
import time
```

```{.python .input}
#@tab pytorch
%matplotlib inline
from d2l import torch as d2l
import math
import torch
import numpy as np
import time
```

```{.python .input}
#@tab tensorflow
%matplotlib inline
from d2l import tensorflow as d2l
import math
import tensorflow as tf
import numpy as np
import time
```

Untuk mengilustrasikan mengapa hal ini sangat penting,
kita bisa (**melihat contoh dua metode untuk menjumlahkan vektor.**)
Untuk memulai kita menginstansiasi dua vektor 10000-dimensi
berisi semua angka satu.
Dalam satu metode kita akan melakukan perulangan atas kedua vektor dengan Python for-loop.
Dalam metode lainnya kita akan mengandalkan satu panggilan ke `+`.

```{.python .input}
#@tab all
n = 10000
a = d2l.ones(n)
b = d2l.ones(n)
```
Karena kita akan sering membuat *benchmark* berdasarkan waktu di buku ini,
[**mari kita definisikan sebuah *timer***].

```{.python .input}
#@tab all
class Timer:  #@save
    """Record multiple running times."""
    def __init__(self):
        self.times = []
        self.start()

    def start(self):
        """Start the timer."""
        self.tik = time.time()

    def stop(self):
        """Stop the timer and record the time in a list."""
        self.times.append(time.time() - self.tik)
        return self.times[-1]

    def avg(self):
        """Return the average time."""
        return sum(self.times) / len(self.times)

    def sum(self):
        """Return the sum of time."""
        return sum(self.times)

    def cumsum(self):
        """Return the accumulated time."""
        return np.array(self.times).cumsum().tolist()
```

