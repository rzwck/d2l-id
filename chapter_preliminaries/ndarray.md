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

