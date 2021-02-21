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

To motivate the approach, let us start with a running example.
Suppose that we wish to estimate the prices of houses (in dollars)
based on their area (in square feet) and age (in years).
To actually develop a model for predicting house prices,
we would need to get our hands on a dataset
consisting of sales for which we know
the sale price, area, and age for each home.
In the terminology of machine learning,
the dataset is called a *training dataset* or *training set*,
and each row (here the data corresponding to one sale)
is called an *example* (or *data point*, *data instance*, *sample*).
The thing we are trying to predict (price)
is called a *label* (or *target*).
The independent variables (age and area)
upon which the predictions are based
are called *features* (or *covariates*).

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

The linearity assumption just says that the target (price)
can be expressed as a weighted sum of the features (area and age):

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
yang ditandai dengan
a *transformasi linier* fitur melalui penjumlahan tertimbang, dikombinasikan dengan
a *translasi* melalui bias yang ditambahkan.

Diberikan dataset, tujuan kita adalah memilih
bobot $\mathbf{w}$ dan bias $b$ sedemikian rupa sehingga secara rata-rata,
prediksi yang dibuat oleh model kita 
paling sesuai dengan harga sebenarnya dalam data.
Model yang prediksi keluarannya
ditentukan oleh transformasi *affine* dari fitur masukan
adalah *model linier*,
dimana transformasi *affine* ditentukan oleh bobot dan bias yang dipilih.

In disciplines where it is common to focus
on datasets with just a few features,
explicitly expressing models long-form like this is common.
In machine learning, we usually work with high-dimensional datasets,
so it is more convenient to employ linear algebra notation.
When our inputs consist of $d$ features,
we express our prediction $\hat{y}$ (in general the "hat" symbol denotes estimates) as

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

Before we start thinking about how to *fit* data with our model,
we need to determine a measure of *fitness*.
The *loss function* quantifies the distance
between the *real* and *predicted* value of the target.
The loss will usually be a non-negative number
where smaller values are better
and perfect predictions incur a loss of 0.
The most popular loss function in regression problems
is the squared error.
When our prediction for an example $i$ is $\hat{y}^{(i)}$
and the corresponding true label is $y^{(i)}$,
the squared error is given by:

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

Note that large differences between
estimates $\hat{y}^{(i)}$ and observations $y^{(i)}$
lead to even larger contributions to the loss,
due to the quadratic dependence.
To measure the quality of a model on the entire dataset of $n$ examples,
we simply average (or equivalently, sum)
the losses on the training set.

Perhatikan bahwa perbedaan besar antara
estimasi $\hat{y}^{(i)}$ dan pengamatan $y^{(i)}$
menyebabkan kontribusi kerugian yang lebih besar,
karena ketergantungan kuadrat.
Untuk mengukur kualitas model di seluruh dataset dengan $n$ sampel,
kita hanya merataratakan (atau boleh juga, menjumlahkan)
semua kerugian di dataset pelatihan.

$$L(\mathbf{w}, b) =\frac{1}{n}\sum_{i=1}^n l^{(i)}(\mathbf{w}, b) =\frac{1}{n} \sum_{i=1}^n \frac{1}{2}\left(\mathbf{w}^\top \mathbf{x}^{(i)} + b - y^{(i)}\right)^2.$$

When training the model, we want to find parameters ($\mathbf{w}^*, b^*$)
that minimize the total loss across all training examples:

Ketika melatih model, kita ingin menemukan parameter ($\mathbf{w}^*, b^*$)
yang meminimalkan total kerugian dari seluruh sampel pelatihan:

$$\mathbf{w}^*, b^* = \operatorname*{argmin}_{\mathbf{w}, b}\  L(\mathbf{w}, b).$$


