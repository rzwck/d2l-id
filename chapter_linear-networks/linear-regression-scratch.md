# Implementasi Regresi Linier dari Awal
:label:`sec_linear_scratch`

Sekarang setelah Anda memahami gagasan utama di balik regresi linier,
kita dapat mulai mengimplementasikannya langsung dalam kode.
Di bagian ini, (**kita akan menerapkan seluruh metode dari awal,
termasuk jalur data, model,
fungsi kerugian, dan pengoptimal penurunan gradien stokastik minibatch.**)
Meskipun kerangka kerja pembelajaran mendalam modern dapat mengotomatiskan hampir semua pekerjaan ini,
menerapkan sesuatu dari awal adalah satu-satunya cara
untuk memastikan bahwa Anda benar-benar tahu apa yang Anda lakukan.
Selain itu, ketika tiba waktunya untuk menyesuaikan model,
mendefinisikan lapisan atau fungsi kerugian kita sendiri,
memahami bagaimana segala sesuatunya bekerja di balik layar akan terbukti berguna.
Pada bagian ini, kita hanya akan mengandalkan tensor dan diferensiasi otomatis.
Setelah itu, kami akan memperkenalkan implementasi yang lebih ringkas,
memanfaatkan fitur-fitur dari kerangka pembelajaran mendalam.

```{.python .input}
%matplotlib inline
from d2l import mxnet as d2l
from mxnet import autograd, np, npx
import random
npx.set_np()
```

```{.python .input}
#@tab pytorch
%matplotlib inline
from d2l import torch as d2l
import torch
import random
```

```{.python .input}
#@tab tensorflow
%matplotlib inline
from d2l import tensorflow as d2l
import tensorflow as tf
import random
```

## Membangkitkan Dataset

Untuk menyederhanakan, kami akan [**membuat kumpulan data buatan
menurut model linier dengan derau aditif.**]
Tugas kita adalah menemukan kembali parameter model ini
menggunakan kumpulan contoh terbatas yang terdapat dalam kumpulan data kita.
Kita akan memastikan data berdimensi rendah sehingga kita dapat memvisualisasikannya dengan mudah.
Dalam potongan kode berikut, kita menghasilkan dataset
berisi 1000 contoh yang masing-masing terdiri dari 2 fitur
yang diambil dari distribusi normal standar.
Jadi dataset sintetis kita akan menjadi matriks
$\mathbf{X}\in \mathbb{R}^{1000 \times 2}$.

(**Parameter yang sebenarnya yang membangkitkan dataset kita adalah 
$\mathbf{w} = [2, -3.4]^\top$ dan $b = 4.2$,
dan**) label sintetis kita akan diberikan nilai tergantung
dari model linier berikut dengan derau $\epsilon$:

(**$$\mathbf{y}= \mathbf{X} \mathbf{w} + b + \mathbf\epsilon.$$**)

You could think of $\epsilon$ as capturing potential
measurement errors on the features and labels.
We will assume that the standard assumptions hold and thus
that $\epsilon$ obeys a normal distribution with mean of 0.
To make our problem easy, we will set its standard deviation to 0.01.
The following code generates our synthetic dataset.

Anda dapat menganggap $\epsilon$ seperti akibat dari  
kesalahan pengukuran pada fitur dan label.
Kita akan berasumsi bahwa asumsi standar berlaku dan karenanya
$\epsilon$ mematuhi distribusi normal dengan rata-rata 0.
Untuk mempermudah masalah, kita akan menetapkan standar deviasi ke 0,01.
Kode berikut menghasilkan dataset sintetis kita.


```{.python .input}
#@tab mxnet, pytorch
def synthetic_data(w, b, num_examples):  #@save
    """Bangkitkan y = Xw + b + derau."""
    X = d2l.normal(0, 1, (num_examples, len(w)))
    y = d2l.matmul(X, w) + b
    y += d2l.normal(0, 0.01, y.shape)
    return X, d2l.reshape(y, (-1, 1))
```

```{.python .input}
#@tab tensorflow
def synthetic_data(w, b, num_examples):  #@save
    """Bangkitkan y = Xw + b + derau."""
    X = d2l.zeros((num_examples, w.shape[0]))
    X += tf.random.normal(shape=X.shape)
    y = d2l.matmul(X, tf.reshape(w, (-1, 1))) + b
    y += tf.random.normal(shape=y.shape, stddev=0.01)
    y = d2l.reshape(y, (-1, 1))
    return X, y
```

```{.python .input}
#@tab all
true_w = d2l.tensor([2, -3.4])
true_b = 4.2
features, labels = synthetic_data(true_w, true_b, 1000)
```

Perhatikan bahwa [**setiap baris di `features` terdiri dari contoh data 2-dimensi
dan bahwa setiap baris di `labels` terdiri dari nilai label 1-dimensi (skalar).**]

```{.python .input}
#@tab all
print('features:', features[0],'\nlabel:', labels[0])
```

Dengan membuat *scatter plot* menggunakan fitur kedua `features[:, 1]` dan `labels`,
kita dapat dengan jelas mengamati korelasi linier antara keduanya.

```{.python .input}
#@tab all
d2l.set_figsize()
d2l.plt.scatter(d2l.numpy(features[:, 1]), d2l.numpy(labels), 1);
```

## Membaca Dataset

Ingatlah bahwa melatih model terdiri dari
melintasi dataset beberapa kali, 
mengambil satu minibatch contoh dalam satu waktu,
dan menggunakannya untuk memutakhirkan model kita.
Karena proses ini sangat mendasar
untuk melatih algoritma pembelajaran mesin,
ada baiknya mendefinisikan fungsi utilitas 
untuk mengacak kumpulan data dan mengaksesnya dalam minibatch.

Pada kode berikut, kita [**mendefinisikan fungsi `data_iter`**] (~~that~~)
untuk mendemonstrasikan satu cara implementasi fungsi ini.
Fungsi ini (**mengambil ukuran batch, matriks fitur,
dan vektor label, menghasilkan minibatch dengan ukuran `batch_size`.**)
Setiap minibatch terdiri dari tupel fitur dan label.


```{.python .input}
#@tab mxnet, pytorch
def data_iter(batch_size, features, labels):
    num_examples = len(features)
    indices = list(range(num_examples))
    # Contoh-contoh ini dibaca acak, tidak ada urutan tertentu
    random.shuffle(indices)
    for i in range(0, num_examples, batch_size):
        batch_indices = d2l.tensor(
            indices[i: min(i + batch_size, num_examples)])
        yield features[batch_indices], labels[batch_indices]
```

```{.python .input}
#@tab tensorflow
def data_iter(batch_size, features, labels):
    num_examples = len(features)
    indices = list(range(num_examples))
    # Contoh-contoh ini dibaca acak, tidak ada urutan tertentu
    random.shuffle(indices)
    for i in range(0, num_examples, batch_size):
        j = tf.constant(indices[i: min(i + batch_size, num_examples)])
        yield tf.gather(features, j), tf.gather(labels, j)
```

Secara umum, perhatikan bahwa kita ingin menggunakan minibatch berukuran wajar
untuk memanfaatkan GPU,
yang unggul dalam operasi paralel.
Karena setiap contoh dapat dimasukkan melalui model kita secara paralel
dan gradien fungsi kerugian untuk setiap contoh juga dapat diambil secara paralel,
GPU memungkinkan kita memproses ratusan contoh dalam waktu yang sedikit lebih lama 
dengan waktu yang diperlukan untuk memproses hanya satu contoh.

Untuk membangun beberapa intuisi, mari kita membaca dan mencetak
*batch* kecil contoh data pertama.
Bentuk fitur di setiap *minibatch* memberi tahu kita 
ukuran *minibatch* maupun jumlah fitur masukan.
Demikian pula, *minibatch* label kita akan memiliki bentuk yang 
ditentukan oleh `batch_size`.

```{.python .input}
#@tab all
batch_size = 10

for X, y in data_iter(batch_size, features, labels):
    print(X, '\n', y)
    break
```

Saat kita menjalankan iterasi, kita mendapat *minibatch* yang berbeda
berturut-turut hingga seluruh *dataset* telah diambil (coba ini).
Sementara iterasi yang diimplementasikan di atas bagus untuk tujuan didaktik,
ini tidak sangkil dan mungkin akan membuat kita mendapat masalah dalam masalah nyata.
Misalnya, kita harus memuat semua data ke dalam memori
dan kita melakukan banyak akses memori acak.
*Iterator* bawaan diimplementasikan dalam kerangka pembelajaran mendalam
jauh lebih sangkil dan dapat menangani
data yang disimpan dalam file maupun data yang dimasukkan melalui aliran data.

## Inisialisasi Parameter Model

[**Sebelum kita dapat mulai mengoptimalkan parameter model kita**] dengan penurunan gradien stokastik *minibatch*,
(**kita harus memiliki beberapa parameter terlebih dahulu.**)
Dalam kode berikut, kita menginisialisasi bobot dengan 
angka acak yang diambil dari distribusi normal dengan rata-rata 0
dan deviasi standar 0,01, dan menyetel bias ke 0.

```{.python .input}
w = np.random.normal(0, 0.01, (2, 1))
b = np.zeros(1)
w.attach_grad()
b.attach_grad()
```

```{.python .input}
#@tab pytorch
w = torch.normal(0, 0.01, size=(2,1), requires_grad=True)
b = torch.zeros(1, requires_grad=True)
```

```{.python .input}
#@tab tensorflow
w = tf.Variable(tf.random.normal(shape=(2, 1), mean=0, stddev=0.01),
                trainable=True)
b = tf.Variable(tf.zeros(1), trainable=True)
```

Setelah menginisialisasi parameter,
tugas kita selanjutnya adalah memutakhirkannya sampai
parameter-parameter tersebut cukup cocok dengan data kita.
Setiap pemutakhiran membutuhkan pengambilan gradien
fungsi kerugian kita terhadap parameter.
Dengan adanya gradien ini, kita dapat memutakhirkan setiap parameter
ke arah yang dapat mengurangi kerugian.

Karena tidak ada yang mau menghitung gradien secara eksplisit
(ini membosankan dan rawan kesalahan),
kita menggunakan diferensiasi otomatis,
seperti yang diperkenalkan di :numref:`sec_autograd`, untuk menghitung gradien.

## Mendefinisikan Model

Selanjutnya, kita harus [**mendefinisikan model kita,
menghubungkan masukan dan parameternya dengan keluarannya.**]
Ingatlah bahwa untuk menghitung keluaran dari model linier,
kita hanya mengambil perkalian titik matriks-vektor
fitur masukan $\mathbf{X}$ dan bobot model $\mathbf{w}$,
dan menambahkan offset $b$ ke setiap contoh.
Perhatikan di bahwa ini, $\mathbf{Xw}$ adalah vektor dan $b$ adalah skalar.
Ingat kembali mekanisme penyebaran seperti yang dijelaskan di :numref:`subsec_broadcasting`.
Saat kita menambahkan vektor dan skalar,
skalar tersebut ditambahkan ke setiap komponen vektor.

```{.python .input}
#@tab all
def linreg(X, w, b):  #@save
    """Model regresi linier."""
    return d2l.matmul(X, w) + b
```

## Mendefinisikan Fungsi Kerugian

Karena [**memutakhirkan model membutuhkan pengambilan
gradien fungsi kerugian,**]
kita harus (**mendefinisikan fungsi kerugian terlebih dahulu.**)
Di sini kita akan menggunakan fungsi kerugian kuadrat
seperti yang dijelaskan dalam :numref:`sec_linear_regresi`.
Dalam implementasinya, kita perlu mengubah nilai sebenarnya `y`
ke dalam bentuk nilai prediksi `y_hat`.
Hasil yang dikembalikan oleh fungsi berikut
juga akan memiliki bentuk yang sama dengan `y_hat`.

```{.python .input}
#@tab all
def squared_loss(y_hat, y):  #@save
    """Kerugian kuadrat."""
    return (y_hat - d2l.reshape(y, y_hat.shape)) ** 2 / 2
```

## Mendefinisikan Algoritma Optimasi

Seperti yang kita diskusikan di :numref:`sec_linear_regresi`,
regresi linier memiliki solusi bentuk tertutup.
Namun, ini bukan buku tentang regresi linier:
ini adalah buku tentang pembelajaran mendalam.
Karena tidak ada model lain yang diperkenalkan buku ini
yang dapat diselesaikan secara analitis, kami akan menggunakan kesempatan ini untuk memperkenalkan contoh pertama anda dari  
penurunan gradien stokastik minibatch.
[~~Meskipun regresi linier memiliki solusi bentuk tertutup, model lain dalam buku ini tidak. Di sini kami memperkenalkan penurunan gradien stokastik minibatch. ~~]

At each step, using one minibatch randomly drawn from our dataset,
we will estimate the gradient of the loss with respect to our parameters.
Next, we will update our parameters
in the direction that may reduce the loss.
The following code applies the minibatch stochastic gradient descent update,
given a set of parameters, a learning rate, and a batch size.
The size of the update step is determined by the learning rate `lr`.
Because our loss is calculated as a sum over the minibatch of examples,
we normalize our step size by the batch size (`batch_size`),
so that the magnitude of a typical step size
does not depend heavily on our choice of the batch size.

Di setiap langkah, menggunakan satu minibatch yang diambil secara acak dari dataset kita,
kita akan memperkirakan gradien kerugian terhadap parameter kita.
Selanjutnya, kita akan memutakhirkan parameter kita 
ke arah yang dapat mengurangi kerugian.
Kode berikut menerapkan pemutakhiran penurunan gradien stokastik minibatch,
berdasarkan sekumpulan parameter, *learning rate*, dan ukuran *batch*.
Ukuran langkah pemutakhiran ditentukan oleh *learning rate* `lr`.
Karena kerugian kita dihitung sebagai jumlah dari minibatch contoh,
kita menormalkan ukuran langkah kita dengan ukuran batch (`batch_size`),
sehingga besarnya ukuran langkah 
tidak sangat bergantung pada pilihan ukuran batch kita.

```{.python .input}
def sgd(params, lr, batch_size):  #@save
    """Minibatch stochastic gradient descent."""
    for param in params:
        param[:] = param - lr * param.grad / batch_size
```

```{.python .input}
#@tab pytorch
def sgd(params, lr, batch_size):  #@save
    """Minibatch stochastic gradient descent."""
    with torch.no_grad():
        for param in params:
            param -= lr * param.grad / batch_size
            param.grad.zero_()
```

```{.python .input}
#@tab tensorflow
def sgd(params, grads, lr, batch_size):  #@save
    """Minibatch stochastic gradient descent."""
    for param, grad in zip(params, grads):
        param.assign_sub(lr*grad/batch_size)
```

## Pelatihan

Sekarang kita memiliki semua bagian,
kita siap untuk [**mengimplementasikan loop pelatihan utama.**]
Sangat penting bagi Anda untuk memahami kode ini
karena Anda akan melihat loop pelatihan yang hampir identik
berulang kali sepanjang karier Anda dalam pembelajaran mendalam.

Di setiap iterasi, kita akan mengambil *minibatch* contoh pelatihan,
dan meneruskannya melalui model kami untuk mendapatkan sekumpulan prediksi.
Setelah menghitung kerugian, kita memulai jalur mundur melalui jaringan,
menyimpan gradien terhadap setiap parameter.
Terakhir, kita akan memanggil algoritma optimasi `sgd`
untuk memutakhirkan parameter model.

Ringkasnya, kita akan mengeksekusi *loop* berikut:

* Inisialisasi parameter $(\mathbf{w}, b)$
* Ulangi sampai selesai
    * Hitung gradien $\mathbf{g} \leftarrow \partial_{(\mathbf{w},b)} \frac{1}{|\mathcal{B}|} \sum_{i \in \mathcal{B}} l(\mathbf{x}^{(i)}, y^{(i)}, \mathbf{w}, b)$
    * Mutakhirkan parameter $(\mathbf{w}, b) \leftarrow (\mathbf{w}, b) - \eta \mathbf{g}$

In each *epoch*,
we will iterate through the entire dataset
(using the `data_iter` function) once
passing through every example in the training dataset
(assuming that the number of examples is divisible by the batch size).
The number of epochs `num_epochs` and the learning rate `lr` are both hyperparameters,
which we set here to 3 and 0.03, respectively.
Unfortunately, setting hyperparameters is tricky
and requires some adjustment by trial and error.
We elide these details for now but revise them
later in
:numref:`chap_optimization`.

Di setiap *epoch*,
kita akan melakukan iterasi melalui seluruh dataset
(menggunakan fungsi `data_iter`) sekali, 
melewati setiap contoh dalam set data pelatihan
(dengan asumsi bahwa jumlah contoh habis dibagi oleh ukuran batch).
Jumlah epoch `num_epochs` dan *learning rate* `lr` merupakan hiperparameter,
yang kita tetapkan di sini masing-masing menjadi 3 dan 0,03.
Sayangnya, menyetel hiperparameter itu sulit 
dan membutuhkan beberapa penyesuaian melalui cara coba-coba.
Kami tidak membahas detail ini untuk saat ini, tetapi kami akan merevisinya
nanti :numref:`chap_optimization`.

```{.python .input}
#@tab all
lr = 0.03
num_epochs = 3
net = linreg
loss = squared_loss
```

```{.python .input}
for epoch in range(num_epochs):
    for X, y in data_iter(batch_size, features, labels):
        with autograd.record():
            l = loss(net(X, w, b), y)  # Minibatch loss in `X` and `y`
        # Karena `l` berbentuk (`batch_size`, 1) dan bukan variabel skalar 
        # elemen dalam `l` dijumlahkan bersama untuk mendapatkan variabel baru,
        # di mana gradien terhadap [`w`, `b`] akan dihitung
        l.backward()
        sgd([w, b], lr, batch_size)  # Mutakhirkan parameter dengan gradiennya
    train_l = loss(net(features, w, b), labels)
    print(f'epoch {epoch + 1}, loss {float(train_l.mean()):f}')
```

```{.python .input}
#@tab pytorch
for epoch in range(num_epochs):
    for X, y in data_iter(batch_size, features, labels):
        l = loss(net(X, w, b), y)  # Minibatch loss in `X` and `y`
        # Hitung gradien dari `l` terhadap [`w`, `b`]
        l.sum().backward()
        sgd([w, b], lr, batch_size)  # Mutakhirkan parameter dengan gradiennya
    with torch.no_grad():
        train_l = loss(net(features, w, b), labels)
        print(f'epoch {epoch + 1}, loss {float(train_l.mean()):f}')
```

```{.python .input}
#@tab tensorflow
for epoch in range(num_epochs):
    for X, y in data_iter(batch_size, features, labels):
        with tf.GradientTape() as g:
            l = loss(net(X, w, b), y)  # Minibatch loss in `X` and `y`
        # Hitung gradien dari `l` terhadap [`w`, `b`]
        dw, db = g.gradient(l, [w, b])
        # Mutakhirkan parameter dengan gradiennya
        sgd([w, b], [dw, db], lr, batch_size)
    train_l = loss(net(features, w, b), labels)
    print(f'epoch {epoch + 1}, loss {float(tf.reduce_mean(train_l)):f}')
```

Dalam hal ini, karena kita sendiri yang membuat datasetnya,
kita tahu persis apa parameter yang sebenarnya.
Dengan demikian, kita dapat [**mengevaluasi keberhasilan kita dalam pelatihan
dengan membandingkan parameter yang sebenarnya
dengan yang telah kita pelajari**] melalui *loop* pelatihan kita.
Memang keduanya ternyata sangat dekat.


```{.python .input}
#@tab all
print(f'error in estimating w: {true_w - d2l.reshape(w, true_w.shape)}')
print(f'error in estimating b: {true_b - b}')
```

Perhatikan bahwa kita tidak boleh menerima begitu saja
bahwa kita dapat menemukan kembali parameter dengan sempurna.
Namun, dalam pembelajaran mesin, kita biasanya tidak terlalu peduli
dengan menemukan kembali parameter yang sebenarnya,
dan lebih peduli dengan parameter yang mengarah pada prediksi yang sangat akurat.
Untungnya, bahkan pada masalah pengoptimalan yang sulit,
penurunan gradien stokastik sering kali dapat menemukan solusi yang sangat bagus,
sebagian karena fakta bahwa, untuk jaringan yang dalam,
ada banyak konfigurasi parameter
yang mengarah pada prediksi yang sangat akurat.


## Ringkasan

* Kita melihat bagaimana jaringan mendalam dapat diimplementasikan dan dioptimasi dari awal, dengan hanya menggunakan tensor dan diferensiasi otomatis, tanpa melibatkan lapisan atau pengoptimal rumit.

* Bagian ini hanya membahas permukaan dari kemampuan yang sebenarnya. Di bagian berikutnya, kami akan menjelaskan model-model lain berdasarkan konsep-konsep yang telah diperkenalkan dan telah dipelajari implementasinya secara ringkas.

## Latihan


1. Apa yang akan terjadi bila kita menginisialisasi bobot ke nol. Apakah algoritma ini masih akan bekerja?
1. Asumsikan bahwa Anda adalah [Georg Simon Ohm](https://en.wikipedia.org/wiki/Georg_Ohm) yang mencoba untuk membuat 
   model antara voltase dan arus. Dapatkah anda memakai diferensiasi otomatis untuk mempelajari parameter model anda?
1. Dapatkah anda menggunakan [Planck's Law](https://en.wikipedia.org/wiki/Planck%27s_law) untuk menentukan suhu obyek menggunakan densitas energi spektral?
1. Apa masalah yang mungkin anda temui jika anda ingin menghitung turunan kedua? Bagaimana anda memperbaikinya?
1. Mengapa fungsi `reshape` diperlukan dalam fungsi `squared_loss`?
1. Lakukan eksperimen menggunakan *learning rate* yang berbeda-beda untuk menemukan seberapa cepat nilai fungsi kerugian turun.
1. Jika jumlah contoh tidak bisa dibagi dengan ukuran *batch*, apa yang terjadi pada perilaku fungsi `data_iter` ?

:begin_tab:`mxnet`
[Diskusi](https://discuss.d2l.ai/t/42)
:end_tab:

:begin_tab:`pytorch`
[Diskusi](https://discuss.d2l.ai/t/43)
:end_tab:

:begin_tab:`tensorflow`
[Diskusi](https://discuss.d2l.ai/t/201)
:end_tab:

