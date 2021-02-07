# Instalasi
:label:`chap_installation`

Untuk membuat Anda siap untuk mendapatkan pengalaman belajar langsung,
kami perlu menyiapkan untuk Anda lingkungan untuk menjalankan Python,
Notebook Jupyter, pustaka yang relevan,
dan kode yang diperlukan untuk menjalankan buku itu sendiri.

## Menginstal *Miniconda*

Cara termudah untuk memulainya adalah dengan menginstal
[Miniconda](https://conda.io/en/latest/miniconda.html). Python versi 3.x. diperlukan. Anda dapat melewati langkah-langkah berikut jika *conda* telah diinstal.
Unduh file *sh* *Miniconda* yang sesuai dari situs web
lalu jalankan penginstalan dari baris perintah
menggunakan `sh <FILENAME> -b`. Untuk pengguna macOS:

```bash
# Nama file mungkin berubah sewaktu-waktu
sh Miniconda3-latest-MacOSX-x86_64.sh -b
```

Bagi pengguna Linux:

```bash
# Nama file mungkin berubah sewaktu-waktu
sh Miniconda3-latest-Linux-x86_64.sh -b
```

Selanjutnya, menginisialisasi *shell* agar kita bisa menjalankan `conda` secara langsung.

```bash
~/miniconda3/bin/conda init
```

Sekarang tutup dan buka lagi *shell* Anda saat ini. Anda seharusnya bisa membuat lingkungan dengan cara berikut:

```bash
conda create --name d2l python=3.8 -y
```

## Mengunduh *Notebook* D2L

Selanjutnya, kita perlu mengunduh kode buku ini. Anda dapat mengklik "Semua
Notes" di bagian atas halaman HTML mana pun untuk mendownload dan mengekstrak kode.
Sebagai alternatif, jika Anda memiliki `unzip` (jika tidak, jalankan` sudo apt install unzip`) yang tersedia:


```bash
mkdir d2l-en && cd d2l-en
curl https://d2l.ai/d2l-en.zip -o d2l-en.zip
unzip d2l-en.zip && rm d2l-en.zip
```

Sekarang kita akan mengaktifkan lingkungan `d2l`.

```bash
conda activate d2l
```

## Menginstal *Framework* dan Paket `d2l`

Sebelum menginstal *framework* *deep learning*, periksa dulu
apakah Anda memiliki GPU yang tepat di mesin Anda atau tidak
(GPU yang dipakai untuk laptop standar
tidak termasuk dalam tujuan ini).
Jika Anda menginstal di server GPU,
lanjutkan ke :ref:`subsec_gpu` untuk instruksi
cara memasang versi *framework* yang mendukung GPU.

Jika tidak, Anda dapat menginstal versi CPU sebagai berikut.
Itu akan menjadi lebih dari cukup tenaga untuk 
melalui beberapa bab pertama tetapi Anda akan membutuhkan GPU untuk menjalankan model yang lebih besar.

:begin_tab:`mxnet`
```bash
pip install mxnet==1.7.0.post1
```
:end_tab:


:begin_tab:`pytorch`
```bash
pip install torch torchvision -f https://download.pytorch.org/whl/torch_stable.html
```
:end_tab:

:begin_tab:`tensorflow`
Anda dapat menginstal TensorFlow dengan dukungan CPU atau GPU dengan cara berikut:
```bash
pip install tensorflow tensorflow-probability
```
:end_tab:

Kita juga akan menginstal paket `d2l` yang mengandung fungsi dan kelas yang sering digunakan di buku ini.

```bash
# -U: Memutakhirkan semua paket ke versi terbaru yang tersedia
pip install -U d2l
```

Setelah semua itu diinstal, kita sekarang bisa membuka *notebook* Jupyter dengan menjalankan:

```bash
jupyter notebook
```

Pada tahap ini, Anda dapat membuka http://localhost:8888 (biasanya terbuka secara otomatis) di browser Web Anda. Kemudian kita dapat menjalankan kode untuk setiap bagian dari buku tersebut.
Harap selalu jalankan `conda activate d2l` untuk mengaktifkan lingkungan *runtime*
sebelum menjalankan kode buku atau memperbarui *framework* *deep learning* atau paket `d2l`.
Untuk keluar dari lingkungan, jalankan `conda deactivate`.


## Dukungan GPU
:label:`subsec_gpu`

:begin_tab:`mxnet`
Secara bawaan, MXNet diinstal tanpa dukungan GPU
untuk memastikan agar ia dapat berjalan di komputer manapun (termasuk kebanyakan laptop).
Sebagian kode dari buku ini membutuhkan atau merekomendasikan untuk dijalankan dengan GPU.
Jika komputer Anda memiliki kartu grafis NVIDIA dan telah menginstal [CUDA](https://developer.nvidia.com/cuda-downloads),
maka Anda harus menginstal versi yang mendukung GPU.
Jika Anda telah menginstal versi yang hanya untuk CPU,
Anda mungkin perlu menghapusnya terlebih dahulu dengan menjalankan:


```bash
pip uninstall mxnet
```

Kemudian kita perlu menemukan versi CUDA yang Anda instal.
Anda dapat memeriksanya melalui `nvcc --version` atau `cat /usr/local/cuda/version.txt`.
Asumsikan bahwa Anda telah menginstal CUDA 10.1,
maka Anda dapat menginstal dengan perintah berikut:

```bash
# Bagi pengguna Windows
pip install mxnet-cu101==1.7.0 -f https://dist.mxnet.io/python

# Bagi pengguna Linux dan macOS
pip install mxnet-cu101==1.7.0
```

Anda dapat mengubah digit terakhir sesuai dengan versi CUDA Anda, misalnya, `cu100` untuk
CUDA 10.0 dan `cu90` untuk CUDA 9.0.
:end_tab:

:begin_tab:`pytorch, tensorflow`
Secara default, *framework* *deep learning* diinstal dengan dukungan GPU.
Jika komputer Anda memiliki GPU NVIDIA dan telah menginstal [CUDA](https://developer.nvidia.com/cuda-downloads),
maka Anda sudah siap.
: end_tab:

## Latihan

1. Unduh kode untuk buku dan instal lingkungan *runtime*.

:begin_tab:`mxnet`
[Discussions](https://discuss.d2l.ai/t/23)
:end_tab:

:begin_tab:`pytorch`
[Discussions](https://discuss.d2l.ai/t/24)
:end_tab:

:begin_tab:`tensorflow`
[Discussions](https://discuss.d2l.ai/t/436)
:end_tab:

