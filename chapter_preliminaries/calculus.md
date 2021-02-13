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
Di antara aplikasi paling kritis dari kalkulus diferensial, yaitu 
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
