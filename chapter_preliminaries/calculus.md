# Kalkulus
:label:`sec_calculus`

Menemukan luas poligon adalah suatu hal yang misterius
sampai setidaknya 2.500 tahun yang lalu, ketika orang Yunani kuno membagi poligon menjadi segitiga dan menjumlahkan luasnya.
Untuk mencari luas dari bentuk lengkung, seperti lingkaran,
Yunani kuno menggambar poligon dalam lingkaran seperti yang ditunjukkan di :numref:`fig_circle_area`,
semakin banyak sisi poligon dalam lingkaran semakin mendekati 
luas lingkaran yang sebenarnya. Proses ini juga dikenal sebagai metode *exhaustion*.

![Mencari luas lingkaran dengan metode *exhaustion*.](../img/polygon-circle.svg)
:label:`fig_circle_area`

Faktanya, metode *exhaustion* asal muasal dari *kalkulus integral* (akan dijelaskan di :numref:`sec_integral_calculus`).
Lebih dari 2.000 tahun kemudian,
cabang kalkulus lainnya, *kalkulus diferensial*,
ditemukan.
Di antara aplikasi paling penting dari kalkulus diferensial, yaitu 
masalah optimasi, menghitung bagaimana melakukan sesuatu dengan cara *yang optimal*.
Seperti yang dibahas di :numref:`subsec_norms_and_objectives`,
masalah seperti itu ada di mana-mana dalam pembelajaran mendalam.

Dalam pembelajaran mendalam, kita *melatih* model, memperbaruinya secara berurutan
sehingga model kita menjadi lebih baik dan lebih baik saat model melihat lebih banyak data.
Biasanya, yang dimaksud dengan lebih baik berarti meminimalkan *fungsi kerugian*,
skor yang menjawab pertanyaan "seberapa *buruk* model kita?"
Pertanyaan ini lebih halus daripada yang terlihat.
Pada akhirnya, apa yang sangat kita pedulikan
adalah menghasilkan model yang berkinerja baik pada data
yang belum pernah kita lihat sebelumnya.
Tapi kita hanya bisa menyesuaikan model dengan data yang sebenarnya bisa kita lihat.
Dengan demikian kita dapat menguraikan tugas menyesuaikan model menjadi dua perhatian utama:
i) *optimasi*: proses menyesuaikan model kita dengan data yang diamati;
ii) *generalisasi*: prinsip matematika dan kebijaksanaan praktisi
yang memandu bagaimana menghasilkan model yang validitasnya juga berlaku 
di luar kumpulan sampel yang digunakan dalam pelatihan.

Untuk membantu Anda memahami
masalah dan metode optimasi di bab selanjutnya,
di sini kami memberikan dasar yang sangat singkat tentang kalkulus diferensial
yang biasa digunakan dalam pembelajaran mendalam.

## Derivatif dan Diferensiasi

Kita mulai dengan menangani perhitungan derivatif,
langkah penting di hampir semua algoritma optimasi pembelajaran mendalam.
Dalam pembelajaran mendalam, kita biasanya memilih fungsi kerugian
yang diferensiabel (mempunyai turunan) terhadap parameter model kita.
Sederhananya, ini berarti untuk setiap parameter,
kita dapat menentukan seberapa cepat kerugian akan bertambah atau berkurang,
apakah kita akan *meningkatkan* atau *menurunkan* nilai parameter itu
dalam jumlah yang sangat kecil.

Misalkan kita memiliki fungsi $f: \mathbb{R} \rightarrow \mathbb{R}$,
yang input dan outputnya adalah skalar.
[**Turunan dari $f$ adalah**]

(**$$f'(x) = \lim_{h \rightarrow 0} \frac{f(x+h) - f(x)}{h},$$**)
:eqlabel:`eq_derivative`

Jika limit ini ada.
Jika $f'(a)$ ada,
maka $f$ bisa disebut diferensiabel pada $a$.
Jika $f$ diferensiabel pada setiap angka di sebuah rentang,
maka fungsi ini diferensiabel pada rentang tersebut.
Kita dapat mengartikan turunan dari $f'(x)$ di :eqref:`eq_derivative`
sebagai 
tingkat perubahan *seketika* (*instantaneous*) dari $f(x)$
terhadap $x$.
Tingkat perubahan seketika tersebut berdasarkan pada 
variasi $h$ dalam $x$, yang mendekati $0$.

Untuk mengilustrasikan turunan, 
mari kita bereksperimen dengan sebuah contoh.
(**Definisikan $u = f(x) = 3x^2-4x$.**)

```{.python .input}
%matplotlib inline
from d2l import mxnet as d2l
from IPython import display
from mxnet import np, npx
npx.set_np()

def f(x):
    return 3 * x ** 2 - 4 * x
```

```{.python .input}
#@tab pytorch
%matplotlib inline
from d2l import torch as d2l
from IPython import display
import numpy as np

def f(x):
    return 3 * x ** 2 - 4 * x
```

```{.python .input}
#@tab tensorflow
%matplotlib inline
from d2l import tensorflow as d2l
from IPython import display
import numpy as np

def f(x):
    return 3 * x ** 2 - 4 * x
```

[**Dengan menetapkan $x=1$ dan membiarkan $h$ mendekati $0$,
hasil numerik dari $\frac{f(x+h) - f(x)}{h}$**]

[**By setting $x=1$ and letting $h$ approach $0$,
the numerical result of $\frac{f(x+h) - f(x)}{h}$**]
dalam :eqref:`eq_derivative`
(**mendekati $2$.**)
Meskipun eksperimen ini bukan bukti matematis, kita akan melihat nanti
bahwa turunan $u'$ adalah $2$ ketika $x=1$.

```{.python .input}
#@tab all
def numerical_lim(f, x, h):
    return (f(x + h) - f(x)) / h

h = 0.1
for i in range(5):
    print(f'h={h:.5f}, numerical limit={numerical_lim(f, 1, h):.5f}')
    h *= 0.1
```

Mari kita biasakan diri dengan beberapa notasi ekuivalen untuk turunan.
Diberikan $y = f(x)$, dimana $x$ dan $y$ adalah variabel independen dan variabel dependen dari fungsi $f$. Ekspresi berikut ini adalah ekuivalen:

$$f'(x) = y' = \frac{dy}{dx} = \frac{df}{dx} = \frac{d}{dx} f(x) = Df(x) = D_x f(x),$$

di mana simbol $\frac{d}{dx}$ dan $D$ adalah operator diferensiasi yang menunjukkan operasi *diferensiasi*.
Kita bisa menggunakan aturan berikut untuk menurunkan fungsi-fungsi yang umum:

* $DC = 0$ ($C$ adalah konstanta),
* $Dx^n = nx^{n-1}$ (aturan pangkat, $n$ adalah bilangan riel sembarang),
* $De^x = e^x$,
* $D\ln(x) = 1/x.$

Untuk menurunkan fungsi yang dibentuk dari beberapa fungsi sederhana seperti di atas, 
aturan berikut bisa berguna buat kita.
Misalkan fungsi $f$ dan $g$ keduanya diferensiabel dan $C$ adalah konstanta,
kita mempunyai aturan perkalian konstanta:

$$\frac{d}{dx} [Cf(x)] = C \frac{d}{dx} f(x),$$

aturan penjumlahan

$$\frac{d}{dx} [f(x) + g(x)] = \frac{d}{dx} f(x) + \frac{d}{dx} g(x),$$

aturan perkalian

$$\frac{d}{dx} [f(x)g(x)] = f(x) \frac{d}{dx} [g(x)] + g(x) \frac{d}{dx} [f(x)],$$

dan aturan pembagian

$$\frac{d}{dx} \left[\frac{f(x)}{g(x)}\right] = \frac{g(x) \frac{d}{dx} [f(x)] - f(x) \frac{d}{dx} [g(x)]}{[g(x)]^2}.$$

Sekarang kita bisa terapkan beberapa aturan di atas untuk menemukan 
$u' = f'(x) = 3 \frac{d}{dx} x^2-4\frac{d}{dx}x = 6x-4$.
Jadi, dengan menetapkan $x = 1$, kita mempunyai $u' = 2$:
ini didukung oleh eksperimen kita di awal dim ana hasil numerikal mendekati $2$.
Turunan ini juga adalah kemiringan dari garis singgung terhadap 
kurva $u = f(x)$ ketika $x = 1$.

[**untuk menggambarkan interpretasi dari turunan,
kita akan memakai `matplotlib`,**]

sebeuah pustaka plotting populer di Python.
Untuk mengonfigurasi properti dari gambar yang dibuat oleh `matplotlib`,
kita perlu membuat beberapa fungsi.
Berikut ini, 
fungsi `use_svg_display` mengatur paket `matplotlib` untuk mengeluarkan gambar dalam format svg untuk gambar yang lebih tajam.
Perhatikan bahwa `#@save` adalah tanda khusus di mana fungsi, kelas atau pernyataan dibawahnya disimpan dalam paket `d2l`
sehingga nantinya bisa secara langsung dipanggil (mis., `d2l.use_svg_display()`) tanpa didefinisikan ulang.

```{.python .input}
#@tab all
def use_svg_display():  #@save
    """Memakai format svg untuk menampilkan plot di Jupyter."""
    display.set_matplotlib_formats('svg')
```

Kita mendefinisikan fungsi `set_figsize` untuk menentukan ukuran gambar. Perhatikan di sini kita menggunakan `d2l.plt` secara langsung karena pernyataan import  `from matplotlib import pyplot as plt` telah ditandai untuk disimpan di paket `d2l` sebelumnya.

```{.python .input}
#@tab all
def set_figsize(figsize=(3.5, 2.5)):  #@save
    """Set the figure size for matplotlib."""
    use_svg_display()
    d2l.plt.rcParams['figure.figsize'] = figsize
```

Fungsi `set_axes` berikut ini mengatur properti sumbu dari gambar yang dihasilkan oleh `matplotlib`.

```{.python .input}
#@tab all
#@save
def set_axes(axes, xlabel, ylabel, xlim, ylim, xscale, yscale, legend):
    """Atur properti sumbu untuk matplotlib."""
    axes.set_xlabel(xlabel)
    axes.set_ylabel(ylabel)
    axes.set_xscale(xscale)
    axes.set_yscale(yscale)
    axes.set_xlim(xlim)
    axes.set_ylim(ylim)
    if legend:
        axes.legend(legend)
    axes.grid()
```

Dengan tiga fungsi ini, kita sekarang bisa mendefinisikan fungsi `plot` untuk 
menggambarkan banyak kurva dengan singkat karena kita akan perlu memvisualisasikan 
banyak kurva di sepanjang buku ini.

```{.python .input}
#@tab all
#@save
def plot(X, Y=None, xlabel=None, ylabel=None, legend=None, xlim=None,
         ylim=None, xscale='linear', yscale='linear',
         fmts=('-', 'm--', 'g-.', 'r:'), figsize=(3.5, 2.5), axes=None):
    """Plot titik data."""
    if legend is None:
        legend = []

    set_figsize(figsize)
    axes = axes if axes else d2l.plt.gca()

    # Kembalikan nilai True jika `X` (tensor atau list) memiliki 1 sumbu
    def has_one_axis(X):
        return (hasattr(X, "ndim") and X.ndim == 1 or isinstance(X, list)
                and not hasattr(X[0], "__len__"))

    if has_one_axis(X):
        X = [X]
    if Y is None:
        X, Y = [[]] * len(X), X
    elif has_one_axis(Y):
        Y = [Y]
    if len(X) != len(Y):
        X = X * len(Y)
    axes.cla()
    for x, y, fmt in zip(X, Y, fmts):
        if len(x):
            axes.plot(x, y, fmt)
        else:
            axes.plot(y, fmt)
    set_axes(axes, xlabel, ylabel, xlim, ylim, xscale, yscale, legend)
```

Sekarang kita bisa [**memplot fungsi $u = f(x)$ dan garis singgungnya $y = 2x - 3$ at $x=1$**], di mana koefisien $2$ adalah kemiringan dari garis singgung.

```{.python .input}
#@tab all
x = np.arange(0, 3, 0.1)
plot(x, [f(x), 2 * x - 3], 'x', 'f(x)', legend=['f(x)', 'Garis singgung (x=1)'])
```

## Turunan Parsial

Sejauh ini kita telah membahas diferensiasi fungsi dengan hanya satu variabel.
Dalam pembelajaran mendalam, fungsi seringkali memiliki banyak variabel.
Jadi kita perlu memperluas konsep diferensiasi untuk fungsi multi-variabel.

Misalkan $y = f(x_1, x_2, \ldots, x_n)$ adalah fungsi dengan $n$ variabel. Turunan parsial dari $y$ terhadap parameter ke-$i$, $x_i$ adalah

$$ \frac{\partial y}{\partial x_i} = \lim_{h \rightarrow 0} \frac{f(x_1, \ldots, x_{i-1}, x_i+h, x_{i+1}, \ldots, x_n) - f(x_1, \ldots, x_i, \ldots, x_n)}{h}.$$

Untuk menghitung $\frac{\partial y}{\partial x_i}$, kita bisa memperlakukan $x_1, \ldots, x_{i-1}, x_{i+1}, \ldots, x_n$ sebagai konstanta dan menghitung turunan $y$ terhadap $x_i$.
Untuk notasi turunan parsial, berikut ini adalah e
For notation of partial derivatives, semua bentuk di bawah ini adalah ekuivalen:

$$\frac{\partial y}{\partial x_i} = \frac{\partial f}{\partial x_i} = f_{x_i} = f_i = D_i f = D_{x_i} f.$$

## Gradien

Kita dapat menggabungkan turunan parsial dari fungsi multi variabel terhadap semua variabelnya untuk mendapatkan vektor gradien dari fungsi tersebut.
Misal input dari fungsi $f: \mathbb{R}^n \rightarrow \mathbb{R}$ adalah vektor $n$-dimensi, $\mathbf{x} = [x_1, x_2, \ldots, x_n]^\top$ dan outputnya adalah skalar. Gradien dari fungsi $f(\mathbf{x})$ terhadap $\mathbf{x}$ adalah vektor dari $n$ turunan parsial:

$$\nabla_{\mathbf{x}} f(\mathbf{x}) = \bigg[\frac{\partial f(\mathbf{x})}{\partial x_1}, \frac{\partial f(\mathbf{x})}{\partial x_2}, \ldots, \frac{\partial f(\mathbf{x})}{\partial x_n}\bigg]^\top,$$

di mana $\nabla_{\mathbf{x}} f(\mathbf{x})$ seringkali diganti oleh $\nabla f(\mathbf{x})$ ketika tidak ada ambiguitas.

Misalkan $\mathbf{x}$ adalah vektor $n$-dimensi, aturan di bawah ini sering dipakai untuk menurunkan fungsi multi variabel:

* Untuk semua $\mathbf{A} \in \mathbb{R}^{m \times n}$, $\nabla_{\mathbf{x}} \mathbf{A} \mathbf{x} = \mathbf{A}^\top$,
* Untuk semua  $\mathbf{A} \in \mathbb{R}^{n \times m}$, $\nabla_{\mathbf{x}} \mathbf{x}^\top \mathbf{A}  = \mathbf{A}$,
* Untuk semua  $\mathbf{A} \in \mathbb{R}^{n \times n}$, $\nabla_{\mathbf{x}} \mathbf{x}^\top \mathbf{A} \mathbf{x}  = (\mathbf{A} + \mathbf{A}^\top)\mathbf{x}$,
* $\nabla_{\mathbf{x}} \|\mathbf{x} \|^2 = \nabla_{\mathbf{x}} \mathbf{x}^\top \mathbf{x} = 2\mathbf{x}$.

Demikian pula, untuk sembarang matrix $\mathbf{X}$, kita punya $\nabla_{\mathbf{X}} \|\mathbf{X} \|_F^2 = 2\mathbf{X}$. Seperti yang akan kita lihat nanti, gradien sangat berguna untuk mendesain algoritma optimasi dalam pembelajaran mendalam.


## Aturan Rantai

However, such gradients can be hard to find.
This is because multivariate functions in deep learning are often *composite*,
so we may not apply any of the aforementioned rules to differentiate these functions.
Fortunately, the *chain rule* enables us to differentiate composite functions.

Let us first consider functions of a single variable.
Suppose that functions $y=f(u)$ and $u=g(x)$ are both differentiable, then the chain rule states that

$$\frac{dy}{dx} = \frac{dy}{du} \frac{du}{dx}.$$

Now let us turn our attention to a more general scenario
where functions have an arbitrary number of variables.
Suppose that the differentiable function $y$ has variables
$u_1, u_2, \ldots, u_m$, where each differentiable function $u_i$
has variables $x_1, x_2, \ldots, x_n$.
Note that $y$ is a function of $x_1, x_2, \ldots, x_n$.
Then the chain rule gives

$$\frac{dy}{dx_i} = \frac{dy}{du_1} \frac{du_1}{dx_i} + \frac{dy}{du_2} \frac{du_2}{dx_i} + \cdots + \frac{dy}{du_m} \frac{du_m}{dx_i}$$

for any $i = 1, 2, \ldots, n$.

