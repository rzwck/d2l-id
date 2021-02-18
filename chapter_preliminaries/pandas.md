# Pengolahan Data
:label:`sec_pandas`

Sejauh ini kami telah mengenalkan berbagai teknik manipulasi data yang disimpan di tensor.
Untuk menerapkan pembelajaran learning untuk menyelesaikan masalah dunia-nyata,
kita seringkali mulai dengan pengolahan data mentah, bukan data yang telah dengan rapi disimpan dalam bentuk tensor.
Di antara alat analisis di Python, paket `pandas` adalah yang paling umum dipakai.
Seperti banyak paket tambahan di Python ekosistem,
`pandas` bisa bekerja sama dengan tensor.
Jadi, kami akan secara singkat mengenalkan langkah-langkah untuk mengolah data mentah dengan `pandas`
dan mengubahnya menjadi format tensor.
Kami akan membahas lebih banyak lagi teknik pengolahan data di bab-bab berikutnya.

## Membaca Dataset

Sebagai contoh, kita mulai dengan (**membuat dataset buatan yang disimpan dalam file csv (comma-separated values)**)
`../data/house_tiny.csv`. Data yang disimpan dalam format lain mungkin bisa diproses dengan cara yang sama.

Di bawah ini kita menulis dataset baris demi baris ke dalam file csv.

```{.python .input}
#@tab all
import os

os.makedirs(os.path.join('..', 'data'), exist_ok=True)
data_file = os.path.join('..', 'data', 'house_tiny.csv')
with open(data_file, 'w') as f:
    f.write('NumRooms,Alley,Price\n')  # Nama kolom
    f.write('NA,Pave,127500\n')  # Setiap baris merepresentasikan satu sampel data
    f.write('2,NA,106000\n')
    f.write('4,NA,178100\n')
    f.write('NA,NA,140000\n')
```

Untuk [**memuat dataset mentah dari file csv yang baru dibuat**],
kita impor paket `pandas` dan memanggil fungsi `read_csv`.
Dataset ini memiliki empat baris dan 3 kolom, di mana setiap baris mendeskripsikan jumlah kamar ("NumRooms"), jenis lorong ("Alley"), dan harga ("Price") dari sebuah rumah.

```{.python .input}
#@tab all
# Jika pandas belum diinstal, hapus komentar baris di bawah ini:
# !pip install pandas
import pandas as pd

data = pd.read_csv(data_file)
print(data)
```

## Menangani Data Hilang

Perhatikan bahwa entri "NaN" adalah nilai-nilai yang hilang.
Untuk menangani data hilang, metode yang umum antara lain *imputasi* dan *penghapusan*,
di mana imputasi mengganti nilai yang hilang dengan nilai pengganti,
sementara penghapusan mengabaikan nilai yang hilang. Di sini kita membahas imputasi.

Dengan pengindeksan berbasis *lokasi-integer* (`iloc`), kita membagi `data` menjadi `inputs` dan `outpus`,
di mana `inputs` hanya mengambil 2 kolom pertama, sementara `outputs` hanya mengambil kolom terakhir.
Untuk mengganti nilai yang hilang di `inputs`, kita [**mengganti entri NaN dengan nilai rata-rata dari kolom yang sama.**]

```{.python .input}
#@tab all
inputs, outputs = data.iloc[:, 0:2], data.iloc[:, 2]
inputs = inputs.fillna(inputs.mean())
print(inputs)
```

[**Untuk nilai kategorikal atau nilai diskrit di `inputs`, kita menganggap "NaN" sebagai kategori.**]
Karena kolom "Alley" hanya mempunyai dua jenis nilai "Pave" dan "NaN",
`pandas` dapat secara otomatis mengubah kolom ini menjadi dua kolom "Alley_Pave" dan "Alley_nan".
Baris yang jenis lorongnya adalah "Pave" akan mempunyai nilai "Alley_Pave" 1 dan nilai "Alley_nan" 0.
Baris yang tidak mempunyai data jenis lorong, kolom Alley_Pave akan bernilai 0 dan kolom Alley_nan bernilai 1.

```{.python .input}
#@tab all
inputs = pd.get_dummies(inputs, dummy_na=True)
print(inputs)
```

## Konversi ke Format Tensor

Sekarang karena [**semua entri di `inputs` dan `outputs` adalah numerik, entri-entri tersebut bisa diubah menjadi format tensor.**]
Setelah data dalam format ini, data tersebut dapat dimanipulasi lebih lanjut dengan fungsionalitas tensor yang telah diperkenalkan di :numref:`sec_ndarray`.

```{.python .input}
from mxnet import np

X, y = np.array(inputs.values), np.array(outputs.values)
X, y
```

```{.python .input}
#@tab pytorch
import torch

X, y = torch.tensor(inputs.values), torch.tensor(outputs.values)
X, y
```

```{.python .input}
#@tab tensorflow
import tensorflow as tf

X, y = tf.constant(inputs.values), tf.constant(outputs.values)
X, y
```

## Ringkasan

* Seperti banyak paket tambahan di ekosistem Python, `pandas` dapat bekerja sama dengan tensor.
* Imputasi dan penghapusan dapat digunakan untuk menangani data yang hilang.


## Latihan

Buatlah dataset mentah dengan lebih banyak baris dan kolom.

1. Hapus kolom dengan paling banyak data yang hilang
2. Konversi data yang telah diproses awal ke format tensor.

:begin_tab:`mxnet`
[Diskusi](https://discuss.d2l.ai/t/28)
:end_tab:

:begin_tab:`pytorch`
[Diskusi](https://discuss.d2l.ai/t/29)
:end_tab:

:begin_tab:`tensorflow`
[Diskusi](https://discuss.d2l.ai/t/195)
:end_tab:
