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

Dalam eksperimen acak melemparkan dadu tadi, kami memperkenalkan gagasan tentang *variabel acak*. Variabel acak bisa bernilai berapapun dan tidak deterministik. Variabel acak bisa mengambil salah satu nilai di antara serangkaian kemungkinan dalam eksperimen acak.
Pertimbangkan variabel acak $X$ yang nilainya ada di ruang sampel $\mathcal{S} = \{1, 2, 3, 4, 5, 6\}$ dari pelemparan dadu. Kita dapat menunjukkan peristiwa "melihat kemunculan angka $5$" sebagai $\{X = 5\}$ atau $X = 5$, dan probabilitasnya sebagai $P(\{X = 5\})$ atau $P(X = 5)$.
Dengan $P(X = a)$, kita membedakan antara variabel acak $X$ dan nilai (misalnya, $a$) dari $X$.
Namun, penulisan seperti itu membuat notasi menjadi rumit.
Untuk notasi yang lebih singkat,
di satu sisi, kita dapat menuliskan $P(X)$ sebagai *distribusi* dari variabel acak $X$:
distribusi memberi tahu kita berapa peluang $X$ bernilai sesuatu nilai tertentu.
Di samping itu,
kita cukup menulis $P(a)$ untuk menunjukkan peluang bahwa variabel acak bernilai $a$.
Karena kejadian dalam teori probabilitas adalah sekumpulan hasil dari ruang sampel,
kita dapat menentukan rentang nilai dari variabel acak.
Misalnya, $P(1 \leq X \leq 3)$ menunjukkan peluang kejadian $\{1 \leq X \leq 3\}$,
yang artinya $\{X = 1, 2, \text{or}, 3\}$. 
Sama halnya, $P(1 \leq X \leq 3)$ merepresentasikan probabilitas variabel acak $X$ bernilai $\{1, 2, 3\}$.

Perhatikan bahwa ada perbedaan antara variabel acak *diskrit*, seperti sisi sebuah dadu, and variabel acak *kontinu*, seperti tinggi dan berat seseorang. Tidak terlalu berguna menanyakan apakah dua orang memiliki tinggi yang persis sama. Bila kita mengukur dengan tingkat presisi yang cukup tinggi, tidak akan ada dua orang di planet ini yang tingginya benar-benar sama persis. Bahkan, faktanya, bila kita mengukur dengan presisi yang sangat tinggi, tinggi anda tidak akan sama ketika anda bangun dan ketika anda tidur. Jadi tidak ada gunanya menanyakan berapa peluang seseorang tingginya adalah tepat 1.80139278291028719210196740527486202 meter. Dengan tingkat populasi manusia di planet ini, peluangnya praktis adalah nol. 
Akan lebih masuk akal dalam hal ini untuk menanyakan apakah tinggi seseorang berada dalam rentang tinggi tertentu, katakanlah antara 1.79 dan 1.81 meter. 
Dalam kasus ini, kita mengkuantifikasi kemungkinan bahwa kita melihat suatu nilai sebagai *densitas*. Tinggi tepat 1.80 meter tidak memiliki probabilitas, tapi memiliki densitas bukan-nol. Dalam rentang antara dua tinggi yang berbeda peluangnya bukan-nol.
Dalam bagian ini selanjutnya, kita akan membahas tentang probabilitas dalam ruang diskrit.
Anda bisa merujuk pada :numref:`sec_random_variables` untuk probabilitas dalam variabel acak kontinu.

## Berurusan dengan Beberapa Variabel Acak

Sangat sering, kita ingin mempertimbangkan lebih dari satu variabel acak pada satu waktu.
Misalnya, kita mungkin ingin membuat model hubungan antara penyakit dan gejala. Diberikan penyakit dan gejala, katakan "flu" dan "batuk", keduanya mungkin terjadi atau tidak terjadi pada pasien dengan probabilitas tertentu. Meskipun kita berharap probabilitas keduanya mendekati nol, kita mungkin ingin memperkirakan probabilitas ini dan hubungannya satu sama lain sehingga kita dapat menerapkan kesimpulan kita untuk perawatan medis yang lebih baik.

Sebagai contoh yang lebih rumit, gambar berisi jutaan piksel, jadi ada jutaan variabel acak. Dan dalam banyak kasus, gambar dilengkapi dengan 
label, mengidentifikasi objek pada gambar. Kita juga dapat menganggap label sebagai
variabel acak. Kita bahkan dapat menganggap semua metadata sebagai variabel acak
seperti lokasi, waktu, bukaan kamera, panjang fokus, ISO, jarak fokus, dan jenis kamera.
Semua ini adalah variabel acak yang terjadi secara bersama-sama. Saat kita berurusan dengan beberapa variabel acak, ada beberapa nilai yang menarik.

### Probabilitas Gabungan (*Joint Probability*)

Pertama disebut dengan probabilitas gabungan $P(A = a, B=b)$. Diberikan dua nilai $a$ dan $b$, probabilitas gabungan menjawab pertanyaan, berapa peluang  $A=a$ and $B=b$ terjadi bersamaan?
Perhatikan untuk sembarang nilai $a$ dan $b$, $P(A=a, B=b) \leq P(A=a)$.
Hal ini dikarenakan agar $A=a$ dan $B=b$ terjadi bersamaan, $A=a$ harus terjadi *and* $B=b$ juga harus terjadi (dan sebaliknya).
Jadi, $A=a$ dan $B=b$ terjadi bersamaan tidak mungkin lebih memungkinkan daripada $A=a$ atau $B=b$ sendirian.

### Probabilitas Bersyarat (*Conditional Probability*)

Hal ini membawa kita pada rasio menarik: $0 \leq \frac{P(A=a, B=b)}{P(A=a)} \leq 1$. Kita sebut ini sevagai *probabilitas bersyarat*
dan menuliskannya dengan $P(B=b \mid A=a)$: probabilitas $B=b$, bila diketahui $A=a$ telah terjadi.

### Teorema Bayes

Menggunakan definisi probabilitas bersyarat, kita bisa turunkan salah satu persamaan statistik paling berguna dan terkenal: *Teorema Bayes*.
Teorema ini dijelaskan sebagai berikut.

Secara konstruksi, kita punya *aturan perkalian* bahwa $P(A, B) = P(B \mid A) P(A)$. Secara simetris, ini juga berlaku untuk $P(A, B) = P(A \mid B) P(B)$. Anggap bahwa $P(B) > 0$. Menyelesaikan salah satu variabel bersyarat (*conditional variable*) kita dapatkan 
$$P(A \mid B) = \frac{P(B \mid A) P(A)}{P(B)}.$$

Perhatikan bahwa di sini kita memakai notasi yang lebih singkat di mana $P(A, B)$ adalah probabilitas gabungan dan $P(A \mid B)$ adalah distribusi bersyarat (*conditional distribution*). Distribusi seperti itu bisa dievaluasi untuk nilai $A = a, B=b$ tertentu.

### Marjinalisasi

Bayes' theorem is very useful if we want to infer one thing from the other, say cause and effect, but we only know the properties in the reverse direction, as we will see later in this section. One important operation that we need, to make this work, is *marginalization*.
It is the operation of determining $P(B)$ from $P(A, B)$. We can see that the probability of $B$ amounts to accounting for all possible choices of $A$ and aggregating the joint probabilities over all of them:

Teorema Bayes sangat berguna jika kita ingin menyimpulkan satu hal dari yang lain, katakanlah sebab dan akibat, tetapi kita hanya mengetahui properti ini dari arah sebaliknya, seperti yang akan kita lihat nanti di bagian ini. Salah satu operasi penting yang kita butuhkan, untuk membuat ini berhasil, adalah *marginalisasi*.
Ini adalah operasi untuk menentukan $P(B)$ dari $P(A, B)$. Kita dapat melihat bahwa probabilitas $B$ sama dengan memperhitungkan semua kemungkinan nilai $A$ dan menggabungkan probabilitas gabungan atas semuanya:

$$P(B) = \sum_{A} P(A, B),$$

disebut juga sebagai *aturan penjumlahan* (*sum rule*). Probabilitas atau distribusi yang dihasilkan dari marginalisasi disebut dengan *probabilitas marginal* atau *distribusi marginal*.

### Independensi

Properti lain yang berguna yang perlu kita lihat adalah *dependensi* vs. *independensi*.
Dua variabel acak $A$ dan $B$ adalah independen, artinya terjadinya kejadian $A$
tidak mengungkap informasi apapun tentang terjadinya kejadian $B$. Dalam kasus ini $P(B \mid A) = P(B)$. 
Statistikawan biasanya mengekspresikan ini sebagai $A \perp  B$. 
Dari Teorema Bayes, hal ini berarti $P(A \mid B) = P(A)$.
Dalam kasus lainnya kita menyebut $A$ dan $B$ dependen. Contohnya, dua pelemparan dadu yang berurutan adalah independen. 
Sebaliknya, posisi saklar lampu dan kecerahan ruangan tidak independen (walaupun tidak sepenuhnya deterministik, karena ada kemungkinan lain seperti
lampu rusak, listrik mati, atau saklar rusak).

Karena $P(A \mid B) = \frac{P(A, B)}{P(B)} = P(A)$ adalah ekuivalen dengan$P(A, B) = P(A)P(B)$, dua variabel acak adalah independen jika dan hanya jika probabilitas gabungannya adalah perkalian dari masing-masing distribusinya.

Begitu juga, dua variabel acak $A$ dan $B$ adalah independen bersyarat bila diberikan variabel acak lain $C$
jika dan hanya jika $P(A, B \mid C) = P(A \mid C)P(B \mid C)$. Hal ini dituliskan sebagai This is expressed as $A \perp B \mid C$.

### Aplikasi
:label:`subsec_probability_hiv_app`

Mari kita uji kemampuan kita. Asumsikan bahwa seorang dokter melakukan tes HIV kepada pasien. Tes ini cukup akurat dan gagal hanya dengan probabilitas 1% jika pasien dalam keadaan sehat tetapi melaporkannya sebagai sakit. Bahkan,
tidak pernah gagal untuk mendeteksi HIV jika pasien benar-benar mengidapnya. Kita menggunakan $D_1$ untuk menunjukkan diagnosis ($1$ jika positif dan $0$ jika negatif) dan $H$ untuk menunjukkan status HIV ($1$ jika positif dan $0$ jika negatif).
:numref:`conditional_prob_D1` mencantumkan probabilitas bersyarat seperti itu.


:Probabilitas Bersyarat dari $P(D_1 \mid H)$.

| Probabilitas Bersyarat | $H=1$ | $H=0$ |
|---|---|---|
|$P(D_1 = 1 \mid H)$|            1 |         0.01 |
|$P(D_1 = 0 \mid H)$|            0 |         0.99 |
:label:`conditional_prob_D1`

Note that the column sums are all 1 (but the row sums are not), since the conditional probability needs to sum up to 1, just like the probability. Let us work out the probability of the patient having HIV if the test comes back positive, i.e., $P(H = 1 \mid D_1 = 1)$. Obviously this is going to depend on how common the disease is, since it affects the number of false alarms. Assume that the population is quite healthy, e.g., $P(H=1) = 0.0015$. To apply Bayes' theorem, we need to apply marginalization and the multiplication rule to determine

Perhatikan bahwa jumlah kolom semuanya 1 (tetapi jumlah baris tidak), karena probabilitas bersyarat harus berjumlah 1, seperti probabilitas. Mari kita hitung kemungkinan pasien mengidap HIV jika hasil tesnya positif, yaitu $P(H = 1 \mid D_1 = 1)$. Jelas ini akan tergantung pada seberapa umum penyakit itu, karena itu mempengaruhi jumlah alarm palsu (*false alarm*). Asumsikan bahwa populasinya cukup sehat, misalnya $P(H = 1) = 0,0015$. Untuk menerapkan teorema Bayes, kita perlu menerapkan marginalisasi dan aturan perkalian untuk menentukan

$$\begin{aligned}
&P(D_1 = 1) \\
=& P(D_1=1, H=0) + P(D_1=1, H=1)  \\
=& P(D_1=1 \mid H=0) P(H=0) + P(D_1=1 \mid H=1) P(H=1) \\
=& 0.011485.
\end{aligned}
$$

Sehingga kita dapatkan 

$$\begin{aligned}
&P(H = 1 \mid D_1 = 1)\\ =& \frac{P(D_1=1 \mid H=1) P(H=1)}{P(D_1=1)} \\ =& 0.1306 \end{aligned}.$$

Dengan kata lain, hanya ada 13,06% kemungkinan penderita
sebenarnya mengidap HIV, meskipun menggunakan tes yang sangat akurat.
Seperti yang bisa kita lihat, probabilitas bisa jadi berlawanan dengan intuisi.

Apa yang harus dilakukan pasien setelah menerima berita menakutkan seperti itu? Kemungkinan besar, pasien
akan meminta dokter untuk melakukan tes lain untuk mendapatkan kejelasan. Tes kedua
memiliki karakteristik yang berbeda dan tidak sebaik yang pertama, seperti yang ditunjukkan pada :numref:`conditional_prob_D2`.

:Probabilitas bersayarat dari $P(D_2 \mid H)$.

| Probabilitas bersyarat | $H=1$ | $H=0$ |
|---|---|---|
|$P(D_2 = 1 \mid H)$|            0.98 |         0.03 |
|$P(D_2 = 0 \mid H)$|            0.02 |         0.97 |
:label:`conditional_prob_D2`

Sayangnya, hasil tes kedua juga positif.
Mari kita menghitung probabilitas yang diperlukan untuk menggunakan teorema Bayes
dengan mengasumsikan independensi bersyarat:


$$\begin{aligned}
&P(D_1 = 1, D_2 = 1 \mid H = 0) \\
=& P(D_1 = 1 \mid H = 0) P(D_2 = 1 \mid H = 0)  \\
=& 0.0003,
\end{aligned}
$$

$$\begin{aligned}
&P(D_1 = 1, D_2 = 1 \mid H = 1) \\
=& P(D_1 = 1 \mid H = 1) P(D_2 = 1 \mid H = 1)  \\
=& 0.98.
\end{aligned}
$$

Sekarang kita bisa terapkan marginalisasi dan aturan perkalian:

$$\begin{aligned}
&P(D_1 = 1, D_2 = 1) \\
=& P(D_1 = 1, D_2 = 1, H = 0) + P(D_1 = 1, D_2 = 1, H = 1)  \\
=& P(D_1 = 1, D_2 = 1 \mid H = 0)P(H=0) + P(D_1 = 1, D_2 = 1 \mid H = 1)P(H=1)\\
=& 0.00176955.
\end{aligned}
$$

Pada akhirnya, probabilitas pasien mengidap HIV dengan diketahui dua tes positif adalah 

$$\begin{aligned}
&P(H = 1 \mid D_1 = 1, D_2 = 1)\\
=& \frac{P(D_1 = 1, D_2 = 1 \mid H=1) P(H=1)}{P(D_1 = 1, D_2 = 1)} \\
=& 0.8307.
\end{aligned}
$$

Artinya, tes kedua memungkinkan kita mendapatkan keyakinan yang jauh lebih tinggi bahwa tidak semuanya baik-baik saja. Meskipun tes kedua kurang akurat dibandingkan yang pertama, pengujian ini masih meningkatkan estimasi kita secara signifikan.

## Ekspektasi dan Varian

Untuk meringkas karakteristik kunci dari distribusi probabilitas,
kita memerlukan suatu ukuran.

Ekspektasi (atau rata-rata) dari variabel acak $X$ dituliskan sebagai 

$$E[X] = \sum_{x} x P(X = x).$$

Ketika masukan dari fungsi $f(x)$ adalah variabel acak yang diambil dari distribusi $P$ dengan nilai $x$ yang beragam,
ekspektasi dari $f(x)$ dapat dihitung sebagai 

$$E_{x \sim P}[f(x)] = \sum_x f(x) P(x).$$

Dalam banyak kasus kita ingin menghitung seberapa banyak variabel acak $X$ menyimpang dari ekspektasinya. Ini bisa dikuantifikasi dengan varian

$$\mathrm{Var}[X] = E\left[(X - E[X])^2\right] =
E[X^2] - E[X]^2.$$

Akar kuadrat dari varian disebut dengan deviasi standar (*standard deviation*).
Varian fungsi dari variabel acak mengukur
seberapa besar fungsi menyimpang dari ekspektasi fungsi tersebut,
ketika beragam nilai $x$ dari variabel acak diambil dari distribusinya:

$$\mathrm{Var}[f(x)] = E\left[\left(f(x) - E[f(x)]\right)^2\right].$$

## Ringkasan

* Kita bisa mengambil sampel dari distribusi probabilitas.
* Kita bisa menganalisa banyak variabel acak dengan distribusi gabungan, distribusi bersyarat, Teorema Bayes, marginalisasi dan asumsi independensi.
* Ekspektasi dan varian menawarkan ukuran yang berguna untuk meringkas karakteristik kunci dari distribusi probabilitas.


## Latihan

1. Kita melakukan $m=500$ kelompok eksperimen di mana setiap kelompok mengambil $n=10$ sampel. Variasikan $m$ dan $n$. Amati dan analisa hasil eksperimen.
1. Diberikan dua kejadian dengan probabilitas $P(\mathcal{A})$ dan $P(\mathcal{B})$, hitung batas atas dan bawah dari $P(\mathcal{A} \cup \mathcal{B})$ 
dan $P(\mathcal{A} \cap \mathcal{B})$. (Petunjuk: gambarkan situasi ini dengan [Diagram Venn](https://en.wikipedia.org/wiki/Venn_diagram).)
1. Asumsikan kita memiliki urutan variabel-variabel acak, katakanlah $A$, $B$, dan $C$, di mana $B$ hanya tergantung pada $A$, dan $C$ hanya tergantung pada $B$, dapatkah anda menyederhanakan probabilitas gabungan $P(A, B, C)$? (Petunjuk: ini adalah [Rantai Markov](https://en.wikipedia.org/wiki/Markov_chain).)
1. Dalam :numref:`subsec_probability_hiv_app`, tes pertama lebih akurat. Kenapa tidak menjalankan tes pertama dua kali daripada menjalankan tes pertama dan kedua?


:begin_tab:`mxnet`
[Diskusi](https://discuss.d2l.ai/t/36)
:end_tab:

:begin_tab:`pytorch`
[Diskusi](https://discuss.d2l.ai/t/37)
:end_tab:

:begin_tab:`tensorflow`
[Diskusi](https://discuss.d2l.ai/t/198)
:end_tab:

