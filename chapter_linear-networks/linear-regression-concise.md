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

Now we can use `data_iter` in much the same way as we called
the `data_iter` function in :numref:`sec_linear_scratch`.
To verify that it is working, we can read and print
the first minibatch of examples.
Comparing with :numref:`sec_linear_scratch`,
here we use `iter` to construct a Python iterator and use `next` to obtain the first item from the iterator.

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
