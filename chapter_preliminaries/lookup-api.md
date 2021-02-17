# Dokumentasi
:begin_tab:`mxnet`
Karena buku ini terbatas, kami tidak mungkin mengenalkan setiap fungsi dan kelas MXNet (dan mungkin Anda juga tidak menginginkannya). Dokumentasi API dan tutorial-tutorial tambahan dan contoh-contoh memberikan banyak sekali dokumentasi yang diluar cakupan buku ini. Di bagian ini kami memberikan beberapa petunjuk untuk mengeksplorasi API MXNet.
:end_tab:

:begin_tab:`pytorch`
Karena buku ini terbatas, kami tidak mungkin mengenalkan setiap fungsi dan kelas PyTorch (dan mungkin Anda juga tidak menginginkannya). Dokumentasi API dan tutorial-tutorial tambahan dan contoh-contoh memberikan banyak sekali dokumentasi yang diluar cakupan buku ini. Di bagian ini kami memberikan beberapa petunjuk untuk mengeksplorasi API PyTorch.
:end_tab:

:begin_tab:`tensorflow`
Karena buku ini terbatas, kami tidak mungkin mengenalkan setiap fungsi dan kelas TensorFlow (dan mungkin Anda juga tidak menginginkannya). Dokumentasi API dan tutorial-tutorial tambahan dan contoh-contoh memberikan banyak sekali dokumentasi yang diluar cakupan buku ini. Di bagian ini kami memberikan beberapa petunjuk untuk mengeksplorasi API TensorFlow.
:end_tab:


## Menemukan semua Fungsi dan Kelas di suatu Modul

Untuk mengetahui fungsi dan kelas yang bisa dipanggil di suatu modul, we 
menggunakan fungsi `dir`. Contohnya, kita bisa (**melihat semua properti di modul
untuk membangkitkan bilangan acak**):

```{.python .input  n=1}
from mxnet import np
print(dir(np.random))
```

```{.python .input  n=1}
#@tab pytorch
import torch
print(dir(torch.distributions))
```

```{.python .input  n=1}
#@tab tensorflow
import tensorflow as tf
print(dir(tf.random))
```

Umumnya, kita bisa mengabaikan fungsi yang diawali dan diakhiri dengan `__` (objyek khusus in Python) atau fungsi yang diawali dengan karakter tunggal `_` (biasanya fungsi internal). Berdasarkan nama fungsi dan atribut yang tersisa, kita bisa menebak bahwa modul ini menawarkan berbagai metode untuk membangkitkan 
bilangan acak, termasuk pengambilan sampel dari distribusi seragam (`uniform`), distribusi normal (`normal`), dan distribusi multinomial (`multinomial`).

## Menemukan Cara Pemakaian Fungsi dan Kelas Tertentu

Instruksi yang lebih spesifik tentang cara menggunakan fungsi dan kelas tertentu, kita bisa memanggil fungsi `help`. Contohnya, mari kita [**eksplorasi instruksi pemakaian untuk fungsi `ones` dari tensor**]

```{.python .input}
help(np.ones)
```

```{.python .input}
#@tab pytorch
help(torch.ones)
```

```{.python .input}
#@tab tensorflow
help(tf.ones)
```

Dari dokumentasi, kita bisa melihat bahwa fungsi `ones` membuat tensor baru dengan bentuk tertentu dan menginisialisasi semua elemennya dengan nilai 1. Disarankan, jika memungkinkan, anda seharusnya menjalankan (**test cepat**) untuk mengonfirmasi interpretasi anda:

```{.python .input}
np.ones(4)
```

```{.python .input}
#@tab pytorch
torch.ones(4)
```

```{.python .input}
#@tab tensorflow
tf.ones(4)
```

Di notebook Jupyter, kita bisa memakai `?` untuk menampilkan dokumen di jendela lain.
Contohnya, `list?` akan membuat  konten yang hampir identik dengan `help(list)`, ditampilkan di jendela peramban baru. 
Selain itu, jika kita memakai dua tanda tanya, seperti `list??`, kode Python implementasi dari fungsi tersebut juga akan ditampilkan.


## Ringkasan

* Dokumentasi resmi memberikan banyak deskripsi dan contoh yang diluar cakupan buku ini.
* Kita bisa mencari dokumentasi untuk penggunaan suatu API dengan memanggil fungsi `dir` dan `help`, atau `?` dan `??` di notebook Jupyter.


## Latihan

1. Carilah dokumentasi untuk sembarang fungsi atau kelas di kerangka kerja pembelajaran mendalam. Apakah anda juga bisa menemukan dokumentasi pada web resmi kerangka kerja tersebut?


:begin_tab:`mxnet`
[Diskusi](https://discuss.d2l.ai/t/38)
:end_tab:

:begin_tab:`pytorch`
[Diskusi](https://discuss.d2l.ai/t/39)
:end_tab:

:begin_tab:`tensorflow`
[Diskusi](https://discuss.d2l.ai/t/199)
:end_tab:
