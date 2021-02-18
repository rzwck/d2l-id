# Probabilitas
:label:`sec_prob`

Dalam berbagai bentuk, pembelajaran mesin adalah tentang membuat prediksi.
Kita mungkin ingin memprediksi *kemungkinan* pasien menderita serangan jantung di tahun depan, berdasarkan riwayat klinisnya. 
Dalam deteksi anomali, kita mungkin ingin menilai seberapa *kemungkinan* kumpulan bacaan indikator dari mesin jet pesawat, jika beroperasi secara normal. 
Dalam pembelajaran penguatan, kita ingin agen bertindak secara cerdas di lingkungan. 
Ini berarti kita perlu memikirkan kemungkinan mendapatkan hadiah tinggi di bawah setiap tindakan yang tersedia. 
Dan ketika kita membangun sistem pemberi rekomendasi, kita juga perlu memikirkan kemungkinan. 
Misalnya, katakanlah kita bekerja untuk penjual buku online besar. Kita mungkin ingin memperkirakan kemungkinan bahwa 
pengguna tertentu akan membeli buku tertentu. Untuk ini kita perlu menggunakan bahasa probabilitas.
Ada banyak kursus, jurusan, tesis, karir, dan bahkan departemen, yang dikhususkan untuk mempelajari probabilitas.
Jadi secara alami, tujuan kita di bagian ini bukanlah untuk mengajarkan keseluruhan mata pelajaran. 
Namun, kami berharap dapat membantu Anda memulai, untuk mengajari Anda cukup hal agar Anda dapat mulai membangun
model pembelajaran mendalam pertama Anda, 
dan cukup untuk Anda merasakan subyek ini sehingga 
Anda dapat mulai menjelajahinya lebih lanjut jika Anda mau.

Kita telah menggunakan probabilitas di bagian-bagian sebelumnya tanpa menjelaskan dengan tepat tentangnya atau memberikan contoh yang kongkrit. 
Mari kita mulai serius dengan melihat kasus pertama:
membedakan kucing dan anjing berdasarkan foto.
Mungkin ini terdengar sederhana, tapi ini sebenarnya tantangan yang berat.
Untuk mengawali, kesulitan dari masalah ini mungkin tergantung dari resolusi gambar.

![Gambar dengan berbagai resolusi ($10 \times 10$, $20 \times 20$, $40 \times 40$, $80 \times 80$, and $160 \times 160$ pixels).](../img/cat-dog-pixels.png)
:width:`300px`
:label:`fig_cat_dog`

Seperti yang ditunjukkan di :numref:`fig_cat_dog`,
mudah bagi manusia untuk mengenali kucing dan anjing pada resolusi $160 \times 160$ ​​piksel,
menjadi lebih menantang pada $40 \times 40$ piksel dan hampir tidak mungkin pada $10 \times 10$ piksel. 
Dengan kata lain, kemampuan kita untuk membedakan kucing dan anjing pada jarak yang jauh (dan dengan demikian resolusi rendah) mungkin mendekati tebakan acak. 
Probabilitas memberi kita cara formal menalar tingkat kepastian kita.
Jika kita benar-benar yakin
bahwa gambar tersebut menggambarkan seekor kucing, kita katakan bahwa *probabilitas* bahwa label terkait $y$ adalah "kucing", dilambangkan dengan $P(y=$ "kucing"$)$ equals $1$.
Jika kita tidak memiliki bukti yang menunjukkan bahwa $y =$ "cat" atau $y =$ "dog", maka kita dapat mengatakan bahwa kedua pilihan itu sama-sama memungkinkan, yang dinyatakan sebagai $P(y=$ "cat"$) = P(y=$ "dog"$) = 0.5$. Jika kita cukup percaya diri, tetapi tidak yakin bahwa gambar tersebut menggambarkan seekor kucing, kita dapat menetapkan probabilitas $0.5  < P(y=$ "cat"$) < 1$.

Sekarang pertimbangkan kasus kedua: berdasarkan data-data pemantauan cuaca, kita ingin memprediksi kemungkinan hujan akan turun di Taipei besok. Jika saat itu musim panas, hujan kemungkinan akan turun dengan probabilitas 0.5.

Dalam kedua kasus tersebut, kita memiliki beberapa nilai yang penting. Dan dalam kedua kasus tersebut kita tidak yakin tentang hasilnya.
Tetapi ada perbedaan utama antara kedua kasus tersebut. Dalam kasus pertama ini, gambar tersebut sebenarnya adalah seekor anjing atau kucing, dan kita tidak tahu yang mana. Dalam kasus kedua, hasilnya mungkin sebenarnya adalah peristiwa acak, jika Anda percaya pada hal-hal seperti itu (dan kebanyakan fisikawan percaya). Jadi probabilitas adalah bahasa yang fleksibel untuk bernalar tentang tingkat kepastian kita, dan dapat diterapkan secara efektif dalam konteks yang luas.


## Teori Dasar Probabilitias

Katakanlah kita melempar dadu dan ingin mengetahui peluang melihat angka 1 daripada angka lain. Jika dadu ini dadu yang *fair*, semua kemungkinan $\{1, \ldots, 6\}$ memiliki peluang yang sama untuk muncul, sehingga kita akan melihat $1$ di antara 6 kasus. Secara formal, kita katakan bahwa $1$ terjadi dengan peluang $\frac{1}{6}$.

Untuk dadu yang kita dapatkan langsung dari pabrik, kita mungkin tidak tahu proporsi tersebut, dan kita ingin menguji apakah dadu ini *fair*. Satu-satunya cara 
untuk memeriksa dadu ini adalah dengan melemparnya berkali-kali dan mencatat hasilnya. Untuk setiap pelemparan dadu, kita akan mengamati nilai $\{1, \ldots, 6\}$. Berdasarkan hasil ini, kita ingin menginvestigasi peluang kemunculan setiap angka.

Salah satu caranya adalah dengan mengambil jumlah kemunculan untuk setiap nilai dan membaginya dengan total eksperimen pelemparan dadu.
Ini memberi kita perkiraan peluang dari kejadian tersebut. 
Hukum *bilangan besar* (The *law of large numbers*) menyatakan bahwa dengan semakin banyaknya pelemparan dadu, perkiraan ini akan semakin mendekati nilai peluang yang sebenarnya. 
Sebelum kita masuk ke detail apa yang terjadi di sini, mari kita coba dulu.

Mari kita impor paket-paket yang diperlukan.

```{.python .input}
%matplotlib inline
from d2l import mxnet as d2l
from mxnet import np, npx
import random
npx.set_np()
```

```{.python .input}
#@tab pytorch
%matplotlib inline
from d2l import torch as d2l
import torch
from torch.distributions import multinomial
```

```{.python .input}
#@tab tensorflow
%matplotlib inline
from d2l import tensorflow as d2l
import tensorflow as tf
import tensorflow_probability as tfp
import numpy as np
```

Selanjutnya, kita ingin bisa melempar dadu. Dalam statistik kita menyebut proses ini
sebagai pengambilan sampel dari distribusi probabilitas (*sampling*).
Distribusi
yang memberikan probabilitas ke sejumlah pilihan diskrit disebut
*distribusi multinomial*. Kami akan memberikan definisi yang lebih formal tentang 
*distribusi* nanti, tetapi pada tingkat tinggi, anggap itu hanya sebagai penugasan
probabilitas untuk suatu kejadian.

Untuk mengambil satu sampel, kita cukup memasukkan vektor probabilitas.
Keluarannya adalah vektor lain dengan panjang yang sama:
nilainya pada indeks $i$ adalah berapa kali hasil pengambilan sampel adalah $i$.

```{.python .input}
fair_probs = [1.0 / 6] * 6
np.random.multinomial(1, fair_probs)
```

```{.python .input}
#@tab pytorch
fair_probs = torch.ones([6]) / 6
multinomial.Multinomial(1, fair_probs).sample()
```

```{.python .input}
#@tab tensorflow
fair_probs = tf.ones(6) / 6
tfp.distributions.Multinomial(1, fair_probs).sample()
```

Jika Anda menjalankan pengambilan sampel beberapa kali, Anda akan menemukan bahwa 
Anda mendapatkan nilai yang acak setiap waktu. 
Seperti halnya memperkirakan sebarapa *fair* sebuah dadu, kita ingin 
mengambil banyak sampel dari distribusi yang sama. Ini akan menjadi sangat lambat
jika melakukan ini dengan instruksi `for` di Python, jadi fungsi yang kita gunakan
mendukung mengambil beberapa sampel sekaligus, mengembalikan senarai sampel independen 
dalam bentuk yang kita inginkan.

```{.python .input}
np.random.multinomial(10, fair_probs)
```

```{.python .input}
#@tab pytorch
multinomial.Multinomial(10, fair_probs).sample()
```

```{.python .input}
#@tab tensorflow
tfp.distributions.Multinomial(10, fair_probs).sample()
```

Sekarang setelah kita mengetahui cara mengambil sampel dari lemparan dadu, kita dapat mensimulasikan 1000 lemparan. Kita
kemudian dapat memeriksa dan menghitung, setiap 1000 gulungan, berapa kali masing-masing nomor muncul.
Secara khusus, kita menghitung frekuensi relatif sebagai estimasi probabilitas.

```{.python .input}
counts = np.random.multinomial(1000, fair_probs).astype(np.float32)
counts / 1000
```

```{.python .input}
#@tab pytorch
# Store the results as 32-bit floats for division
counts = multinomial.Multinomial(1000, fair_probs).sample()
counts / 1000  # Relative frequency as the estimate
```

```{.python .input}
#@tab tensorflow
counts = tfp.distributions.Multinomial(1000, fair_probs).sample()
counts / 1000
```

0Karena kita membuat data dari dadu yang *fair*, kita tahu bahwa setiap hasil memiliki probabilitas yang benar $\frac{1}{6}$, sekitar $0.167$, jadi perkiraan keluaran di atas terlihat bagus.

Kami juga dapat memvisualisasikan bagaimana probabilitas ini mengerucut dari waktu ke waktu menuju probabilitas yang benar.
Mari kita lakukan 500 kelompok percobaan dimana setiap kelompok mengambil 10 sampel.

```{.python .input}
counts = np.random.multinomial(10, fair_probs, size=500)
cum_counts = counts.astype(np.float32).cumsum(axis=0)
estimates = cum_counts / cum_counts.sum(axis=1, keepdims=True)

d2l.set_figsize((6, 4.5))
for i in range(6):
    d2l.plt.plot(estimates[:, i].asnumpy(),
                 label=("P(die=" + str(i + 1) + ")"))
d2l.plt.axhline(y=0.167, color='black', linestyle='dashed')
d2l.plt.gca().set_xlabel('Groups of experiments')
d2l.plt.gca().set_ylabel('Estimated probability')
d2l.plt.legend();
```

```{.python .input}
#@tab pytorch
counts = multinomial.Multinomial(10, fair_probs).sample((500,))
cum_counts = counts.cumsum(dim=0)
estimates = cum_counts / cum_counts.sum(dim=1, keepdims=True)

d2l.set_figsize((6, 4.5))
for i in range(6):
    d2l.plt.plot(estimates[:, i].numpy(),
                 label=("P(die=" + str(i + 1) + ")"))
d2l.plt.axhline(y=0.167, color='black', linestyle='dashed')
d2l.plt.gca().set_xlabel('Groups of experiments')
d2l.plt.gca().set_ylabel('Estimated probability')
d2l.plt.legend();
```

```{.python .input}
#@tab tensorflow
counts = tfp.distributions.Multinomial(10, fair_probs).sample(500)
cum_counts = tf.cumsum(counts, axis=0)
estimates = cum_counts / tf.reduce_sum(cum_counts, axis=1, keepdims=True)

d2l.set_figsize((6, 4.5))
for i in range(6):
    d2l.plt.plot(estimates[:, i].numpy(),
                 label=("P(die=" + str(i + 1) + ")"))
d2l.plt.axhline(y=0.167, color='black', linestyle='dashed')
d2l.plt.gca().set_xlabel('Groups of experiments')
d2l.plt.gca().set_ylabel('Estimated probability')
d2l.plt.legend();
```

Setiap kurva penuh berhubungan dengan satu dari enam nilai dadu dan memberikan estimasi probabilitas munculnya nilai tersebut dalam setiap kelompok eksperimen.
Garis hitam terputus adalah nilai probabilitas yang benar. 
Dengan semakin banyaknya data yang kita dapatkan dari eksperimen, semua kurva tersebut mengerucut ke probabilitas yang benar.

### Aksioma Teori Probabilitas

Ketika berhadapan dengan pelemparan dadu, 
kita menyebut himpunan $\mathcal{S} = \{1, 2, 3, 4, 5, 6\}$ sebagai *ruang sampel* atau *ruang hasil*, di mana setiap elemennya adalah *hasil*.
Sebuah kejadian adalah sekumpulan hasil dari suatu ruang sampel.
Contohnya, "melihat angka $5$" ($\{5\}$) dan "melihat angka ganjil" ($\{1, 3, 5\}$) adalah kejadian yang sahih dari pelemparan dadu.
Perhatikan bahwa jika hasil dari eksperimen acak ada dalam kejadian $\mathcal{A}$,
maka kejadian $\mathcal{A}$ telah terjadi. Jadi, bila $3$ titik terlihat setelah melempar dadu, karena $3 \in \{1, 3, 5\}$,
kita bisa mengatakan bahwa kejadian "melihat angka ganjil" telah terjadi.

Secara formal, *probabilitas* dapat dianggap sebagai fungsi yang memetakan himpunan ke nilai riel.
Probabilitas kejadian $\mathcal{A}$ dalam ruang sampel yang diberikan $\mathcal{S}$,
dilambangkan sebagai $P(\mathcal{A})$, memenuhi properti berikut:

* Untuk sembarang kejadian $\mathcal{A}$, probabilitasnya tidak mungkin negatif, yaitu, $P(\mathcal{A}) \geq 0$;
* Probabilitas dari seluruh ruang sampel adalah $1$, yaitu, $P(\mathcal{S}) = 1$;
* Untuk sembarang kejadian berurutan $\mathcal{A}_1, \mathcal{A}_2, \ldots$ yang *ekslusif mutual* ($\mathcal{A}_i \cap \mathcal{A}_j = \emptyset$ for all $i \neq j$), probabilitas bahwa salah satunya terjadi adalah sama dengan jumlah masing-masing probabilitasnya, i.e., $P(\bigcup_{i=1}^{\infty} \mathcal{A}_i) = \sum_{i=1}^{\infty} P(\mathcal{A}_i)$.


Ini juga merupakan aksioma teori probabilitas, yang dikemukakan oleh Kolmogorov pada tahun 1933.
Berkat sistem aksioma ini, kita dapat menghindari perselisihan filosofis tentang keacakan;
sebaliknya, kita dapat bernalar secara ketat dengan bahasa matematika.
Misalnya, dengan menganggap kejadian  $\mathcal{A}_1$ sebagai seluruh ruang sampel dan $\mathcal{A}_i = \emptyset$ untuk semua $i > 1$, kita dapat membuktikan bahwa $P(\emptyset) = 0$, yaitu probabilitas dari suatu peristiwa yang tidak mungkin adalah $0$.


### Variabel Acak


