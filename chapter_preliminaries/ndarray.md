# Manipulasi Data
:label:`sec_ndarray`

Agar bisa menyelesaikan sesuatu, kita perlu suatu cara untuk menyimpan dan memanipulasi data.
Secara umum, ada dua hal penting yang perlu dilakukan terhadap data: (i) pengumpulan data;
dan (ii) mengolahnya setelah data tersebut tersimpan di komputer. Tidak ada gunanya mengumpulkan data
tanpa ada cara untuk menyimpannya, jadi mari kita coba bermain-main dengan data buatan.
Pada awalnya, kami mengenalkan array $n$-dimensi, atau disebut juga dengan *tensor*.

Jika Anda pernah bekerja memakai NumPy sebelumnya, paket komputasi ilmiah paling populer di Python,
maka bagian ini akan terdengar akrab bagi Anda.
Apapun kerangka kerja yang Anda pakai, kelas tensor (`ndarray` di MXNet, `Tensor` di PyTorch dan TensorFlow) 
mirip dengan `ndarray` di Numpy dengan beberapa fitur *"mematikan"*.
Pertama, GPU sangat didukung dengan baik untuk mempercepat komputasi sementara NumPy hanya mendukung CPU.
Kedua, kelas tensor mendukung diferensiasi otomatis.
Semua properti ini membuat kelas tensor cocok untuk pembelajaran mendalam.
Di sepanjang buku ini, ketika kami mengatakan tensor, kami merujuk pada obyek dari kelas tensor, kecuali
disebutkan sebaliknya.

## Mulai

Di bagian ini, kami ingin menyiapkan anda,
membekali Anda dengan matematika dasar dan komputasi numerik
yang akan Anda kembangkan seiring kemajuan Anda dalam buku ini.
Jangan khawatir jika Anda kesulitan untuk memahami beberapa
konsep matematika atau fungsi pustaka.
Pada bab-bab berikutnya kita akan meninjau kembali materi ini
dalam konteks contoh praktis dan itu akan membuat anda lebih mengerti.
Di sisi lain, jika Anda sudah memiliki latar belakang tentang ini
dan ingin mempelajari lebih dalam tentang konten matematika, lewati saja bagian ini.

:begin_tab:`mxnet`
Untuk mengawali, kita impor modul `np` (`numpy`) dan 
`npx` (`numpy_extension`) dari MXNet.
Di sini, modul `np` mengandung fungsi-fungsi yang didukung NumPy,
sementara modul `npx` mengandung sekumpulan tambahan yang dikembangkan untuk
mendukung pembelajaran mendalam dalam lingkungan mirip NumPy.
Ketika memakai tensor, kita hampir selalu memanggil fungsi `set_np`:
ini untuk kompatibilitas pengolahan tensor oleh komponen lain dari MXNet.
:end_tab:

:begin_tab:`pytorch`
(**Untuk mengawali, kita impor `torch`. Perhatikan bahwa meskipun ini dipanggil PyTorch, 
kita harus impor `torch` bukannya `pytorch`.**)
:end_tab:

:begin_tab:`tensorflow`
Untuk mengawali, kita impor `tensorflow`. Karena namanya agak panjang, kita sering menyingkatnya
dengan nama aliasnya `tf`.
:end_tab:

```{.python .input}
from mxnet import np, npx
npx.set_np()
```

```{.python .input}
#@tab pytorch
import torch
```

```{.python .input}
#@tab tensorflow
import tensorflow as tf
```

[**Tensor merepresenasikan sebuah senarai (mungkin multi-dimensi) dari nilai-nilai numerik.**]
Tensor dengan satu sumbu secara matematis mirip dengan vektor.
Tensor dengan dua sumbu mirip dengan matriks.
Sedangkan tensor dengan lebih dari dua sumbu tidak memiliki nama khusus dalam matematika.

Sebagai permulaan, kita bisa menggunakan `arange` untuk membuat vektor baris `x`
mengandung 12 bilangan bulat pertama dimulai dari 0,
meskipun mereka dibuat sebagai bilangan pecahan (*float*) secara bawaan.

Setiap nilai dalam tensor disebut dengan *elemen* dari tensor.
Contohnya, terdapat 12 elemen dalam tensor `x`.
Kecuali disebutkan sebaliknya, tensor baru akan disimpan di memori utama dan ditujukan untuk
komputasi berbasis CPU.

```{.python .input}
x = np.arange(12)
x
```

```{.python .input}
#@tab pytorch
x = torch.arange(12)
x
```

```{.python .input}
#@tab tensorflow
x = tf.range(12)
x
```

(**Kita bisa mengakses *bentuk* tensor**) (~~dan jumlah total elemen~~) (panjang sepanjang suatu sumbu)
dengan memeriksa properti `shape`.

```{.python .input}
#@tab all
x.shape
```

Jika kita hanya ingin mengetahui jumlah total elemen dalam sebuah tensor,
yaitu, perkalian semua elemen *bentuk*, kita bisa memeriksa ukurannya.
Karena disini kita berhadapan dengan vektor, satu elemen dari `shape`-nya 
identik dengan ukurannya.

```{.python .input}
x.size
```

```{.python .input}
#@tab pytorch
x.numel()
```

```{.python .input}
#@tab tensorflow
tf.size(x)
```

[**Untuk mengubah bentuk tensor tanpa mengubah jumlah atau nilai elemennya**],
kita bisa memanggil fungsi `reshape`.
Contohnya, kita bisa mengubah tensor kita, `x`,
dari vektor baris dengan bentuk (12,) ke matris dengan bentuk (3,4).
Tensor baru ini mengandung nilai yang sama persis,
namun dianggap sebagai matriks terorganisasi dalam 3 baris dan 4 kolom.
Sekali lagi, meskipun bentuknya berubah, elemen-elemennya tidak berubah.
Perhatikan bahwa perubahan bentuk (*reshaping*) tidak mengubah ukuran.

```{.python .input}
#@tab mxnet, pytorch
X = x.reshape(3, 4)
X
```

```{.python .input}
#@tab tensorflow
X = tf.reshape(x, (3, 4))
X
```

Mengubah bentuk dengan cara menyebutkan setiap dimensi secara manual tidak diperlukan.
Jika target kita adalah matriks dengan bentuk (tinggi, lebar),
maka setelah kita tahu lebarnya, tingginya otomatis diketahui secara implisit.
Kenapa kita harus melakukan pembagian sendiri secara?
Dalam contoh di atas, untuk mendapatkan matriks dengan 3 baris,
kita menyebutkan keduanya, 3 baris dan 4 kolom.
Untungnya, tensor dapat secara otomatis mengetahui satu dimensi berdasarkan dimensi-dimensi lainnya.
Kita memanggil kemampuan ini dengan menyebutkan `-1` untuk dimensi yang kita ingin secara otomatis 
ditentukan oleh tensor.
Dalam contoh kita, daripada memanggil `x.reshape(3, 4)`, kita bisa juga secara ekuivalen
memanggil `x.reshape(-1, 4)` atau `x.reshape(3, -1)`.

Biasanya, kita ingin matriks terinisialisasi dengan nol, satu atau konstanta lainnya,
atau bilangan acak dari suatu distribusi.
[**Kita bisa membuat tensor dengan semua elemennya berisi 0**] (~~atau  1~~)
dan berbentuk (2, 3, 4) sebagai berikut:

```{.python .input}
np.zeros((2, 3, 4))
```

```{.python .input}
#@tab pytorch
torch.zeros((2, 3, 4))
```

```{.python .input}
#@tab tensorflow
tf.zeros((2, 3, 4))
```

Demikian pula, kita bisa membuat tensor dengan setiap elemen bernilai 1 sebagai berikut:

```{.python .input}
np.ones((2, 3, 4))
```

```{.python .input}
#@tab pytorch
torch.ones((2, 3, 4))
```

```{.python .input}
#@tab tensorflow
tf.ones((2, 3, 4))
```

Seringkali, kita ingin [**secara acak mengambil sampel untuk setiap elemen dalam tensor**]
dari suatu distribusi probabilitas.
Contohnya, ketika kita membuat senarai untuk menjadi parameter dalam jaringan saraf,
kita biasanya ingin menginisialisasi nilainya secara acak.
Cuplikan berikut membuat tensor dengan bentuk (3, 4).
Setiap elemen-elemennya secara random diambil dari distribusi standar Gaussian (normal)
dengan rata-rata 0 dan deviasi standar 1.

```{.python .input}
np.random.normal(0, 1, size=(3, 4))
```

```{.python .input}
#@tab pytorch
torch.randn(3, 4)
```

```{.python .input}
#@tab tensorflow
tf.random.normal(shape=[3, 4])
```

Kita juga bisa [**menyebutkan nilai pasti untuk setiap elemen**] di suatu tensor
dengan mengirimkan *list* Python (atau *list of list*) berisi nilai-nilai numerik.
Di sini, list terluar sesuai dengan sumbu 0, dan list terdalam sesuai dengan sumbu 1.

```{.python .input}
np.array([[2, 1, 4, 3], [1, 2, 3, 4], [4, 3, 2, 1]])
```

```{.python .input}
#@tab pytorch
torch.tensor([[2, 1, 4, 3], [1, 2, 3, 4], [4, 3, 2, 1]])
```

```{.python .input}
#@tab tensorflow
tf.constant([[2, 1, 4, 3], [1, 2, 3, 4], [4, 3, 2, 1]])
```

## Operasi

Buku ini bukan tentang rekayasa perangkat lunak.
Kepentingan kita tidak terbatas hanya pada
membaca dan menulis data dari/ke senarai.
Kami ingin melakukan operasi matematika pada senarai tersebut.
Beberapa operasi paling sederhana dan paling berguna
adalah operasi *per elemen* yang menerapkan operasi skalar standar
ke setiap elemen senarai.
Untuk fungsi yang menggunakan dua senarai sebagai input,
operasi per-elemen menerapkan beberapa operator biner standar
pada setiap pasangan elemen yang sesuai dari dua larik.
Kita dapat membuat fungsi per-elemen dari sembarang fungsi 
yang memetakan dari skalar ke skalar.

Dalam notasi matematika, operator skalar *unary* (satu input) seperti 
itu dengan $f: \mathbb{R} \rightarrow \mathbb{R}$.
Ini berarti fungsi ini adalah pemetaan dari sembarang bilangan riel ke 
bilangan riel lainnya.
Demikian juga, operator skalar biner (mengambil dua input, menghasilkan satu output)
dituliskan dengan $f: \mathbb{R}, \mathbb{R} \rightarrow \mathbb{R}$.
Diberikan sembarang dua vektor $\mathbf{u}$ dan $\mathbf{v}$ *berbentuk sama*,
dan operator biner $f$, kita bisa menghasilkan sebuah vektor 
$\mathbf{c} = F(\mathbf{u},\mathbf{v})$
dengan menetapkan $c_i \gets f(u_i, v_i)$ untuk semua $i$,
di mana $c_i, u_i$, dan $v_i$ adalah elemen ke-$i$
dari vektor $\mathbf{c}, \mathbf{u}$, dan $\mathbf{v}$.
Di sini, kita menghasilkan $F: \mathbb{R}^d, \mathbb{R}^d \rightarrow \mathbb{R}^d$
bernilai vektor dengan *mengangkat* fungsi skalar ke operasi vektor per-elemen.

Operator aritmatika standar yang umum 
(`+`, `-`, `*`, `/`, and `**`)
semua telah *diangkat* ke operasi per-elemen 
untuk sembarang tensor berbentuk identik.
Kita bisa memanggil operasi per-elemen untuk sembarang dua tensor berbentuk sama.
Dalam contoh berikut, kita menggunakan koma untuk memformulasikan tupel 5 elemen,
di mana setiap elemen adalah hasil dari operasi per-elemen.

### Operations

[**Operator aritmatika standar yang umum 
(`+`, `-`, `*`, `/`, and `**`)
semua telah *diangkat* ke operasi per-elemen.**]

```{.python .input}
x = np.array([1, 2, 4, 8])
y = np.array([2, 2, 2, 2])
x + y, x - y, x * y, x / y, x ** y  # Operator ** adalah pemangkatan
```

```{.python .input}
#@tab pytorch
x = torch.tensor([1.0, 2, 4, 8])
y = torch.tensor([2, 2, 2, 2])
x + y, x - y, x * y, x / y, x ** y  # Operator ** adalah pemangkatan
```

```{.python .input}
#@tab tensorflow
x = tf.constant([1.0, 2, 4, 8])
y = tf.constant([2.0, 2, 2, 2])
x + y, x - y, x * y, x / y, x ** y  # Operator ** adalah pemangkatan
```

Banyak lagi (**oeprasi dapat diterapkan secara per-elemen**),
termasuk operator *unary* seperti pemangkatan.

```{.python .input}
np.exp(x)
```

```{.python .input}
#@tab pytorch
torch.exp(x)
```

```{.python .input}
#@tab tensorflow
tf.exp(x)
```

Selain komputasi per-elemen,
kita juga bisa melakukan operasi aljabar linier,
termasuk perkalian titik vektor dan perkalian matriks.
Kami akan menjelaskan hal penting tentang aljabar linier
(pengetahuan sebelumnya tidak diperlukan) dalam :numref:`sec_linear-algebra`.

Kita juga bisa [***menggabungkan* beberapa tensor,**]
menumpuknya ujung-ke-ujung untuk membentuk tensor yang lebih besar.
Kita hanya perlu untuk memberikan senarai berisi tensor
dan memberitahu sistem melalui sumbu mana tensor-tenser tersebut 
akan digabungkan.

Dua contoh di bawah menunjukkan apa yang terjadi ketika kita menggabungkan
dua matriks sepanjang baris (sumbu 0, elemen pertama dari bentuk)
vs. kolom (sumbu 1, elemen kedua dari bentuk).
Kita bisa melihat bahwa pada tensor keluaran pertama, panjang sumbu 0 adalah $6$,
ini adalah jumlah dari panjang sumbu 0 dari kedua tensor masukan ($3 + 3$); 
Sementara pada tensor keluaran kedua, panjang sumbu 1 adalah $8$,
ini adalah jumlah dari panjang sumbu 1 dari kedua tensor masukan ($4 + 4$).

```{.python .input}
X = np.arange(12).reshape(3, 4)
Y = np.array([[2, 1, 4, 3], [1, 2, 3, 4], [4, 3, 2, 1]])
np.concatenate([X, Y], axis=0), np.concatenate([X, Y], axis=1)
```

```{.python .input}
#@tab pytorch
X = torch.arange(12, dtype=torch.float32).reshape((3,4))
Y = torch.tensor([[2.0, 1, 4, 3], [1, 2, 3, 4], [4, 3, 2, 1]])
torch.cat((X, Y), dim=0), torch.cat((X, Y), dim=1)
```

```{.python .input}
#@tab tensorflow
X = tf.reshape(tf.range(12, dtype=tf.float32), (3, 4))
Y = tf.constant([[2.0, 1, 4, 3], [1, 2, 3, 4], [4, 3, 2, 1]])
tf.concat([X, Y], axis=0), tf.concat([X, Y], axis=1)
```

Terkadang, kita ingin untuk [**membangun tensor biner dengan *pernyataan logika*.**]
Lihat `X == Y` sebagai contoh.
Pada setiap posisi, jika `X` dan `Y` sama pada posisi tersebut, maka pada posisi yang sama 
di tensor baru akan bernilai 1, yang artinya, pernyatan logika `X == Y` bernilai benar pada 
posisi tersebut; jika sebaliknya salah, maka nilainya adalah 0.

```{.python .input}
#@tab all
X == Y
```

[**Menjumlahkan semua elemen tensor**] menghasilkan tensor baru dengan hanya satu elemen.

```{.python .input}
#@tab mxnet, pytorch
X.sum()
```

```{.python .input}
#@tab tensorflow
tf.reduce_sum(X)
```
## Mekanisme Penyiaran (*Broadcasting Mechanism*)
:label:`subsec_broadcasting`

Pada bagian di atas, kita telah melihat bagaimana melakukan operasi per-elemen
pada dua tensor berbentuk sama. Pada kondisi tertentu,
meskipun bentuknya berbeda, kita masih bisa [**melakukan operasi per-elemen
dengan memanfaatkan mekanisme penyebaran.**]
Mekanisme ini bekerja dengan cara berikut:
Pertama, memperluas salah satu atau kedua senarai
dengan menyalin elemen-elemen secara tepat sehingga
setelah transformasi ini kedua tensor berbentuk sama.
Kedua, melakukan operasi per-elemen pada senara-senarai hasil transformasi tersebut.

Pada umumnya, kita menyiarkan sepanjang sumbu di mana senarai awalnya hanya punya panjang 1,
seperti pada contoh berikut:


```{.python .input}
a = np.arange(3).reshape(3, 1)
b = np.arange(2).reshape(1, 2)
a, b
```

```{.python .input}
#@tab pytorch
a = torch.arange(3).reshape((3, 1))
b = torch.arange(2).reshape((1, 2))
a, b
```

```{.python .input}
#@tab tensorflow
a = tf.reshape(tf.range(3), (3, 1))
b = tf.reshape(tf.range(2), (1, 2))
a, b
```

Karena `a` dan `b` adalah matriks berukuran $3\times1$ dan $1\times2$,
bentuknya tidak cocok untuk operasi penjumlahan.
Kita *menyiarkan* nilai-nilai dari kedua matriks menjadi matriks yang lebih besar $3\times2$ sebagai berikut:
salin kolom dari matriks `a`, dan salin baris dari matriks `b` sebelum menambahkan keduanya secara per-elemen.

```{.python .input}
#@tab all
a + b
```

## Pengindeksan dan Pemotongan

Sama seperti senarai Python biasa, elemen-elemen dalam tensor juga bisa diakses dengan indeks.
Juga sama seperti senarai Python, elemen pertama memiliki indeks 0 dan rentang indeks disebutkan
dengan mengikutkan elemen pertama tapi *sebelum* elemen terakhir.
Sama seperti senarai Python, kita bisa mengakses elemen-elemen berdasarkan posisinya relatif
terhadap akhir dari senarai dengan indeks negatif.

Jadi, [**`[-1]` memilih elemen terakhir dan `[1:3]`
memilih elemen ke-2 dan elemen ke-3**] as follows:

```{.python .input}
#@tab all
X[-1], X[1:3]
```

:begin_tab:`mxnet, pytorch`
(**Kita juga bisa menulis elemen suatu matriks dengan menyebutkan indeksnya.**)
:end_tab:

:begin_tab:`tensorflow`
`Tensor` di TensorFlow tidak dapat diubah (*immutable*), sehingga tidak bisa diberi nilai.
`Variable` di TensorFlow adalah kontainer keadaan yang mendukung penugasan nilai (*assignment*).
Perlu diingat bahwa gradien di TensorFlow tidak mengalir mundur melalui penugasan variabel.

Selain menugaskan nilai ke seluruh `Variable` sekaligus, kita juga bisa menulis elemen suatu `Variable`
dengan menyebutkan indeksnya.
:end_tab:

```{.python .input}
#@tab mxnet, pytorch
X[1, 2] = 9
X
```

```{.python .input}
#@tab tensorflow
X_var = tf.Variable(X)
X_var[1, 2].assign(9)
X_var
```

Jika kita ingin [**menugaskan beberapa elemen dengan nilai yang sama,
kita hanya perlu mengindeks semuanya dan menugaskan nilainya.**]
Contohnya, `[0:2, :]` mengakses baris pertama dan kedua,
di mana `:` mengakses semua elemen sepanjang sumbu 1 (kolom).
Meskipun kita membahas pengindeksan untuk matriks,
ini juga berlaku untuk vektor dan tensor dengan lebih dari 2 dimensi.

```{.python .input}
#@tab mxnet, pytorch
X[0:2, :] = 12
X
```

```{.python .input}
#@tab tensorflow
X_var = tf.Variable(X)
X_var[0:2, :].assign(tf.ones(X_var[0:2,:].shape, dtype = tf.float32) * 12)
X_var
```

## Menghemat Memori

[**Menjalankan operasi akan mengalokasikan memori baru untuk menyimpan hasilnya.**]
Contohnya, jika kita menulis `Y = X + Y`,
kita akan mencabut rujukan ke tensor yang sebelumnya dirujuk oleh `Y`, dan
`Y` akan merujuk ke alokasi memori baru.

Dalam contoh berikut, kita akan menunjukkan ini dengan fungsi `id()` dari Python,
yang memberikan alamat tepat sebuah obyek di memori.
Setelah menjalankan `Y = Y + X`, kita akan temukan bahwa `id(Y)` menunjuk pada lokasi yang berbeda.
Hal ini karena Python pertama mengevaluasi `Y + X`,
mengalokasikan memori baru untuk hasilnya dan kemudian membuat `Y` menunjuk ke lokasi baru ini.

```{.python .input}
#@tab all
before = id(Y)
Y = Y + X
id(Y) == before
```

Kondisi mungkin kurang diinginkan karena 2 hal.
Pertama, kita tidak ingin menghabiskan waktu untuk terus menerus mengalokasikan memori tanpa
ada keperluan.
Dalam pembelajaran mesin, kita mungkin punya ratusan *megabyte* parameter dan memutakhirkan
semuanya beberapa kali tiap detik.
Biasanya, kita ingin melakukan pemutakhiran ini langsung di-tempat (*in place*).
Kedua, kita mungkin ingin menunjuk pada parameter yang sama dari beberapa variabel.

Kedua, ada kemungkinan beberapa variabel merujuk pada satu parameter yang sama.
Jika kita tidak memutakhirkan di-tempat, variabel lain mungkin masih menunjuk pada
lokasi memori yang lama, sehingga memungkinkan terjadinya kode yang menunjuk pada
obyek yang sudah ditinggalkan.

:begin_tab:`mxnet, pytorch`
Untungnya, (**melakukan operasi di-tempat**) itu mudah.
Kita bisa menugaskan hasil suatu operasi ke suatu senarai 
yang sebelumnya sudah dialokasikan dengan notasi irisan,
contohnya, `Y[:] = <expression>`.
Untuk mengilustrasikan konsep ini, pertama kita membuat matrix baru `Z`
dengan bentuk sama seperti `Y`,
menggunakan `zeros_like` untuk mengalokasikan satu blok yang semuanya berisi $0$.
:end_tab:

:begin_tab:`tensorflow`
`Variables` adalah kontainer *keadaan* (*state*) yang bisa diubah di TensorFlow.
Variabel memberikan cara untuk menyimpan parameter model.
Kita bisa menugaskan hasil dari operasi ke `Variable` dengan `assign`.
Untuk mengilustrasikan konsep ini, kita membuat `Variable` `Z`
dengan bentuk sama seperti tensor lain `Y`,
menggunakan `zeros_like` untuk mengalokasikan satu block yang semuanya berisi $0$.
:end_tab:

```{.python .input}
Z = np.zeros_like(Y)
print('id(Z):', id(Z))
Z[:] = X + Y
print('id(Z):', id(Z))
```

```{.python .input}
#@tab pytorch
Z = torch.zeros_like(Y)
print('id(Z):', id(Z))
Z[:] = X + Y
print('id(Z):', id(Z))
```

```{.python .input}
#@tab tensorflow
Z = tf.Variable(tf.zeros_like(Y))
print('id(Z):', id(Z))
Z.assign(X + Y)
print('id(Z):', id(Z))
```

:begin_tab:`mxnet, pytorch`
[**If the value of `X` is not reused in subsequent computations,
we can also use `X[:] = X + Y` or `X += Y`
to reduce the memory overhead of the operation.**]
[**Jika nilai `X` tidak dipakai lagi di komputasi berikutnya,
kita juga bisa menggunakan `X[:] = X + Y` atau `X += Y`
untuk mengurangi pemakaian memori dari operasi ini.**]
:end_tab:

:begin_tab:`tensorflow`
Bahkan setelah anda menyimpan keadaan secara tetap dalam `Variable`, anda
mungkin ingin mengurangi pemakaian memori lebih jauh dengan menghindari
alokasi berlebihan untuk tensor yang bukan bagian dari parameter model.

Karena `Tensor` di TensorFlow tidak dapat diubah dan gradien tidak mengalir 
ke penugasan `Variable`, TensorFlow tidak menyediakan cara eksplisit untuk
menjalankan operasi individu di-tempat.

Namun, TensorFlow memberikan dekorator `tf.function` untuk membungkus komputasi
di salam graf TensorFlow yang akan dikompilasi dan dioptimasi sebelum dijalankan.
Hal ini memungkinkan TensorFlow untuk membuang nilai yang tidak dipakai, dan
untuk memakai ulang alokasi sebelumnya yang tidak lagi dipakai. Ini bisa meminimalkan
pemakaian memori dari komputasi TensorFlow.
:end_tab:

```{.python .input}
#@tab mxnet, pytorch
before = id(X)
X += Y
id(X) == before
```

```{.python .input}
#@tab tensorflow
@tf.function
def computation(X, Y):
    Z = tf.zeros_like(Y)  # Nilai yang tidak dipakai ini akan dibuang
    A = X + Y  # Alokasi akan dipakai lagi setelah tidak dibutuhkan lagi
    B = A + Y
    C = B + Y
    return C + Y

computation(X, Y)
```
## Konversi ke Obyek Python Lain

[**Konversi ke tensor NumPy**], atau sebaliknya itu mudah.
Hasil konversi tidak akan berbagi memori.
Ketidaknyamanan kecil ini sebenarnya cukup penting:
ketika anda melakukan operasi pada CPU atau GPU,
anda tidak ingin menghentikan komputasi, menunggu untuk melihat
apakah paket NumPy dari Python sedang melakukan sesuatu dengan 
potongan memori yang sama.


```{.python .input}
A = X.asnumpy()
B = np.array(A)
type(A), type(B)
```

```{.python .input}
#@tab pytorch
A = X.numpy()
B = torch.tensor(A)
type(A), type(B)
```

```{.python .input}
#@tab tensorflow
A = X.numpy()
B = tf.constant(A)
type(A), type(B)
```

Untuk (**mengubah tensor berukuran-1 ke skalar Python**),
kita bisa memanggil fungsi `item` atau fungsi bawaan Python.

```{.python .input}
a = np.array([3.5])
a, a.item(), float(a), int(a)
```

```{.python .input}
#@tab pytorch
a = torch.tensor([3.5])
a, a.item(), float(a), int(a)
```

```{.python .input}
#@tab tensorflow
a = tf.constant([3.5]).numpy()
a, a.item(), float(a), int(a)
```

## Ringkasan

* Antarmuka utama untuk menyimpan dan memanipulasi data untuk pembelajaran mendalam adalah tensor (array dimensi-$n$). Ini menyediakan berbagai fungsi termasuk operasi matematika dasar, penyebaran, pengindeksan, pemotongan, penyimpanan memori, dan konversi ke objek Python lainnya.

## Latihan

1. Jalankan kode di bagian ini. Ubahlah pernyataan kondisional `X == Y` menjadi `X < Y` atau `X > Y`, dan lihat tensor apa yang anda dapatkan.
1. Ganti dua tensor yang beroperasi per-elemen dalam mekanisme penyebaran, dengan bentuk lain, contohnya, tensor 3-dimensi. Apakah hasilnya sama seperti yang diharapkan?
