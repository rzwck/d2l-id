# Diferensiasi Otomatis
:label:`sec_autograd`

Seperti yang telah kami jelaskan di :numref:`sec_calculus`,
diferensiasi adalah langkah penting di hampir semua algoritma optimasi pembelajaran mendalam.
Meskipun kalkulasi untuk mengambil turunan ini sangat mudah,
hanya membutuhkan beberapa kalkulus dasar,
untuk model yang kompleks, mengerjakan pembaruan bobot dengan tangan
bisa menyebalkan (dan seringkali rawan kesalahan).

*Framework* pembelajaran mendalam mempercepat pekerjaan ini
dengan menghitung turunan secara otomatis dengan *diferensiasi otomatis*.
Prakteknya,
berdasarkan model yang kita rancang
sistem membangun *grafik komputasi*,
melacak data mana yang digabungkan
dengan operasi mana untuk menghasilkan output.
Diferensiasi otomatis memungkinkan sistem untuk melakukan propagasi mundur (*backpropagation*) selanjutnya.
Di sini, *backpropagate* berarti melacak melalui grafik komputasi,
mengisi turunan parsial terhadap setiap parameter.

```{.python .input}
from mxnet import autograd, np, npx
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

## Contoh Sederhana

Sebagai contoh mainan, katakan bahwa kita tertarik
untuk (**menurunkan fungsi
$y = 2\mathbf{x}^{\top}\mathbf{x}$
terhadap vektor kolom $\mathbf{x}$.**)
Untuk memulai, mari kita buat variabel `x` dan tetapkan nilai awalnya.

```{.python .input}
x = np.arange(4.0)
x
```

```{.python .input}
#@tab pytorch
x = torch.arange(4.0)
x
```

```{.python .input}
#@tab tensorflow
x = tf.range(4, dtype=tf.float32)
x
```

[**Bahkan sebelum kita menghitung gradien
dari $y$ terhadap $\mathbf{x}$,
kita akan membutuhkan tempat untuk menyimpannya.**]
Penting agar kita tidak mengalokasikan memori baru
setiap kali kita mengambil turunan terhadap suatu parameter
karena kita akan sering mengupdate parameter yang sama
ribuan atau jutaan kali
dan akan cepat kehabisan memori.
Perhatikan bahwa gradien dari fungsi skalar
terhadap vektor $\mathbf{x}$ bernilai vektor dan berbentuk sama dengan $\mathbf{x}$.

```{.python .input}
# Kita alokasikan memori untuk gradien tensor dengan `attach_grad`
x.attach_grad()
# Setelah kita hitung gradient terhadap `x`, kita akan bisa mengaksesnya
# melalui atribut `grad` yang nilai awalnya 0
x.grad
```

```{.python .input}
#@tab pytorch
x.requires_grad_(True)  # Sama dengan `x = torch.arange(4.0, requires_grad=True)`
x.grad  # Nilai default adalah None
```

```{.python .input}
#@tab tensorflow
x = tf.Variable(x)
```

(**Sekarang mari kita hitung $y$.**)

```{.python .input}
# Tulis kode kita di dalam cakupan `autograd.record` untuk membuat grafik komputasi
with autograd.record():
    y = 2 * np.dot(x, x)
y
```

```{.python .input}
#@tab pytorch
y = 2 * torch.dot(x, x)
y
```

```{.python .input}
#@tab tensorflow
# Rekam semua komputasi dalam tape
with tf.GradientTape() as t:
    y = 2 * tf.tensordot(x, x, axes=1)
y
```

Karena `x` adalah vektor dengan panjang 4,
perlu dihitung *inner product* dari `x` dan `x`,
menghasilkan keluaran skalar yang disimpan di `y`.
Kemudian, [**Kita bisa menghitung secara otomatis gradien `y`
terhadap semua komponen dari`x`**]
dengan memanggil fungsi untuk *backpropagation* dan menampilkan gradien.

```{.python .input}
y.backward()
x.grad
```

```{.python .input}
#@tab pytorch
y.backward()
x.grad
```

```{.python .input}
#@tab tensorflow
x_grad = t.gradient(y, x)
x_grad
```


(**Gradien dari fungsi $y = 2\mathbf{x}^{\top}\mathbf{x}$
terhadap $\mathbf{x}$ adalah $4\mathbf{x}$.**)
Mari kita memverifikasi bahwa gradien yang kita inginkan berhasil dihitung dengan benar.

```{.python .input}
x.grad == 4 * x
```

```{.python .input}
#@tab pytorch
x.grad == 4 * x
```

```{.python .input}
#@tab tensorflow
x_grad == 4 * x
```

[**Sekarang mari kita hitung fungsi lain dari `x`.**]

```{.python .input}
with autograd.record():
    y = x.sum()
y.backward()
x.grad  # Ditimpa dengan gradien yang baru dihitung
```

```{.python .input}
#@tab pytorch
# PyTorch mengumpulkan gradien secara bawaan, kita perlu menghapus 
# nilai sebelumnya
x.grad.zero_()
y = x.sum()
y.backward()
x.grad
```

```{.python .input}
#@tab tensorflow
with tf.GradientTape() as t:
    y = tf.reduce_sum(x)
t.gradient(y, x)  # Ditimpa dengan gradien yang baru dihitung
```

## *Backward* untuk Variabel Non-Skalar

Secara teknis, jika `y` bukan skalar,
interpretasi paling alami dari diferensiasi vektor `y`
terhadap vektor, `x` adalah matriks.
Untuk `y` dan` x` dengan dimensi dan orde yang lebih tinggi,
hasil diferensiasinya bisa berupa tensor orde tinggi.

Namun, sementara benda-benda yang lebih eksotis ini muncul
dalam pembelajaran mesin lanjutan (termasuk [**di pembelajaran mendalam**]),
lebih sering (**saat kita memanggil backward pada vektor,**)
kita mencoba menghitung turunan dari fungsi kerugian
untuk setiap konstituen dari *batch* sampel pelatihan.
Di sini, (**maksud kami**) bukan untuk menghitung matriks diferensiasi
melainkan (**jumlah dari turunan parsial
dihitung secara individual untuk setiap sampel**) dalam *batch*.

```{.python .input}
# Ketika kita memanggil `backward` pada variabel vektor `y` (fungsi dari `x`),
# sebuah variabel skalar terbentuk dari penjumlahan elemen-elemen dalam `y`.
# Kemudian, gradien dari variabel skalar itu terhadap `x` dihitung.
with autograd.record():
    y = x * x  # `y` is a vector
y.backward()
x.grad  # Equals to y = sum(x * x)
```

```{.python .input}
#@tab pytorch
# Memanggil `backward` pada non-skalar perlu memberikan argumen `gradient` yang
# menentukan gradien dari fungsi turunan terhadap `self`.
# Dalam kasus kita, kita hanya ingin menjumlah turunan parsial, jadi memberikan
# argumen gradien dari `ones` sudah tepat
x.grad.zero_()
y = x * x
# y.backward(torch.ones(len(x))) equivalent to the below
y.sum().backward()
x.grad
```

```{.python .input}
#@tab tensorflow
with tf.GradientTape() as t:
    y = x * x
t.gradient(y, x)  # Same as `y = tf.reduce_sum(x * x)`
```

## Melepaskan Komputasi

Terkadang, kita ingin [**memindahkan beberapa kalkulasi
di luar grafik komputasi yang direkam.**]
Misalnya, `y` dihitung sebagai fungsi dari` x`,
dan selanjutnya `z` dihitung sebagai fungsi dari` y` dan `x`.
Sekarang, bayangkan kita ingin menghitung
gradien `z` terhadap` x`,
tetapi ingin memperlakukan `y` sebagai konstanta,
dan hanya memperhitungkan peran `x` setelah` y` dihitung.

Di sini, kita dapat melepaskan `y` untuk mengembalikan variabel baru `u`
yang memiliki nilai yang sama dengan `y` tetapi membuang informasi apa pun
tentang bagaimana `y` dihitung dalam grafik komputasi.
Dengan kata lain, gradien tidak akan mengalir mundur melalui `u` ke` x`.
Jadi, fungsi propagasi mundur berikut menghitung
turunan parsial dari `z = u * x` terhadap `x` sambil memperlakukan `u` sebagai konstanta,
alih-alih turunan parsial dari `z = x * x * x` terhadap `x`.


```{.python .input}
with autograd.record():
    y = x * x
    u = y.detach()
    z = u * x
z.backward()
x.grad == u
```

```{.python .input}
#@tab pytorch
x.grad.zero_()
y = x * x
u = y.detach()
z = u * x

z.sum().backward()
x.grad == u
```

```{.python .input}
#@tab tensorflow
# Atur `persistent=True` untuk menjalankan `t.gradient` lebih dari satu
with tf.GradientTape(persistent=True) as t:
    y = x * x
    u = tf.stop_gradient(y)
    z = u * x

x_grad = t.gradient(z, x)
x_grad == u
```

Karena komputasi `y` direkam,
kita kemudian bisa memanggil backpropagation pada `y` untuk mengambil turunan dari `y = x * x` terhadap `x`, yaitu `2 * x`.

```{.python .input}
y.backward()
x.grad == 2 * x
```

```{.python .input}
#@tab pytorch
x.grad.zero_()
y.sum().backward()
x.grad == 2 * x
```

```{.python .input}
#@tab tensorflow
t.gradient(y, x) == 2 * x
```

## Menghitung Gradien Aliran Kontrol Python

Salah satu keuntungan menggunakan diferensiasi otomatis
adalah [**meskipun jika**] membuat grafik komputasi (**fungsi
diperlukan melewati labirin aliran kontrol Python**)
(misalnya, kondisional, loop, dan panggilan fungsi arbitrer),
(**kita masih bisa menghitung gradien dari variabel yang dihasilkan.**)
Dalam cuplikan berikut, perhatikan bahwa
jumlah iterasi dari loop `while`
dan evaluasi `if`
keduanya bergantung pada nilai input `a`.

```{.python .input}
def f(a):
    b = a * 2
    while np.linalg.norm(b) < 1000:
        b = b * 2
    if b.sum() > 0:
        c = b
    else:
        c = 100 * b
    return c
```

```{.python .input}
#@tab pytorch
def f(a):
    b = a * 2
    while b.norm() < 1000:
        b = b * 2
    if b.sum() > 0:
        c = b
    else:
        c = 100 * b
    return c
```

```{.python .input}
#@tab tensorflow
def f(a):
    b = a * 2
    while tf.norm(b) < 1000:
        b = b * 2
    if tf.reduce_sum(b) > 0:
        c = b
    else:
        c = 100 * b
    return c
```


Mari kita hitung gradiennya.

```{.python .input}
a = np.random.normal()
a.attach_grad()
with autograd.record():
    d = f(a)
d.backward()
```

```{.python .input}
#@tab pytorch
a = torch.randn(size=(), requires_grad=True)
d = f(a)
d.backward()
```

```{.python .input}
#@tab tensorflow
a = tf.Variable(tf.random.normal(shape=()))
with tf.GradientTape() as t:
    d = f(a)
d_grad = t.gradient(d, a)
d_grad
```

Sekarang kita dapat menganalisis fungsi `f` yang ditentukan di atas.
Perhatikan bahwa ini fungsi *piecewise linear* dalam inputnya `a`.
Dengan kata lain, untuk setiap `a` terdapat beberapa skalar konstan `k`
sedemikian rupa sehingga `f(a) = k * a`, di mana nilai `k` bergantung pada input `a`.
Akibatnya, `d / a` memungkinkan kita memverifikasi bahwa gradien sudah benar.

```{.python .input}
a.grad == d / a
```

```{.python .input}
#@tab pytorch
a.grad == d / a
```

```{.python .input}
#@tab tensorflow
d_grad == d / a
```

## Ringkasan

* Kerangka pembelajaran mendalam dapat mengotomatiskan penghitungan turunan. Untuk menggunakannya, pertama-tama kita mengikatkan gradien ke variabel tersebut terhadap turunan parsial yang kita inginkan. Kita kemudian merekam perhitungan nilai target, menjalankan fungsinya untuk propagasi mundur, dan mengakses gradien yang dihasilkan.


## Latihan

1. Mengapa turunan kedua jauh lebih mahal untuk dihitung daripada turunan pertama?
1. Setelah menjalankan fungsi untuk propagasi mundur, segera jalankan lagi dan lihat apa yang terjadi.
1. Dalam contoh aliran kontrol di mana kita menghitung turunan dari `d` terhadap` a`, apa yang akan terjadi jika kita mengubah variabel `a` menjadi vektor atau matriks acak. Pada titik ini, hasil kalkulasi `f(a)` bukan lagi skalar. Apa yang terjadi dengan hasilnya? Bagaimana kita menganalisis ini?
1. Desain ulang contoh menemukan gradien aliran kontrol. Jalankan dan analisis hasilnya.
1. Misalkan $f(x) = \sin(x)$. Plot $f(x)$ dan $\frac{df(x)}{dx}$, di mana yang kedua dihitung tanpa mengeksploitasi bahwa $f'(x) = \cos(x)$.

:begin_tab:`mxnet`
[Discussions](https://discuss.d2l.ai/t/34)
:end_tab:

:begin_tab:`pytorch`
[Discussions](https://discuss.d2l.ai/t/35)
:end_tab:

:begin_tab:`tensorflow`
[Discussions](https://discuss.d2l.ai/t/200)
:end_tab:


