# Dataset Klasifikasi Gambar
:label:`sec_fashion_mnist`

(~~Dataset MNIST adalah salah satu dataset yang paling banyak dipakai untuk klasifikasi gambar, namun terlalu sederhana untuk tolok ukur (*benchmark*). Kita akan memakai dataset serupa, namun lebih kompleks dataset Fashion-MNIST~~)

Salah satu dataset yang paling banyak dipakai untuk klasifikasi gambar adalan dataset MNIST :cite:`LeCun.Bottou.Bengio.ea.1998`.
Walaupun dataset ini telah menjadi *tolok ukur* cukup lama, namun bahkan model sederhana menurut standar sekarang, dapat mencapai akurasi lebih dari 95%,
menjadikannya tidak cocok untuk membedakan antara model yang lebih kuat dan lemah.
Saat ini, MNIST dipakai lebih banyak untuk tes sederhana daripada sebagai tolok ukur.

Untuk menaikkan standar sedikit, kami akan memfokuskan diskusi kita di bagian selanjutnya
pada dataset Fashion-MNIST yang secara kualitatif mirip, tetapi relatif kompleks
:cite:`Xiao.Rasul.Vollgraf.2017`, yang dirilis pada 2017.

```{.python .input}
%matplotlib inline
from d2l import mxnet as d2l
from mxnet import gluon
import sys

d2l.use_svg_display()
```

```{.python .input}
#@tab pytorch
%matplotlib inline
from d2l import torch as d2l
import torch
import torchvision
from torchvision import transforms
from torch.utils import data

d2l.use_svg_display()
```

```{.python .input}
#@tab tensorflow
%matplotlib inline
from d2l import tensorflow as d2l
import tensorflow as tf

d2l.use_svg_display()
```

## Membaca Dataset

Kita bisa [**mengunduh dan membaca dataset Fashion-MNIST ke memori dengan fungsi bawaan dari kerangka kerja.** ]

```{.python .input}
mnist_train = gluon.data.vision.FashionMNIST(train=True)
mnist_test = gluon.data.vision.FashionMNIST(train=False)
```

```{.python .input}
#@tab pytorch
# `ToTensor` mengubah data gambar dari jenis PIL ke tensor bilangan pecahan 32-bit
# Fungsi ini membagi semua angka dengan 255 sehingga semua pixel bernilai antara 0 dan 1
trans = transforms.ToTensor()
mnist_train = torchvision.datasets.FashionMNIST(
    root="../data", train=True, transform=trans, download=True)
mnist_test = torchvision.datasets.FashionMNIST(
    root="../data", train=False, transform=trans, download=True)
```

```{.python .input}
#@tab tensorflow
mnist_train, mnist_test = tf.keras.datasets.fashion_mnist.load_data()
```

Fashion-MNIST berisi gambar-gambar dari 10 kategori, setiap kategori direpresentasikan 
oleh 6000 gambar dalam dataset pelatihan dan oleh 1000 gambar dalam dataset tes.
Dataset tes digunakan untuk mengevaluasi kinerja model dan tidak untuk pelatihan.
Sehingga dataset pelatihan dan tes seluruhnya berisi 60,000 dan 10,000 gambar.

```{.python .input}
#@tab mxnet, pytorch
len(mnist_train), len(mnist_test)
```

```{.python .input}
#@tab tensorflow
len(mnist_train[0]), len(mnist_test[0])
```

Tinggi dan lebar setiap gambar masukan adalah 28 piksel masing-masing.
Perhatikan bahwa dataset ini terdiri dari gambar skala abu-abu (*grayscale*), yang jumlah kanalnya adalah 1.
Untuk singkatnya, sepanjang buku ini kami menyimpan bentuk gambar dengan tinggi $h$ lebar $w$ sebagai $h \times w$ or ($h$, $w$).

```{.python .input}
#@tab all
mnist_train[0][0].shape
```

[~~Dua fungsi utilitas untuk memvisualisasi dataset~~]

Gambar-gambar dalam Fashion-MNIST terdiri dari kategori berikut:
t-shirt, trousers, pullover, dress, coat, sandal, shirt, sneaker, bag, and ankle boot.
Fungsi berikut mengubah label indeks menjadi nama label dalam bentuk teks.

```{.python .input}
#@tab all
def get_fashion_mnist_labels(labels):  #@save
    """Return text labels for the Fashion-MNIST dataset."""
    text_labels = ['t-shirt', 'trouser', 'pullover', 'dress', 'coat',
                   'sandal', 'shirt', 'sneaker', 'bag', 'ankle boot']
    return [text_labels[int(i)] for i in labels]
```

Sekarang kita bisa membuat fungsi untuk memvisualkan contoh-contoh ini.

```{.python .input}
#@tab mxnet, tensorflow
def show_images(imgs, num_rows, num_cols, titles=None, scale=1.5):  #@save
    """Plot a list of images."""
    figsize = (num_cols * scale, num_rows * scale)
    _, axes = d2l.plt.subplots(num_rows, num_cols, figsize=figsize)
    axes = axes.flatten()
    for i, (ax, img) in enumerate(zip(axes, imgs)):
        ax.imshow(d2l.numpy(img))
        ax.axes.get_xaxis().set_visible(False)
        ax.axes.get_yaxis().set_visible(False)
        if titles:
            ax.set_title(titles[i])
    return axes
```

```{.python .input}
#@tab pytorch
def show_images(imgs, num_rows, num_cols, titles=None, scale=1.5):  #@save
    """Plot a list of images."""
    figsize = (num_cols * scale, num_rows * scale)
    _, axes = d2l.plt.subplots(num_rows, num_cols, figsize=figsize)
    axes = axes.flatten()
    for i, (ax, img) in enumerate(zip(axes, imgs)):
        if torch.is_tensor(img):
            # Tensor Image
            ax.imshow(img.numpy())
        else:
            # PIL Image
            ax.imshow(img)
        ax.axes.get_xaxis().set_visible(False)
        ax.axes.get_yaxis().set_visible(False)
        if titles:
            ax.set_title(titles[i])
    return axes
```

Ini adalah [**gambar dan labelnya**] (in text)
untuk beberapa contoh awal dari dataset pelatihan.

```{.python .input}
X, y = mnist_train[:18]

print(X.shape)
show_images(X.squeeze(axis=-1), 2, 9, titles=get_fashion_mnist_labels(y));
```

```{.python .input}
#@tab pytorch
X, y = next(iter(data.DataLoader(mnist_train, batch_size=18)))
show_images(X.reshape(18, 28, 28), 2, 9, titles=get_fashion_mnist_labels(y));
```

```{.python .input}
#@tab tensorflow
X = tf.constant(mnist_train[0][:18])
y = tf.constant(mnist_train[1][:18])
show_images(X, 2, 9, titles=get_fashion_mnist_labels(y));
```

## Membaca Tumpukan Mini (*Minibatch*)

Untuk membuat hidup kita lebih mudah ketika membaca dataset pelatihan dan tes,
kita akan menggunakan *iterator* data bawaan daripada membuat dari awal.
Ingat bawah pada setiap iterasi, pemuat data (*data loader*) 
[**membaca sebuah tumpukan mini data dengan ukuran `batch_size` setiap waktu.**]
Kita juga mengacak contoh-contoh untuk iterator data pelatihan.

```{.python .input}
batch_size = 256

def get_dataloader_workers():  #@save
    """Use 4 processes to read the data except for Windows."""
    return 0 if sys.platform.startswith('win') else 4

# `ToTensor` converts the image data from uint8 to 32-bit floating point. It
# divides all numbers by 255 so that all pixel values are between 0 and 1
transformer = gluon.data.vision.transforms.ToTensor()
train_iter = gluon.data.DataLoader(mnist_train.transform_first(transformer),
                                   batch_size, shuffle=True,
                                   num_workers=get_dataloader_workers())
```

```{.python .input}
#@tab pytorch
batch_size = 256

def get_dataloader_workers():  #@save
    """Use 4 processes to read the data."""
    return 4

train_iter = data.DataLoader(mnist_train, batch_size, shuffle=True,
                             num_workers=get_dataloader_workers())
```

```{.python .input}
#@tab tensorflow
batch_size = 256
train_iter = tf.data.Dataset.from_tensor_slices(
    mnist_train).batch(batch_size).shuffle(len(mnist_train[0]))
```

Mari kita lihat waktu yang diperlakukan untuk membaca data pelatihan.

```{.python .input}
#@tab all
timer = d2l.Timer()
for X, y in train_iter:
    continue
f'{timer.stop():.2f} sec'
```


## Menggabungkan Semuanya

Now we define [**the `load_data_fashion_mnist` function
that obtains and reads the Fashion-MNIST dataset.**]
It returns the data iterators for both the training set and validation set.
In addition, it accepts an optional argument to resize images to another shape.

Sekarang kita mendefinisikan [**fungsi `load_data_fashion_mnist` yang mendapatkan dan 
membaca dataset Fashion-MNIST.**]
Fungsi ini mengembalikan iterator data untuk data pelatihan dan data validasi.
Selain itu, fungsi ini menerima argumen opsional untuk mengubah ukuran gambar ke bentuk lain.

```{.python .input}
def load_data_fashion_mnist(batch_size, resize=None):  #@save
    """Unduh dataset Fashion-MNIST dan memuatnya ke memori."""
    dataset = gluon.data.vision
    trans = [dataset.transforms.ToTensor()]
    if resize:
        trans.insert(0, dataset.transforms.Resize(resize))
    trans = dataset.transforms.Compose(trans)
    mnist_train = dataset.FashionMNIST(train=True).transform_first(trans)
    mnist_test = dataset.FashionMNIST(train=False).transform_first(trans)
    return (gluon.data.DataLoader(mnist_train, batch_size, shuffle=True,
                                  num_workers=get_dataloader_workers()),
            gluon.data.DataLoader(mnist_test, batch_size, shuffle=False,
                                  num_workers=get_dataloader_workers()))
```

```{.python .input}
#@tab pytorch
def load_data_fashion_mnist(batch_size, resize=None):  #@save
    """Unduh dataset Fashion-MNIST dan memuatnya ke memori."""
    trans = [transforms.ToTensor()]
    if resize:
        trans.insert(0, transforms.Resize(resize))
    trans = transforms.Compose(trans)
    mnist_train = torchvision.datasets.FashionMNIST( \
        root="../data", train=True, transform=trans, download=True)
    mnist_test = torchvision.datasets.FashionMNIST( \
        root="../data", train=False, transform=trans, download=True)
    return (data.DataLoader(mnist_train, batch_size, shuffle=True,
                            num_workers=get_dataloader_workers()),
            data.DataLoader(mnist_test, batch_size, shuffle=False,
                            num_workers=get_dataloader_workers()))
```

```{.python .input}
#@tab tensorflow
def load_data_fashion_mnist(batch_size, resize=None):   #@save
    """Unduh dataset Fashion-MNIST dan memuatnya ke memori."""
    mnist_train, mnist_test = tf.keras.datasets.fashion_mnist.load_data()
    # Bagi semua angka dengan 255 sehingga semua nilai piksel
    # antara 0 dan 1, tambahkan sebuah dimensi batch di akhir.
    # Ubah label menjadi int32
    process = lambda X, y: (tf.expand_dims(X, axis=3) / 255,
                            tf.cast(y, dtype='int32'))
    resize_fn = lambda X, y: (
        tf.image.resize_with_pad(X, resize, resize) if resize else X, y)
    return (
        tf.data.Dataset.from_tensor_slices(process(*mnist_train)).batch(
            batch_size).shuffle(len(mnist_train[0])).map(resize_fn),
        tf.data.Dataset.from_tensor_slices(process(*mnist_test)).batch(
            batch_size).map(resize_fn))
```

Di bawah ini kita menguji fitur pengubahan ukuran dari fungsi `load_data_fashion_mnist`
dengan menyebutkan argumen `resize`.

```{.python .input}
#@tab all
train_iter, test_iter = load_data_fashion_mnist(32, resize=64)
for X, y in train_iter:
    print(X.shape, X.dtype, y.shape, y.dtype)
    break
```

Kita sekarang siap untuk bekerja dengan dataset Fashion-MNIST di bab-bab selanjutnya.

## Ringkasan

* Fashion_MNIST adalah dataset klasifikasi pakaian mengandung gambar-gambar dari 10 kategori. Kita akan menggunakan dataset ini dalam bab-bab selanjutnya untuk
mengevaluasi berbagai algoritma klasifikasi.
* Kita menyimpan bentuk dari sembarang gambar dengan tinggi $h$ dan lebar $w$ piksel sebagai $h \times w$ or ($h$, $w$).
* Iterator data adalah komponen kunci untuk kinerja yang sangkil (*efficient*). Andalkan iterator data yang telah diimplementasikan dengan baik yang mengeksploitasi komputasi kinerja-tinggi agar tidak memperlambat pelatihan anda.

## Latihan

1. Apakah mengurangi `batch_size` (contohnya menjadi 1) mempengaruhi kinerja pembacaan?
1. Kinerja iterator data itu penting. Apakah anda pikir implementasi saat ini cukup cepat? Eksplorasi berbagai pilihan untuk mempercepatnya.
1. Lihat dokumentasi API dari kerangka kerja. Dataset apa lagi yang tersedia?

:begin_tab:`mxnet`
[Diskusi](https://discuss.d2l.ai/t/48)
:end_tab:

:begin_tab:`pytorch`
[Diskusi](https://discuss.d2l.ai/t/49)
:end_tab:

:begin_tab:`tensorflow`
[Diskusi](https://discuss.d2l.ai/t/224)
:end_tab:
