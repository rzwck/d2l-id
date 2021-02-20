# Implementasi Ringkas Regresi Linier
:label:`sec_linear_concise`

Minat yang luas dan intens dalam pembelajaran mendalam selama beberapa tahun terakhir
telah menginspirasi perusahaan, akademisi, dan penghobi
untuk mengembangkan berbagai kerangka kerja sumber terbuka yang matang
untuk mengotomatiskan pekerjaan berulang mengimplementasikan algoritma 
pembelajaran berbasis gradien.
Dalam :numref:`sec_linear_scratch`, kita hanya mengandalkan
(i) tensor untuk penyimpanan data dan aljabar linier;
dan (ii) diferensiasi otomatis untuk menghitung gradien.
Dalam praktiknya, karena pengulang (*iterator*) data, fungsi kerugian, pengoptimasi,
dan lapisan jaringan saraf
sudah sangat umum, banyak kode pustaka modern telah mengimplementasikan 
komponen ini untuk kita juga.

Di bagian ini, (**kami akan menunjukkan cara mengimplementasikan
model regresi linier**) dari :numref:`sec_linear_scratch`
(**secara ringkas dengan menggunakan API tingkat tinggi**) dari kerangka kerja pembelajaran mendalam.

## Membangkitkan Dataset

Untuk memulai, kita akan membangkitkan dataset yang sama seperti di :numref:`sec_linear_scratch`.

```{.python .input}
from d2l import mxnet as d2l
from mxnet import autograd, gluon, np, npx
npx.set_np()
```

```{.python .input}
#@tab pytorch
from d2l import torch as d2l
import numpy as np
import torch
from torch.utils import data
```

```{.python .input}
#@tab tensorflow
from d2l import tensorflow as d2l
import numpy as np
import tensorflow as tf
```

```{.python .input}
#@tab all
true_w = d2l.tensor([2, -3.4])
true_b = 4.2
features, labels = d2l.synthetic_data(true_w, true_b, 1000)
```

## Membaca Dataset

Daripada membuat sendiri pengulang kita sendiri,
kita bisa [**memanggil API yang tersedia di kerangka kerja untuk membaca data.**]
Kita memberikan `features` dan `labels` sebagai argumen dan menyebutkan `batch_size`
ketika menginstansiasi obyek pengulang data (*data iterator object*).
Di samping itu, nilai boolean `is_train` 
menunjukkan apakah kita ingin obyek pengulang data untuk mengacak data 
pada setiap epoch (melewati seluruh dataset).

```{.python .input}
def load_array(data_arrays, batch_size, is_train=True):  #@save
    """Construct a Gluon data iterator."""
    dataset = gluon.data.ArrayDataset(*data_arrays)
    return gluon.data.DataLoader(dataset, batch_size, shuffle=is_train)
```

```{.python .input}
#@tab pytorch
def load_array(data_arrays, batch_size, is_train=True):  #@save
    """Construct a PyTorch data iterator."""
    dataset = data.TensorDataset(*data_arrays)
    return data.DataLoader(dataset, batch_size, shuffle=is_train)
```

```{.python .input}
#@tab tensorflow
def load_array(data_arrays, batch_size, is_train=True):  #@save
    """Construct a TensorFlow data iterator."""
    dataset = tf.data.Dataset.from_tensor_slices(data_arrays)
    if is_train:
        dataset = dataset.shuffle(buffer_size=1000)
    dataset = dataset.batch(batch_size)
    return dataset
```

```{.python .input}
#@tab all
batch_size = 10
data_iter = load_array((features, labels), batch_size)
```

Sekarang kita bisa memakai `data_iter` dengan cara yang sama seperti 
kita memanggil fungsi `data_iter` di :numref:`sec_linear_scratch`.
Untuk memverifikasi bahwa ini bekerja dengan baik, kita bisa membaca dan mencetak
*minibatch* pertama dari contoh-contoh.

Dibandingkan dengan :numref:`sec_linear_scratch`,
di sini kita memakai `iter` untuk membuat pengulang Python dan menggunakan `next` untuk
mendapatkan *item* pertama dari pengulang.

```{.python .input}
#@tab all
next(iter(data_iter))
```
## Mendefinisikan Model

Saat kita mengimplementasikan regresi linier dari awal 
dalam :numref:`sec_linear_scratch`,
kita mendefinisikan parameter model kita secara eksplisit
dan memprogram penghitungannya untuk menghasilkan keluaran
menggunakan operasi aljabar linier dasar.
Anda seharusnya tahu bagaimana melakukan ini.
Tapi begitu model Anda menjadi lebih kompleks,
dan begitu Anda harus mulai melakukan ini hampir setiap hari,
Anda akan senang bantuan ini.
Situasinya mirip dengan memprogram blog Anda sendiri dari awal.
Melakukannya satu atau dua kali bermanfaat dan instruktif,
tetapi Anda akan menjadi pengembang web yang buruk
jika setiap kali Anda membutuhkan blog Anda menghabiskan waktu sebulan
menciptakan kembali sesuatu yang sudah tersedia.

Untuk operasi standar, kita dapat [**menggunakan lapisan yang tersedia di kerangka kerja,**]
sehingga kita bisa fokus 
pada lapisan-lapisan yang digunakan untuk membuat model 
daripada harus fokus pada implementasi.
Pertama-tama kita akan mendefinisikan variabel model `net`,
yang akan merujuk ke instans dari kelas `Sequential`.
Kelas `Sequential` mendefinisikan sebuah wadah
untuk beberapa lapisan yang akan dirangkai bersama.
Dengan adanya data masukan, instans `Sequential` meneruskannya
ke lapisan pertama, dan pada gilirannya meneruskan keluarannya 
sebagai masukan lapisan kedua dan seterusnya.
Pada contoh berikut, model kita hanya terdiri dari satu lapisan,
jadi kita tidak terlalu membutuhkan `Sequential`.
Tapi karena hampir semua model kita nanti 
akan melibatkan banyak lapisan,
kami akan tetap menggunakannya hanya untuk membiasakan Anda
dengan alur kerja paling standar.

Ingat arsitektur jaringan lapisan tunggal seperti yang ditunjukkan di :numref:`fig_single_neuron`.
Lapisan tersebut dikatakan *terhubung-penuh* (*fully connected*)
karena setiap masukannya terhubung ke setiap keluarannya 
dengan perkalian matriks-vektor.

:begin_tab:`mxnet`
Di Gluon, lapisan terhubung-penuh didefinisikan di kelas `Dense`.
Karena kita hanya ingin menghasilkan satu keluaran skalar,
kami menetapkan argumennya menjadi 1.

Perlu dicatat bahwa, untuk kemudahan,
Gluon tidak mengharuskan kita untuk menentukan
bentuk masukan untuk setiap lapisan.
Jadi di sini, kita tidak perlu memberi tahu Gluon
berapa banyak input yang masuk ke lapisan linier ini.
Saat kita pertama kali mencoba melewatkan data melalui model kita,
misalnya, ketika kita mengeksekusi `net (X)` nanti,
Gluon secara otomatis akan menyimpulkan jumlah input untuk setiap lapisan.
Kami akan menjelaskan cara kerjanya lebih detail nanti.
:end_tab:

:begin_tab:`pytorch`
Di PyTorch, lapisan terhubung-penuh didefinisikan di kelas `Linear`. Perhatikan bahwa kita memberikan dua argumen ke dalam `nn.Linear`. Yang pertama menentukan dimensi fitur masukan, yaitu 2, dan yang kedua adalah dimensi fitur keluaran, yang merupakan skalar tunggal dan oleh karena itu 1.
:end_tab:

:begin_tab:`tensorflow`
Di Keras, lapisan terhubung-penuh didefinisikan di kelas `Dense`.
Karena kita hanya ingin menghasilkan satu keluaran skalar,
kami menetapkan argumennya menjadi 1.
Perlu dicatat bahwa, untuk kemudahan,
Keras tidak mengharuskan kita untuk menentukan
bentuk masukan untuk setiap lapisan.
Jadi di sini, kita tidak perlu memberi tahu Keras
berapa banyak input yang masuk ke lapisan linier ini.
Saat kita pertama kali mencoba melewatkan data melalui model kita,
misalnya, ketika kita mengeksekusi `net (X)` nanti,
Keras secara otomatis akan menyimpulkan jumlah input untuk setiap lapisan.
Kami akan menjelaskan cara kerjanya lebih detail nanti.
:end_tab:

```{.python .input}
# `nn` is an abbreviation for neural networks
from mxnet.gluon import nn
net = nn.Sequential()
net.add(nn.Dense(1))
```

```{.python .input}
#@tab pytorch
# `nn` is an abbreviation for neural networks
from torch import nn
net = nn.Sequential(nn.Linear(2, 1))
```

```{.python .input}
#@tab tensorflow
# `keras` is the high-level API for TensorFlow
net = tf.keras.Sequential()
net.add(tf.keras.layers.Dense(1))
```
## Inisialisasi Parameter Model

Sebelum menggunakan `net`, kita perlu (**menginisialisasi parameter model,**)
seperti bobot dan bias dalam model regresi linier.
Kerangka pembelajaran mendalam sering kali memiliki cara tertentu untuk menginisialisasi parameter.
Di sini kita menentukan bahwa setiap parameter bobot
harus diambil sampelnya secara acak dari distribusi normal
dengan rata-rata 0 dan deviasi standar 0,01.
Parameter bias akan diinisialisasi ke nol.

:begin_tab:`mxnet`
Kita akan mengimpor modul `initializer` dari MXNet.
Modul ini menyediakan berbagai metode untuk inisialisasi parameter model.
Gluon membuat `init` tersedia sebagai pintasan (singkatan)
untuk mengakses paket `initializer`.
Kita hanya menentukan cara menginisialisasi bobot dengan memanggil `init.Normal(sigma=0.01)`.
Parameter bias diinisialisasi ke nol secara bawaan.
:end_tab:

:begin_tab:`pytorch`
Kita telah menentukan dimensi masukan dan keluaran saat membangun `nn.Linear`. Sekarang kita mengakses parameter secara langsung untuk menentukan nilai awalnya. Pertama-tama kita mengakses lapisan dengan `net[0]`, yang merupakan lapisan pertama dalam jaringan, lalu menggunakan metode `weight.data` dan` bias.data` untuk mengakses parameter. Selanjutnya kita menggunakan metode `normal_` dan`isi_` untuk menimpa nilai parameter.
:end_tab:

:begin_tab:`tensorflow`
Modul `initializers` di TensorFlow menyediakan berbagai metode untuk inisialisasi parameter model. Cara termudah untuk menentukan metode inisialisasi di Keras adalah ketika membuat lapisan dengan menyebutkan `kernel_initializer`. Di sini kita membuat ulang `net` lagi.
:end_tab:

```{.python .input}
from mxnet import init
net.initialize(init.Normal(sigma=0.01))
```

```{.python .input}
#@tab pytorch
net[0].weight.data.normal_(0, 0.01)
net[0].bias.data.fill_(0)
```

```{.python .input}
#@tab tensorflow
initializer = tf.initializers.RandomNormal(stddev=0.01)
net = tf.keras.Sequential()
net.add(tf.keras.layers.Dense(1, kernel_initializer=initializer))
```

:begin_tab:`mxnet`
Kode di atas mungkin terlihat sederhana tetapi Anda harus perhatikan
bahwa sesuatu yang aneh sedang terjadi di sini.
Kita menginisialisasi parameter untuk sebuah jaringan
padahal Gluon belum tahu
berapa banyak dimensi yang akan dimilikinya!
Mungkin 2 seperti dalam contoh kita atau mungkin 2000.
Gluon membiarkan kita melakukan itu karena di balik layar,
inisialisasi sebenarnya *ditangguhkan*.
Inisialisasi yang sebenarnya hanya akan terjadi
ketika kita untuk pertama kalinya mencoba melewatkan data melalui jaringan.
Berhati-hatilah untuk mengingatnya karena parameternya
belum diinisialisasi,
jadi kita tidak dapat mengakses atau memanipulasinya.
:end_tab:

:begin_tab:`pytorch`

:end_tab:

:begin_tab:`tensorflow`
Kode di atas mungkin terlihat sederhana tetapi Anda harus perhatikan
bahwa sesuatu yang aneh sedang terjadi di sini.
Kita menginisialisasi parameter untuk sebuah jaringan
padahal Keras belum tahu
berapa banyak dimensi yang akan dimilikinya!
Mungkin 2 seperti dalam contoh kita atau mungkin 2000.
Keras membiarkan kita melakukan itu karena di balik layar,
inisialisasi sebenarnya *ditangguhkan*.
Inisialisasi yang sebenarnya hanya akan terjadi
ketika kita untuk pertama kalinya mencoba melewatkan data melalui jaringan.
Berhati-hatilah untuk mengingatnya karena parameternya
belum diinisialisasi,
jadi kita tidak dapat mengakses atau memanipulasinya.
:end_tab:


## Mendefinisikan Fungsi Kerugian

:begin_tab:`mxnet`
Di Gluon, modul `loss` mendefinisikan berbagai fungsi kerugian.
Dalam contoh ini, kita akan memakai implementasi Gluon untuk
kerugian kuadrat (*squared loss*) (`L2Loss`).
:end_tab:

:begin_tab:`pytorch`
[**Kelas `MSELoss` menghitung rata-rata kuadrat galat (*mean squared error*), disebut juga sebagai norma $L_2$ kuadrat.**]
Secara bawaan kelas ini mengembalikan rata-rata kerugian dari contoh-contoh.
:end_tab:

:begin_tab:`tensorflow`
Kelas `MeanSquaredError` menghitung rata-rata kuadrat galat (*mean squared error*), disebut juga sebagai norma $L_2$ kuadrat.
Secara bawaan kelas ini mengembalikan rata-rata kerugian dari contoh-contoh.
:end_tab:

```{.python .input}
loss = gluon.loss.L2Loss()
```

```{.python .input}
#@tab pytorch
loss = nn.MSELoss()
```

```{.python .input}
#@tab tensorflow
loss = tf.keras.losses.MeanSquaredError()
```

## Mendefinisikan Algoritma Optimasi

:begin_tab:`mxnet`
Penurunan gradien stokastik minibatch (*minibatch stochastic gradient descent*) adalah alat standar
untuk mengoptimalkan jaringan saraf 
dan dengan demikian Gluon mendukungnya bersama sejumlah
variasi algoritma ini dalam kelas `Trainer`.
Saat kita menginstansiasi `Trainer`,
kita akan menentukan parameter yang akan dioptimalkan
(dapat diperoleh dari model kita `net` melalui `net.collect_params()`),
algoritma optimasi yang ingin kita gunakan (`sgd`),
dan *dictionary* hiperparameter
yang dibutuhkan oleh algoritma optimasi kita.
Penurunan gradien stokastik minibatch hanya memerlukan 
kita menetapkan nilai `learning_rate`, yang disetel ke 0,03 di sini.
:end_tab:

:begin_tab:`pytorch`
Penurunan gradien stokastik minibatch (*minibatch stochastic gradient descent*) adalah alat standar
untuk mengoptimalkan jaringan saraf 
dan dengan demikian PyTorch mendukungnya bersama sejumlah
variasi algoritma ini dalam modul `optim`.
Saat kita menginstansiasi `SGD`,
kita akan menentukan parameter yang akan dioptimalkan
(dapat diperoleh dari model kita `net.parameters()`),
dengan sebuah *dictionary* hiperparameter
yang dibutuhkan oleh algoritma optimasi kita.
Penurunan gradien stokastik minibatch hanya memerlukan 
kita menetapkan nilai `lr`, yang disetel ke 0,03 di sini.
:end_tab:

:begin_tab:`tensorflow`
Penurunan gradien stokastik minibatch (*minibatch stochastic gradient descent*) adalah alat standar
untuk mengoptimalkan jaringan saraf 
dan dengan demikian Keras mendukungnya bersama sejumlah
variasi algoritma ini dalam modul `optimizers`.
Penurunan gradien stokastik minibatch hanya memerlukan 
kita menetapkan nilai `learning_rate`, yang disetel ke 0,03 di sini.
:end_tab:

```{.python .input}
from mxnet import gluon
trainer = gluon.Trainer(net.collect_params(), 'sgd', {'learning_rate': 0.03})
```

```{.python .input}
#@tab pytorch
trainer = torch.optim.SGD(net.parameters(), lr=0.03)
```

```{.python .input}
#@tab tensorflow
trainer = tf.keras.optimizers.SGD(learning_rate=0.03)
```

## Pelatihan

Anda mungkin telah memperhatikan bahwa mengekspresikan model melalui
API tingkat tinggi dari kerangka pembelajaran mendalam
membutuhkan sedikit baris kode.
Kita tidak harus mengalokasikan parameter secara individual,
menentukan fungsi kerugian, atau mengimplementasikan penurunan gradien stokastik minibatch.
Setelah kita mulai bekerja dengan model yang jauh lebih kompleks,
keuntungan dari API tingkat tinggi akan makin berkembang.
Namun, setelah kita memiliki semua bagian-bagian dasar,
[**bagian iterasi pelatihan sangat mirip
dengan apa yang sudah kita lakukan saat mengimplementasikan semuanya dari awal.**]

Untuk menyegarkan ingatan Anda: untuk beberapa *epoch*,
kita akan menyelesaikan seluruh dataset (`train_data`),
secara berulang mengambil satu minibatch masukan 
beserta labelnya.
Untuk setiap minibatch, kita melakukan ritual berikut:

* Bangkitkan prediksi dengan memanggil `net(X)`  dan menghitung kerugian (`l`) (propagasi maju).
* Hitung gradien dengan menjalankan propagasi mundur.
* Mutakhirkan parameter model dengan memanggil pengoptimal.

Selain itu, kita juga menghitung kerugian setelah setiap epoch dan menampilkannya untuk mengamati kemajuannya.

```{.python .input}
num_epochs = 3
for epoch in range(num_epochs):
    for X, y in data_iter:
        with autograd.record():
            l = loss(net(X), y)
        l.backward()
        trainer.step(batch_size)
    l = loss(net(features), labels)
    print(f'epoch {epoch + 1}, loss {l.mean().asnumpy():f}')
```

```{.python .input}
#@tab pytorch
num_epochs = 3
for epoch in range(num_epochs):
    for X, y in data_iter:
        l = loss(net(X) ,y)
        trainer.zero_grad()
        l.backward()
        trainer.step()
    l = loss(net(features), labels)
    print(f'epoch {epoch + 1}, loss {l:f}')
```

```{.python .input}
#@tab tensorflow
num_epochs = 3
for epoch in range(num_epochs):
    for X, y in data_iter:
        with tf.GradientTape() as tape:
            l = loss(net(X, training=True), y)
        grads = tape.gradient(l, net.trainable_variables)
        trainer.apply_gradients(zip(grads, net.trainable_variables))
    l = loss(net(features), labels)
    print(f'epoch {epoch + 1}, loss {l:f}')
```

Below, we [**compare the model parameters learned by training on finite data
and the actual parameters**] that generated our dataset.
To access parameters,
we first access the layer that we need from `net`
and then access that layer's weights and bias.
As in our from-scratch implementation,
note that our estimated parameters are
close to their ground-truth counterparts.


Di bawah ini, kita [**membandingkan parameter model yang dipelajari dari pelatihan 
dan parameter model yang sebenarnya**] yang menghasilkan dataset kita.
Untuk mengakses parameter,
pertama kita akses layer yang kita butuhkan dari `net`
dan kemudian mengakses bobot dan bias lapisan itu.
Sama seperti implementasi dari awal,
perhatikan bahwa estimasi parameter kita 
dekat dengan nilai yang sebenarnya.


```{.python .input}
w = net[0].weight.data()
print(f'error in estimating w: {true_w - d2l.reshape(w, true_w.shape)}')
b = net[0].bias.data()
print(f'error in estimating b: {true_b - b}')
```

```{.python .input}
#@tab pytorch
w = net[0].weight.data
print('error in estimating w:', true_w - d2l.reshape(w, true_w.shape))
b = net[0].bias.data
print('error in estimating b:', true_b - b)
```

```{.python .input}
#@tab tensorflow
w = net.get_weights()[0]
print('error in estimating w', true_w - d2l.reshape(w, true_w.shape))
b = net.get_weights()[1]
print('error in estimating b', true_b - b)
```

## Ringkasan

:begin_tab:`mxnet`
* Menggunakan Gluon, kita bisa mengimplementasikan model dengan jauh lebih ringkas.
* Dalam Gluon, modul `data` menyediakan alat untuk mengolah data, modul `nn` mendefinisikan banyak lapis jaringan saraf, dan modul `loss` mendefinisikan
banyak fungsi kerugian yang umum dipakai.
* Modul `initializer` dari MXNet menyediakan berbagai metode untuk inisialisasi parameter model.
* Dimensionalitas dan penyimpanan bisa secara otomatis ditentukan, namun berhati-hatilah untuk tidak mencoba untuk mengakses paremeter sebelum
parameter tersebut diinisialisasi.
:end_tab:

:begin_tab:`pytorch`
* Menggunakan API tingkat-tinggi dari PyTorch, kita bisa mengimplementasikan model dengan jauh lebih ringkas.
* Dalam PyTorch, modul `data` menyediakan alat untuk mengolah data, modul `nn` mendefinisikan banyak lapis jaringan saraf dan fungsi kerugian yang umum.
* Kita bisa menginisialisasikan parameter dengan mengganti nilai-nilainya dengan metode yang berakhiran `_`.
:end_tab:

:begin_tab:`tensorflow`
* Menggunakan API tingkat-tinggi dari TensorFlow, kita bisa mengimplementasikan model dengan jauh lebih ringkas.
* Dalam PyTorch, modul `data` menyediakan alat untuk mengolah data, modul `keras` mendefinisikan banyak lapis jaringan saraf dan fungsi kerugian yang umum.
* Modul `initializer` dari TensorFlow menyediakan berbagai metode untuk inisialisasi parameter model.
* Dimensionalitas dan penyimpanan bisa secara otomatis ditentukan, namun berhati-hatilah untuk tidak mencoba untuk mengakses paremeter sebelum
parameter tersebut diinisialisasi.
:end_tab:

## Latihan

:begin_tab:`mxnet`
1. Jika kita mengganti `l = loss(output, y)` dengan `l = loss(output, y).mean()`, kita perlu mengubah `trainer.step(batch_size)` menjadi `trainer.step(1)` agar perilaku keduanya identik, mengapa?
1. Tinjau dokumentasi MXNet untuk melihat apa saja fungsi kerugian dan metode inisialisasi yang disediakan modul `gluon.loss` dan `init`. Ganti kerugiannya dengan kerugian Huber (*Huber's loss*)
1. Bagaimana anda mengakses gradien dari `dense.weight` ?

[Diskusi](https://discuss.d2l.ai/t/44)
:end_tab:

:begin_tab:`pytorch`
1. Jika kita mengganti `nn.MSELoss(reduction='sum')` dengan `nn.MSELoss()`, bagaimana seharusnya kita mengubah *learning rate* agar perilaku kedua kode identik. Mengapa?
1. Tinjau dokumentasi PyTorch untuk melihat apa saja fungsi kerugian dan metode inisialisasi yang tersedia. Ganti kerugiannya dengan kerugian Huber (*Huber's loss*)
1. Bagaimana anda mengakses gradien dari `net[0].weight`?

[Diskusi](https://discuss.d2l.ai/t/45)
:end_tab:

:begin_tab:`tensorflow`
1. Tinjau dokumentasi TensorFlow untuk melihat apa saja fungsi kerugian dan metode inisialisasi yang tersedia. Ganti kerugiannya dengan kerugian Huber (*Huber's loss*)

[Diskusi](https://discuss.d2l.ai/t/204)
:end_tab:
