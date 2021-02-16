# Aljabar Linier
:label:`sec_linear-algebra`

Sekarang setelah Anda dapat menyimpan dan memanipulasi data,
mari kita tinjau secara singkat bagian dari aljabar linier dasar
yang diperlukan untuk memahami dan mengimplementasikan 
sebagian besar model dalam buku ini.
Di bawah ini, kami perkenalkan objek matematika dasar, aritmatika,
dan operasi dalam aljabar linier,
mengekspresikan semuanya itu dengan notasi matematika
dan mengimplementasikannya dalam kode.

## Skalar

Jika Anda belum pernah mempelajari aljabar linier atau pembelajaran mesin,
maka pengalaman Anda sebelumnya dengan matematika mungkin termasuk
memikirkan satu nomor pada satu waktu.
Dan, jika Anda pernah menyeimbangkan buku cek
atau pernah membayar makan malam di restoran
maka Anda sudah tahu bagaimana melakukan hal-hal dasar
seperti menambah dan mengalikan pasangan angka.
Misalnya, suhu di Palo Alto adalah $52$ derajat Fahrenheit.
Secara formal, kami menyebut nilai yang terdiri
hanya dari satu kuantitas numerik sebagai *skalar*.
Jika Anda ingin mengonversi nilai ini ke Celsius
(skala suhu sistem metrik yang lebih masuk akal),
Anda akan mengevaluasi ekspresi $c = \frac{5}{9}(f - 32)$, mengganti $f$ dengan $52$.
Dalam persamaan ini, masing-masing suku --- $5$, $9$, dan $32$ --- adalah nilai skalar.
Simbol $c$ dan $f$ disebut *variabel*
dan mewakili nilai skalar yang tidak diketahui.

Dalam buku ini, kami mengadopsi notasi matematika
dimana variabel skalar dilambangkan
dengan huruf kecil biasa (misalnya, $x$, $y$, dan $z$).
Kami menamakan ruang dari semua skalar *riel* kontinyu dengan $\mathbb{R}$.
Kami akan membahas definisi dari apa tepatnya *ruang* itu,
tapi ingatlah sekarang bahwa ekspresi $x \in \mathbb{R}$
adalah cara formal untuk mengatakan bahwa $x$ adalah skalar riel.
Simbol $\in$ bisa diucapkan "in"
dan hanya menunjukkan keanggotaan dalam satu set.
Secara analogi, kita dapat menulis $x, y \in \{0, 1\}$
untuk menyatakan bahwa $x$ dan $y$ adalah angka
yang nilainya hanya bisa $0$ atau $1$.

(**Sebuah skalar direpresentasikan sebagai sebuah tensor dengan satu elemen.**)
Di cuplikan berikut, kita membuat 2 skalar dan melakukan beberapa operasi
aritmetik dengannya, penjumlahan, perkalian, pembagian dan pemangkatan.

```{.python .input}
from mxnet import np, npx
npx.set_np()

x = np.array(3.0)
y = np.array(2.0)

x + y, x * y, x / y, x ** y
```

```{.python .input}
#@tab pytorch
import torch

x = torch.tensor([3.0])
y = torch.tensor([2.0])

x + y, x * y, x / y, x**y
```

```{.python .input}
#@tab tensorflow
import tensorflow as tf

x = tf.constant([3.0])
y = tf.constant([2.0])

x + y, x * y, x / y, x**y
```


## Vektor

[**Anda bisa menganggap vektor sebagai kumpulan nilai skalar.**]
Kita menyebut nilai-nilai ini sebagai elemen (atau komponen atau entri) dari vektor.
Bila vektor kita merepresentasikan sample dari dataset, 
nilai-nilainya mengandung nilai dunia-nyata yang signifikan.
Contohnya, jika kita melatih sebuah model untuk memprediksi
resiko gagal bayar suatu pinjaman,
kita mungkin mengasosiasikan setiap aplikan dengan sebuah vektor
yang komponennya adalah pendapatan, lama pekerjaan, jumlah gagal bayar sebelumnya, 
dan faktor-faktor lainnya.
Jika kita mempelajari resiko gagal jantung dari seorang pasien di rumah sakit, 
kita mungkin merepresentasikan setiap pasien dengan vektor 
yang komponennya mencakup tanda vital terbaru, level kolesterol, lama olahraga setiap hari, dll.
Dalam notasi matematika, kita biasanya menggambarkan vektor dengan huruf kecil (e.g., $\mathbf{x}$, $\mathbf{y}$, and $\mathbf{z})$.

Kita bekerja dengan vektor dengan tensor satu dimensi.
Secara umum, tensor bisa mempunyai panjang sembarang, dibatasi oleh kapasitas memori.

```{.python .input}
x = np.arange(4)
x
```

```{.python .input}
#@tab pytorch
x = torch.arange(4)
x
```

```{.python .input}
#@tab tensorflow
x = tf.range(4)
x
```

Kita bisa merujuk sembarang elemen dari vektor dengan menggunakan subskrip.
Contohnya, kita merujuk elemen ke-$i$ dari $\mathbf{x}$ dengan $x_i$.
Perhatikan bahwa elemen $x_i$ adalah skalar, 
jadi kita tidak menuliskan dalam huruf tebal.
Karena banyak literatur umumnya menggunakan kolom vektor sebagai orientasi vektor bawaan
sebuah vektor, maka buku ini juga memakainya.
Dalam matematika, vektor $\mathbf{x}$ bisa ditulis sebagai

$$\mathbf{x} =\begin{bmatrix}x_{1}  \\x_{2}  \\ \vdots  \\x_{n}\end{bmatrix},$$
:eqlabel:`eq_vec_def`

di mana $x_1, \ldots, x_n$ adalah elemen dari vektor.

Dalam kode,
kita (**mengakses sembarang elemen dengan menggunakan indeks dari tensor.**)

```{.python .input}
x[3]
```

```{.python .input}
#@tab pytorch
x[3]
```

```{.python .input}
#@tab tensorflow
x[3]
```

### Panjang, Dimensi dan Bentuk

Mari kita lihat kembali beberapa konsep dari :numref:`sec_ndarray`.
Vektor hanyalah senarai dari angka-angka.
Dan seperti senarai yang memiliki panjang, begitu pula vektor.
Dalam notasi matematika, jika kita ingin mengatakan vektor $\mathbf{x}$
terdiri dari $n$ skalar riel,
kita bisa mengekspresikan sebagai $\mathbf{x} \in \mathbb{R}^n$.
Panjang vektor biasanya disebut dimensi vektor.

Seperti senarai Python biasa,
kita bisa mendapatkan panjang tensor dengan fungsi Python `len()`.

```{.python .input}
len(x)
```

```{.python .input}
#@tab pytorch
len(x)
```

```{.python .input}
#@tab tensorflow
len(x)
```

Jika tensor merepresentasikan vektor (dengan satu sumbu),
kita juga bisa mendapatkan panjangnya dengan atribut `.shape`.
*shape* atribut adalah tupel yang mengandung panjang (dimensi) 
untuk setiap sumbu tensor.
(**Tensor dengan satu sumbu, shape hanya terdiri dari satu elemen.**)

```{.python .input}
x.shape
```

```{.python .input}
#@tab pytorch
x.shape
```

```{.python .input}
#@tab tensorflow
x.shape
```

Perhatikan bahwa kata "dimensi" cenderung dipakai berlebihan dalam berbagai konteks
dan ini menimbulkan banyak orang kebingungan.
Untuk memperjelas, kita menggunakan dimensionalitas vektor atau sumbu untuk merujuk
pada panjangnya, yaitu, jumlah elemen suatu vektor atau sumbu.
Namun, kita menggunakan dimensionalitas tensor untuk merujuk pada jumlah sumbu yang 
dimiliki sebuah tensor.
Dalam hal ini, dimensionalitas dari suatu sumbu dari tensor adalah panjang dari sumbu itu.

## Matriks

Seperti vektor adalah generalisasi dari skalar dari orde nol ke orde satu, 
matriks menggeneralisasi vektor dari orde satu ke orde dua.
Matriks, yang biasa dituliskan dengan huruf kapital tebal (mis., $\mathbf{X}$, $\mathbf{Y}$, and $\mathbf{Z}$),
direpresentasikan dalam kode sebagai tensor dengan dua sumbu.

Dalam notasi matematika, kita memakai $\mathbf{A} \in \mathbb{R}^{m \times n}$
untuk mengekspresikan matriks $\mathbf{A}$ terdiri dari $m$ baris dan $n$ kolom berisi skalar bernilai riel.
Secara visual, kita bisa mengilustrasikan sembarang matriks $\mathbf{A} \in \mathbb{R}^{m \times n}$ sebagai tabel,
di mana setiap elemen $a_{ij}$ berasal dari baris ke-$i$ dan kolom ke-$j$:

$$\mathbf{A}=\begin{bmatrix} a_{11} & a_{12} & \cdots & a_{1n} \\ a_{21} & a_{22} & \cdots & a_{2n} \\ \vdots & \vdots & \ddots & \vdots \\ a_{m1} & a_{m2} & \cdots & a_{mn} \\ \end{bmatrix}.$$
:eqlabel:`eq_matrix_def`

Untuk sembarang $\mathbf{A} \in \mathbb{R}^{m \times n}$, bentuk dari $\mathbf{A}$
adalah ($m$, $n$) atau $m \times n$.
Khususnya, jika matriks memiliki jumlah baris dan kolom yang sama,
bentuknya menjadi persegi; sehingga disebut sebagai matriks persegi (*square matrix*).

Kita bisa membuat matriks $m \times n$ 
dengan menyebutkan bentuknya dalam dua komponen $m$ dan $n$
ketika memanggil salah satu fungsi favorit kita untuk membuat tensor.

```{.python .input}
A = np.arange(20).reshape(5, 4)
A
```

```{.python .input}
#@tab pytorch
A = torch.arange(20).reshape(5, 4)
A
```

```{.python .input}
#@tab tensorflow
A = tf.reshape(tf.range(20), (5, 4))
A
```

Kita bisa mengakses elemen skalar $a_{ij}$ dari matriks $\mathbf{A}$ di :eqref:`eq_matrix_def`
dengan menyebutkan indeks untuk baris ($i$) dan kolom ($j$),
seperti $[\mathbf{A}]_{ij}$.
Ketika elemen skalar dari matrix $\mathbf{A}$, seperti di :eqref:`eq_matrix_def`, tidak disebutkan,
kita bisa langsung memakai huruf kecil dari matriks $\mathbf{A}$ dengan indeks subskrip, $a_{ij}$,
untuk merujuk pada $[\mathbf{A}]_{ij}$.
Untuk menjaga notasi tetap sederhana, koma dimasukkan untuk memisahkan indeks hanya jika diperlukan,
seperti $a_{2, 3j}$ dan $[\mathbf{A}]_{2i-1, 3}$.


Terkadang, kita ingin membalikkan sumbu-sumbunya.
Ketika kita menukar baris dan kolom matriks, 
hasilnya disebut dengan *transpose* dari matrix.
Secara formal, kita menulis transpos dari matriks $\mathbf{A}$'s dengan $\mathbf{A}^\top$
dan bila $\mathbf{B} = \mathbf{A}^\top$, maka $b_{ij} = a_{ji}$ untuk sembarang $i$ dan $j$.
Jadi, transpose dari $\mathbf{A}$ di :eqref:`eq_matrix_def` adalah
sebuah matriks a $n \times m$:

$$
\mathbf{A}^\top =
\begin{bmatrix}
    a_{11} & a_{21} & \dots  & a_{m1} \\
    a_{12} & a_{22} & \dots  & a_{m2} \\
    \vdots & \vdots & \ddots  & \vdots \\
    a_{1n} & a_{2n} & \dots  & a_{mn}
\end{bmatrix}.
$$

Sekarang kita mengakses matriks transpose dalam kode.

```{.python .input}
A.T
```

```{.python .input}
#@tab pytorch
A.T
```

```{.python .input}
#@tab tensorflow
tf.transpose(A)
```

Sebagai jenis khusus dari matriks persegi,
[**matriks simetris $\mathbf{A}$ sama dengan transposenya:
$\mathbf{A} = \mathbf{A}^\top$.**]
Disini kita mendefinisikan matriks simetris `B`.

```{.python .input}
B = np.array([[1, 2, 3], [2, 0, 4], [3, 4, 5]])
B
```

```{.python .input}
#@tab pytorch
B = torch.tensor([[1, 2, 3], [2, 0, 4], [3, 4, 5]])
B
```

```{.python .input}
#@tab tensorflow
B = tf.constant([[1, 2, 3], [2, 0, 4], [3, 4, 5]])
B
```

Sekarang kita bandingkan `B` dengan transposenya.


```{.python .input}
B == B.T
```

```{.python .input}
#@tab pytorch
B == B.T
```

```{.python .input}
#@tab tensorflow
B == tf.transpose(B)
```

Matriks adalah struktur data yang berguna:
matriks memungkinkan kita untuk mengorganisasi data yang memiliki jenis bervariasi.
Contohnya, baris dalam matriks mungkin berisi rumah, sementara kolom-kolomnya mungkin
berisi atribut-atribut yang berbeda
Ini mungkin terdengar akrab jika anda pernah menggunakan software spreadsheet atau 
pernah membaca :numref:`sec_pandas`.
Jadi, walaupun orientasi bawaan dari satu vektor adalah vektor kolom,
dalam matriks yang merepresentasikan dataset berbentuk tabel, 
biasanya setiap sampel data diperlakukan seperti vektor baris dalam matriks.
Seperti yang akan kita lihat nanti, konvensi ini memungkinkan beberapa praktek umum dalam 
pembelajaran mendalam.
Contohnya, sepanjang sumbu terluar sebuah tensor,
kita bisa mengakses atau mengenumerasi minibatch dari data sampel,
atau data sampel saja jika tidak ada minibatch.

## Tensor

Sama seperti vektor yang menggeneralisasi skalar, dan matriks yang menggeneralisasi vektor, kita bisa membangun
struktur data dengan lebih banyak sumbu.
[**Tensor**]
("tensor"  dalam bagian ini merujuk pada objek algebra)
(**memberikan kita cara yang umum untuk mendeskripsikan senarai $n$-dimensi dengan sembarang jumlah sumbu.**)
Vektor, sebagai contoh, adalah tensor orde-pertama, dan matriks adalah tensor orde-kedua.
Tensor dituliskan dalam huruf kapital dengan font khusus
(contohnya, $\mathsf{X}$, $\mathsf{Y}$, dan $\mathsf{Z}$)
dan mekanisme pengindeksannya (contohnya, $x_{ijk}$ dan $[\mathsf{X}]_{1, 2i-1, 3}$) mirip dengan pengindeksan matriks.

Tensor akan menjadi lebih penting ketika kita mulai bekerja dengan gambar,
yang akan diberikan sebagai senarai $n$-dimensi dengan 3 sumbu bersesuaian dengan tinggi, lebar dan sumbu kanal untuk menumpuk kanal warna (merah, hijau dan biru). Sekarang, kita akan melewati tensor orde tinggi dan berfokus pada dasar-dasarnya.


```{.python .input}
X = np.arange(24).reshape(2, 3, 4)
X
```

```{.python .input}
#@tab pytorch
X = torch.arange(24).reshape(2, 3, 4)
X
```

```{.python .input}
#@tab tensorflow
X = tf.reshape(tf.range(24), (2, 3, 4))
X
```

## Properti Dasar Aritmatika Tensor

Skalar, vektor, matriks dan tensor ("tensor" di bagian ini merujuk pada obyek algebra)
dengan sembarang jumlah sumbu
memiliki beberapa properti yang bagus yang seringkali berguna.
Contohnya, anda mungkin memperhatikan
dari definisi suatu operasi per elemen bahwa 
operasi tunggal per elemen tidak mengubah bentuk operannya.
Demikian pula, [**diberikan dua tensor dengan bentuk sama,
hasil dari sembarang operasi biner per elemen adalah tensor dengan bentuk sama.**]
Contohnya, menambahkan dua matriks dengan bentuk sama
berarti melakukan penjumlahan per elemen dari kedua matriks tersebut.

```{.python .input}
A = np.arange(20).reshape(5, 4)
B = A.copy()  # Assign a copy of `A` to `B` by allocating new memory
A, A + B
```

```{.python .input}
#@tab pytorch
A = torch.arange(20, dtype=torch.float32).reshape(5, 4)
B = A.clone()  # Assign a copy of `A` to `B` by allocating new memory
A, A + B
```

```{.python .input}
#@tab tensorflow
A = tf.reshape(tf.range(20, dtype=tf.float32), (5, 4))
B = A  # No cloning of `A` to `B` by allocating new memory
A, A + B
```

Secara khusus, 
perkalian per elemen dari dua matriks disebut dengan perkalian *Hadamard*
(dengan notasi matematika $\odot$).
Perhatikan matriks $\mathbf{B} \in \mathbb{R}^{m \times n}$ di mana elemen pada baris $i$ dan kolom $j$ adalah $b_{ij}$.
Perkalian Hadamard dari matriks $\mathbf{A}$ (defined in :eqref:`eq_matrix_def`) dan $\mathbf{B}$

$$
\mathbf{A} \odot \mathbf{B} =
\begin{bmatrix}
    a_{11}  b_{11} & a_{12}  b_{12} & \dots  & a_{1n}  b_{1n} \\
    a_{21}  b_{21} & a_{22}  b_{22} & \dots  & a_{2n}  b_{2n} \\
    \vdots & \vdots & \ddots & \vdots \\
    a_{m1}  b_{m1} & a_{m2}  b_{m2} & \dots  & a_{mn}  b_{mn}
\end{bmatrix}.
$$

```{.python .input}
A * B
```

```{.python .input}
#@tab pytorch
A * B
```

```{.python .input}
#@tab tensorflow
A * B
```

[**Perkalian dan penambahan sebuah tensor dan sebuah skalar**] juga tidak mengubah bentuk tensor,
di mana setiap elemen dari tensor operan akan ditambahkan atau dikalikan dengan skalar.

```{.python .input}
a = 2
X = np.arange(24).reshape(2, 3, 4)
a + X, (a * X).shape
```

```{.python .input}
#@tab pytorch
a = 2
X = torch.arange(24).reshape(2, 3, 4)
a + X, (a * X).shape
```

```{.python .input}
#@tab tensorflow
a = 2
X = tf.reshape(tf.range(24), (2, 3, 4))
a + X, (a * X).shape
```

## Reduksi
:label:`subseq_lin-alg-reduction`

Satu operasi berguna yang bisa kita lakukan dengan sembarang tensor
adalah untuk 
menghitung [**jumlah dari elemen-elemennya.**]
Dalam notasi matematika, kita mengekspresikan jumlah menggunakan simbol $\sum$.
Untuk mengekspresikan jumlah elemen dalam vektor $\mathbf{x}$ dengan panjang $d$,
kita menulis $\sum_{i=1}^d x_i$.
Dalam kode, kita bisa memanggil fungsi untuk menghitung jumlah.

```{.python .input}
x = np.arange(4)
x, x.sum()
```

```{.python .input}
#@tab pytorch
x = torch.arange(4, dtype=torch.float32)
x, x.sum()
```

```{.python .input}
#@tab tensorflow
x = tf.range(4, dtype=tf.float32)
x, tf.reduce_sum(x)
```

Kita bisa mengekspresikan [**jumlah dari semua elemen tensor dengan bentuk sembarang.**]
Contohnya, jumlah dari elemen-elemen matriks $\mathbf{A}$ berdimensi $m \times n$ bisa ditulis sebagai $\sum_{i=1}^{m} \sum_{j=1}^{n} a_{ij}$.

```{.python .input}
A.shape, A.sum()
```

```{.python .input}
#@tab pytorch
A.shape, A.sum()
```

```{.python .input}
#@tab tensorflow
A.shape, tf.reduce_sum(A)
```

Secara bawaan, memanggil fungsi untuk menghitung jumlah berarti 
mereduksi tensor sepanjang semua sumbunya menjadi sebuah skalar.
Kita juga bisa menyebutkan sumbu di mana tensor akan direduksi dengan penjumlahan.
Kita bisa mengambil contoh matriks.
Untuk mereduksi dimensi baris (sumbu 0) dengan menjumlahkan elemen-elemen dari semua baris,
kita harus menambahkan `axis=0` ketika memanggil fungsinya.
Karena matriks input mereduksi sepanjang sumbu 0 untuk menghasilkan vektor output,
dimensi dari sumbu 0 dari input menjadi hilang di bentuk output.

```{.python .input}
A_sum_axis0 = A.sum(axis=0)
A_sum_axis0, A_sum_axis0.shape
```

```{.python .input}
#@tab pytorch
A_sum_axis0 = A.sum(axis=0)
A_sum_axis0, A_sum_axis0.shape
```

```{.python .input}
#@tab tensorflow
A_sum_axis0 = tf.reduce_sum(A, axis=0)
A_sum_axis0, A_sum_axis0.shape
```

Menyebutkan `axis=1` akan mereduksi dimensi kolom (sumbu 1) dengan menjumlahkan elemen-elemen di semua kolom.
Sehingga, dimensi dari sumbu 1 dari input akan hilang di bentuk output.

```{.python .input}
A_sum_axis1 = A.sum(axis=1)
A_sum_axis1, A_sum_axis1.shape
```

```{.python .input}
#@tab pytorch
A_sum_axis1 = A.sum(axis=1)
A_sum_axis1, A_sum_axis1.shape
```

```{.python .input}
#@tab tensorflow
A_sum_axis1 = tf.reduce_sum(A, axis=1)
A_sum_axis1, A_sum_axis1.shape
```

Mereduksi matriks sepanjang baris dan kolom sekaligus melalui penjumlahan 
adalah ekuivalen dengan menjumlahkan semua elemen matriks.

```{.python .input}
A.sum(axis=[0, 1])  # Same as `A.sum()`
```

```{.python .input}
#@tab pytorch
A.sum(axis=[0, 1])  # Same as `A.sum()`
```

```{.python .input}
#@tab tensorflow
tf.reduce_sum(A, axis=[0, 1])  # Same as `tf.reduce_sum(A)`
```

Kuantitas terkait lainnya adalah *mean*, yang juga disebut dengan rata-rata.
Kita menghitung rata-rata dengan membagi jumlah dengan total banyaknya elemen.
Dalam kode, kita bisa memanggil fungsi untuk menghitung rata-rata tensor 
sembarang bentuk.

```{.python .input}
A.mean(), A.sum() / A.size
```

```{.python .input}
#@tab pytorch
A.mean(), A.sum() / A.numel()
```

```{.python .input}
#@tab tensorflow
tf.reduce_mean(A), tf.reduce_sum(A) / tf.size(A).numpy()
```

Begitu pula, fungsi untuk menghitung rata-rata juga bisa mereduksi tensor sepanjang sumbu tertentu.

```{.python .input}
A.mean(axis=0), A.sum(axis=0) / A.shape[0]
```

```{.python .input}
#@tab pytorch
A.mean(axis=0), A.sum(axis=0) / A.shape[0]
```

```{.python .input}
#@tab tensorflow
tf.reduce_mean(A, axis=0), tf.reduce_sum(A, axis=0) / A.shape[0]
```

### Penjumlahan Non-Reduksi
:label:`subseq_lin-alg-non-reduction`

Namun, terkadang kita perlu untuk menjaga jumlah sumbu tidak berubah
ketika memanggil fungsi untuk menghitung jumlah atau rata-rata.

```{.python .input}
sum_A = A.sum(axis=1, keepdims=True)
sum_A
```

```{.python .input}
#@tab pytorch
sum_A = A.sum(axis=1, keepdims=True)
sum_A
```

```{.python .input}
#@tab tensorflow
sum_A = tf.reduce_sum(A, axis=1, keepdims=True)
sum_A
```

Contohnya, karena `sum_A` tetap menjaga kedua sumbunya setelah menjumlahkan setiap baris, kita bisa membagi `A` dengan `sum_A` melalui *broadcasting*.

```{.python .input}
A / sum_A
```

```{.python .input}
#@tab pytorch
A / sum_A
```

```{.python .input}
#@tab tensorflow
A / sum_A
```

Bila kita ingin menghitung jumlah kumulatif dari elemen-elemen `A` sepanjang suatu sumbu, katakanlah, `axis=0` (baris per baris),
kita memanggil fungsi `cumsum`. Fungsi ini tidak akan mereduksi tensor input sepanjang sumbu apapun.

```{.python .input}
A.cumsum(axis=0)
```

```{.python .input}
#@tab pytorch
A.cumsum(axis=0)
```

```{.python .input}
#@tab tensorflow
tf.cumsum(A, axis=0)
```

## Perkalian Titik (*Dot Product*)

Sejauh ini, kita hanya melakukan operasi per elemen, penjumlahan dan rata-rata. Dan bila hanya ini yang bisa kita lakukan, aljabar linier mungkin tidak pantas 
diberi satu bab khusus. Namun, salah satu operasi fundamental adalah perkalian titik.
Diberikan dua vektor $\mathbf{x}, \mathbf{y} \in \mathbb{R}^d$, perkalian titik $\mathbf{x}^\top \mathbf{y}$ (or $\langle \mathbf{x}, \mathbf{y}  \rangle$) adalah penjumlahan atas perkalian elemen-elemen di posisi yang sama: $\mathbf{x}^\top \mathbf{y} = \sum_{i=1}^{d} x_i y_i$.

[~~Perkalian titik dua vektor adalah penjumlahan atas perkalian elemen-elemen pada posisi yang sama~~]

```{.python .input}
y = np.ones(4)
x, y, np.dot(x, y)
```

```{.python .input}
#@tab pytorch
y = torch.ones(4, dtype = torch.float32)
x, y, torch.dot(x, y)
```

```{.python .input}
#@tab tensorflow
y = tf.ones(4, dtype=tf.float32)
x, y, tf.tensordot(x, y, axes=1)
```

Perhatikan bahwa perkalian titik dua vektor sama saja dengan melakukan perkalian per elemen dan kemudian penjumlahan:

```{.python .input}
np.sum(x * y)
```

```{.python .input}
#@tab pytorch
torch.sum(x * y)
```

```{.python .input}
#@tab tensorflow
tf.reduce_sum(x * y)
```

Perkalian titik berguna dalam berbagai konteks.
Contohnya, diberikan sejumlah nilai dalam vektor $\mathbf{x}  \in \mathbb{R}^d$
dan sekumpulan bobot $\mathbf{w} \in \mathbb{R}^d$,
penjumlahan berbobot dari nilai-nilai dalam $\mathbf{x}$
berdasarkan bobot $\mathbf{w}$
dapat diekspresikan sebagai perkalian titik $\mathbf{x}^\top \mathbf{w}$.
Bila bobotnya non-negatif, 
dan berjumlah satu (i.e., $\left(\sum_{i=1}^{d} {w_i} = 1\right)$),
perkalian titik tersebut disebut rata-rata berbobot (*weighted average*).
Setelah menormalisasi dua vektor dengan panjang unit,
perkalian titik mengekspresikan kosinus dari sudut di antara kedua vektor.
Kita akan secara formal memprekenalkan arti dari *panjang* nanti di seksi ini.

## Perkalian Matriks-Vektor

Sekarang setelah kita mengetahui bagaimana menghitung perkalian titik,
kita bisa mulai mengerti *perkalian matriks-vektor*.
Ingat matriks $\mathbf{A} \in \mathbb{R}^{m \times n}$
dan vektor $\mathbf{x} \in \mathbb{R}^n$
terdefinisi dan tergambarkan di :eqref:`eq_matrix_def` dan :eqref:`eq_vec_def`.
Mari kita mulai dengan memvisualkan matriks $\mathbf{A}$ dalam vektor barisnya

$$\mathbf{A}=
\begin{bmatrix}
\mathbf{a}^\top_{1} \\
\mathbf{a}^\top_{2} \\
\vdots \\
\mathbf{a}^\top_m \\
\end{bmatrix},$$

di mana setiap $\mathbf{a}^\top_{i} \in \mathbb{R}^n$
adalah vektor baris merepresentasikan $i^\mathrm{th}$ baris dari matriks $\mathbf{A}$.

[**Perkalian matriks-vektor$\mathbf{A}\mathbf{x}$
adalah vektor kolom dengan panjang $m$,
di mana elemen ke-$i$ adalah perkalian titik $\mathbf{a}^\top_i \mathbf{x}$:**]

$$
\mathbf{A}\mathbf{x}
= \begin{bmatrix}
\mathbf{a}^\top_{1} \\
\mathbf{a}^\top_{2} \\
\vdots \\
\mathbf{a}^\top_m \\
\end{bmatrix}\mathbf{x}
= \begin{bmatrix}
 \mathbf{a}^\top_{1} \mathbf{x}  \\
 \mathbf{a}^\top_{2} \mathbf{x} \\
\vdots\\
 \mathbf{a}^\top_{m} \mathbf{x}\\
\end{bmatrix}.
$$

Kita bisa menganggap perkalian dengan matriks $\mathbf{A}\in \mathbb{R}^{m \times n}$
sebagai transformasi yang memproyeksikan vektor dari $\mathbb{R}^{n}$ ke $\mathbb{R}^{m}$.
Transformasi ini ternyata sangat berguna.
Contohnya, kita bisa merepresentasikan rotasi 
sebagai perkalian dengan matriks persegi.
Seperti yang akan kita lihat di bab-bab selanjutnya,
kita juga bisa menggunakan perkalian matriks-vektor 
untuk mendeskripsikan kalkulasi paling intensif 
yang diperlukan ketika menghitung setiap lapis dari jaringan saraf
berdasarkan nilai dari lapis sebelumnya.

Mengekspresikan perkalian matriks-vektor dalam kode dengan tensor,
kita menggunakan fungsi `dot` yang sama seperti perkalian titik.
Ketika memanggil `np.dot(A, x)` dengan matrix `A` dan vektor `x`,
perkalian matriks-vektor dilakukan.
Perhatikan bahwa dimensi kolom dari `A` (panjang sumbu 1) 
harus sama dengan dimensi `x` (panjangnya).

```{.python .input}
A.shape, x.shape, np.dot(A, x)
```

```{.python .input}
#@tab pytorch
A.shape, x.shape, torch.mv(A, x)
```

```{.python .input}
#@tab tensorflow
A.shape, x.shape, tf.linalg.matvec(A, x)
```

## Perkalian Matriks-Matriks

Jika Anda telah mengerti perkalian titik dan perkalian matriks-vektor,
maka perkalian matriks-matriks seharusnya mudah.


Misalkan kita punya 2 matriks $\mathbf{A} \in \mathbb{R}^{n \times k}$ dan $\mathbf{B} \in \mathbb{R}^{k \times m}$:

$$\mathbf{A}=\begin{bmatrix}
 a_{11} & a_{12} & \cdots & a_{1k} \\
 a_{21} & a_{22} & \cdots & a_{2k} \\
\vdots & \vdots & \ddots & \vdots \\
 a_{n1} & a_{n2} & \cdots & a_{nk} \\
\end{bmatrix},\quad
\mathbf{B}=\begin{bmatrix}
 b_{11} & b_{12} & \cdots & b_{1m} \\
 b_{21} & b_{22} & \cdots & b_{2m} \\
\vdots & \vdots & \ddots & \vdots \\
 b_{k1} & b_{k2} & \cdots & b_{km} \\
\end{bmatrix}.$$


$\mathbf{a}^\top_{i} \in \mathbb{R}^k$
adalah vektor baris merepresentasikan baris ke-$i$ dari matriks $\mathbf{A}$,
dan $\mathbf{b}_{j} \in \mathbb{R}^k$
adalah kolom vektor dari kolom ke-$j$ dari matriks $\mathbf{B}$.
Untuk memproduksi perkalian matriks $\mathbf{C} = \mathbf{A}\mathbf{B}$, lebih mudah untuk menggambarkan $\mathbf{A}$ dalam bentuk vektor-vektor barisnya dan $\mathbf{B}$ dalam bentuk vektor-vektor kolomnya:

$$\mathbf{A}=
\begin{bmatrix}
\mathbf{a}^\top_{1} \\
\mathbf{a}^\top_{2} \\
\vdots \\
\mathbf{a}^\top_n \\
\end{bmatrix},
\quad \mathbf{B}=\begin{bmatrix}
 \mathbf{b}_{1} & \mathbf{b}_{2} & \cdots & \mathbf{b}_{m} \\
\end{bmatrix}.
$$


Maka perkalian matriks $\mathbf{C} \in \mathbb{R}^{n \times m}$ dihasilkan dengan menghitung setiap elemennya $c_{ij}$ sebagai perkalian titik $\mathbf{a}^\top_i \mathbf{b}_j$:

$$\mathbf{C} = \mathbf{AB} = \begin{bmatrix}
\mathbf{a}^\top_{1} \\
\mathbf{a}^\top_{2} \\
\vdots \\
\mathbf{a}^\top_n \\
\end{bmatrix}
\begin{bmatrix}
 \mathbf{b}_{1} & \mathbf{b}_{2} & \cdots & \mathbf{b}_{m} \\
\end{bmatrix}
= \begin{bmatrix}
\mathbf{a}^\top_{1} \mathbf{b}_1 & \mathbf{a}^\top_{1}\mathbf{b}_2& \cdots & \mathbf{a}^\top_{1} \mathbf{b}_m \\
 \mathbf{a}^\top_{2}\mathbf{b}_1 & \mathbf{a}^\top_{2} \mathbf{b}_2 & \cdots & \mathbf{a}^\top_{2} \mathbf{b}_m \\
 \vdots & \vdots & \ddots &\vdots\\
\mathbf{a}^\top_{n} \mathbf{b}_1 & \mathbf{a}^\top_{n}\mathbf{b}_2& \cdots& \mathbf{a}^\top_{n} \mathbf{b}_m
\end{bmatrix}.
$$

[**Kita bisa menganggap perkalian matriks-matriks $\mathbf{AB}$ sebagai melakukan $m$ perkalian matriks-vektor dan menggabungkan hasil-hasilnya untuk membentuk matriks $n \times m$.**]
Dalam cuplikan berikut, kita melakukan perkalian matriks pada `A` dan `B`.
Di sini, `A` adalah matriks dengan 5 baris dan 4 kolom,
dan `B` adalah matriks dengan 4 baris dan 4 kolom.
Setelah perkalian, kita mendapatkan matriks dengan 5 baris dan 3 kolom.

```{.python .input}
B = np.ones(shape=(4, 3))
np.dot(A, B)
```

```{.python .input}
#@tab pytorch
B = torch.ones(4, 3)
torch.mm(A, B)
```

```{.python .input}
#@tab tensorflow
B = tf.ones((4, 3), tf.float32)
tf.matmul(A, B)
```

Perkalian matriks-matriks juga bisa disebut dengan *perkalian matriks*, dan seharusnya tidak akan disalahmengertikan dengan perkalian Hadamard.

## Norm
:label:`subsec_lin-algebra-norms`

Beberapa operator yang paling berguna dalam aljabar linier adalah norm.
Secara informal, norm vektor memberitahukan besar sebuah vektor.
Arti dari *ukuran* yang dimaksud disini tidak berarti dimensi, tapi 
yang dimaksud adalah besarnya nilai dari komponen-komponennya.

Dalam aljabar linier, norm vektor adalah fungsi $f$ yang memetakan vektor
ke nilai skalar yang memnuhi beberapa properti.
Diberikan sembarang vektor $\mathbf{x}$,
properti pertama mengatakan
bahwa jika kita menskalakan semua elemen dari vektor 
dengan suatu faktor konstanta $\alpha$,
norm vektor tersebut juga akan terskalakan dengan *nilai mutlak*
dari faktor konstanta yang sama:

$$f(\alpha \mathbf{x}) = |\alpha| f(\mathbf{x}).$$


Properti kedua adalah berkenaan dengan ketidaksamaan segitiga:

$$f(\mathbf{x} + \mathbf{y}) \leq f(\mathbf{x}) + f(\mathbf{y}).$$


Properti ketiga mengatakan bahwa nilai norm harus non-negatif: 

$$f(\mathbf{x}) \geq 0.$$

Hal ini masuk akal, karena dalam hampir semua konteks, *ukuran* terkecil dari sesuatu adalah 0.
Properti terakhir mensyaratkan bahwa norm terkecil dicapai dan hanya mungkin dicapai
dengan vektor yang semua elemennya adalah nol.

$$\forall i, [\mathbf{x}]_i = 0 \Leftrightarrow f(\mathbf{x})=0.$$


