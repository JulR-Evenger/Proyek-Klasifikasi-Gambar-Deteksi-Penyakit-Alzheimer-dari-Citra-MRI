# Proyek Klasifikasi Gambar: Deteksi Penyakit Alzheimer dari Citra MRI


## Ringkasan Proyek (TL;DR)
Proyek ini bertujuan untuk membangun model *Deep Learning* yang mampu mengklasifikasikan citra pemindaian otak (MRI) untuk mendeteksi penyakit Alzheimer. Model *Convolutional Neural Network* (CNN) dibangun menggunakan TensorFlow/Keras untuk membedakan antara otak sehat dan yang menunjukkan tanda-tanda Alzheimer. Model ini berhasil mencapai **akurasi 100%** pada data uji dan disimpan dalam format `SavedModel`, `TFLite`, dan `TensorFlow.js` untuk potensi implementasi di berbagai platform.

**Tags:** `Python`, `TensorFlow`, `Keras`, `Scikit-Learn`, `CNN`, `Computer Vision`, `Klasifikasi Gambar`, `Kesehatan`

---

## Daftar Isi
1. [Latar Belakang Masalah](#latar-belakang-masalah)
2. [Tujuan Proyek](#tujuan-proyek)
3. [Dataset](#dataset)
4. [Alur Kerja Proyek (Workflow)](#alur-kerja-proyek)
5. [Hasil & Evaluasi](#hasil--evaluasi)
6. [Deployment & Format Model](#deployment--format-model)
7. [Kesimpulan](#kesimpulan)

---

### Latar Belakang Masalah
Penyakit Alzheimer adalah gangguan neurodegeneratif yang memengaruhi jutaan orang di seluruh dunia. Deteksi dini sangat krusial untuk memperlambat progresivitas penyakit dan meningkatkan kualitas hidup pasien. Analisis manual citra MRI oleh ahli radiologi memakan waktu dan bersifat subjektif. Oleh karena itu, diperlukan sebuah sistem otomatis berbasis AI untuk membantu proses diagnosis secara cepat dan akurat.

### Tujuan Proyek
* **Tujuan Utama:** Mengembangkan model klasifikasi gambar menggunakan CNN untuk secara otomatis mendeteksi keberadaan Alzheimer dari citra MRI otak.
* **Target Performa:** Mencapai akurasi validasi dan pengujian di atas 90% untuk memastikan keandalan model.

### Dataset
* **Sumber Data:** Dataset yang digunakan adalah **"Alzheimer's Detection Dataset"**, yang berisi kumpulan citra MRI otak yang telah dilabeli.
* **Pembagian Data:** Dataset dibagi menjadi tiga bagian dengan rasio:
    * **Data Latih (Training):** 70% (988 gambar)
    * **Data Validasi (Validation):** 15% (192 gambar)
    * **Data Uji (Testing):** 15% (192 gambar)
* **Kelas:** Terdapat 2 kelas klasifikasi utama yang digunakan dalam model ini.

### Alur Kerja Proyek (Workflow)
Proyek ini mengikuti alur kerja standar dalam pengembangan model *computer vision*:

1.  **Persiapan Data:**
    * Dataset dimuat dan direktori diatur untuk data latih, validasi, dan uji.
    * **Augmentasi Gambar:** Untuk mencegah *overfitting* dan memperkaya variasi data, teknik augmentasi diterapkan pada data latih menggunakan `ImageDataGenerator`. Augmentasi mencakup rotasi, pergeseran (lebar & tinggi), *shear*, *zoom*, dan *horizontal flip*.
    * Gambar diubah ukurannya menjadi `224x224` piksel dan dinormalisasi dengan skala `1./255`.

2.  **Arsitektur Model (CNN):**
    * Model sekuensial dibangun dengan arsitektur CNN sederhana namun efektif:
        * **Layer Konvolusi 1:** 32 filter, kernel (3,3), aktivasi ReLU.
        * **Layer MaxPooling 1:** Ukuran pool (2,2).
        * **Layer Konvolusi 2:** 64 filter, kernel (3,3), aktivasi ReLU.
        * **Layer MaxPooling 2:** Ukuran pool (2,2).
        * **Flatten Layer:** Mengubah data menjadi vektor 1D.
        * **Dense Layer 1 (Hidden):** 256 neuron, aktivasi ReLU.
        * **Dropout Layer:** Mencegah *overfitting* dengan rate 0.5.
        * **Dense Layer 2 (Output):** Jumlah neuron sesuai jumlah kelas, aktivasi `softmax` untuk klasifikasi multikelas.

3.  **Pelatihan Model:**
    * **Optimizer:** `Adam`.
    * **Loss Function:** `categorical_crossentropy`, sesuai untuk masalah klasifikasi.
    * **Callbacks:**
        * `EarlyStopping`: Menghentikan pelatihan jika akurasi validasi tidak meningkat setelah 5 *epoch* untuk efisiensi.
        * `ReduceLROnPlateau`: Mengurangi *learning rate* jika *loss* validasi stagnan.
    * Model dilatih selama 30 *epoch* dengan *batch size* 32.

### Hasil & Evaluasi
Model menunjukkan performa yang sangat baik. Pelatihan berhenti lebih awal pada **epoch ke-6** karena `EarlyStopping`, yang menandakan model mencapai performa puncak dengan cepat.

* **Akurasi & Loss:**
    * Grafik menunjukkan bahwa akurasi validasi secara konsisten mencapai **100%**, sementara *loss* validasi tetap sangat rendah. Ini menandakan model dapat menggeneralisasi dengan sempurna pada data yang belum pernah dilihat sebelumnya (set validasi).
    ![Grafik Akurasi dan Loss](https://raw.githubusercontent.com/user-attachments/assets/189d9e4a-99ae-4d2a-a2ca-1cfd330c6a30)

* **Evaluasi Akhir:**
    * **Training Set:** Akurasi ~90.6%
    * **Testing Set:** **Akurasi 100.0%** dengan *loss* 0.20

Hasil ini melebihi target performa awal dan menunjukkan bahwa model yang dikembangkan sangat andal untuk tugas klasifikasi ini.

### Deployment & Format Model
Untuk memastikan model dapat digunakan di berbagai platform, model akhir diekspor ke dalam beberapa format:

1.  **SavedModel:** Format standar TensorFlow untuk production-level serving.
2.  **TensorFlow Lite (`.tflite`):** Format yang dioptimalkan dan ringan, ideal untuk aplikasi mobile (Android/iOS) dan perangkat IoT. Disertai dengan `label.txt`.
3.  **TensorFlow.js:** Format untuk menjalankan model secara langsung di browser atau pada aplikasi berbasis JavaScript (Node.js).

### Kesimpulan
Proyek ini berhasil mengembangkan sebuah model CNN yang akurat dan efisien untuk mendeteksi penyakit Alzheimer dari citra MRI. Dengan akurasi 100% pada data uji dan ketersediaan dalam berbagai format, model ini memiliki potensi besar untuk diintegrasikan ke dalam sistem pendukung keputusan klinis, membantu para profesional medis dalam melakukan diagnosis yang lebih cepat dan objektif.
