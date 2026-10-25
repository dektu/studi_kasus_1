# Pengelompokan Motif Ukiran Tradisional Bali Menggunakan Unsupervised Clustering

Repositori ini memuat implementasi *Unsupervised Machine Learning* untuk mengelompokkan ragam citra motif ukiran tradisional Bali ke dalam 5 kelompok motif utama (**Barong**, **Goak**, **Gajah**, **Daun**, dan **Cina**) menggunakan perbandingan tiga algoritma: **K-Means**, **Tree (Agglomerative Hierarchical)**, dan **DBSCAN**.

---

## Struktur Direktori

```text
studiKasus1/
├── dataset/                     # Tempat meletakkan dataset citra asli BaliCarving (Diabaikan oleh Git)
├── output_data/                 # Folder output otomatis (Diabaikan oleh Git, dibuat saat notebook jalan)
├── kode_program/
│   ├── 01_preprocessing.ipynb        # Tahap 1: Cek dataset, EDA, Resize 224x224 & CLAHE
│   ├── 02_ekstraksi_fitur.ipynb      # Tahap 2: Ekstraksi fitur CNN + L2 Normalization + PCA (50 Komponen)
│   └── 03_pengujian_clustering.ipynb # Tahap 3: Pengujian K-Means vs Tree vs DBSCAN & Evaluasi Label Asli
├── .gitignore                   # Mengabaikan dataset/ dan output_data/ agar tidak ter-push ke GitHub
├── requirements.txt             # Daftar dependensi pustaka Python
└── README.md                    # Dokumentasi proyek
```

---

## Petunjuk Instalasi & Menjalankan Program

### 1. Kloning Repositori & Instalasi Dependensi
```bash
git clone <URL_REPOSITORI_ANDA>
cd studiKasus1
pip install -r requirements.txt
```

### 2. Menyiapkan Dataset
Letakkan folder dataset `BaliCarving` yang berisi citra ukiran di dalam folder `dataset/`:
```text
dataset/
└── BaliCarving/
    ├── JPEGImages/                       # 2.364 citra ukiran (.jpg)
    └── classification_*_ukiran.csv       # File anotasi multi-label
```

### 3. Eksekusi Notebook secara Berurutan
Folder `output_data/` beserta seluruh isinya akan dibuat secara otomatis saat Anda menjalankan notebook berikut di VS Code atau JupyterLab:

1. **Jalankan `kode_program/01_preprocessing.ipynb`**:
   - Memindai 2.364 citra dan menyaring ke 5 motif pembeda utama.
   - Menghasilkan `output_data/data_ukiran.csv`.
   - Melakukan *Resize* ke $224 \times 224$ piksel dan peningkatan kontras *CLAHE*, lalu menyimpannya ke folder fisik `output_data/citra_preprocessed/`.
2. **Jalankan `kode_program/02_ekstraksi_fitur.ipynb`**:
   - Membaca citra bersih dari `output_data/citra_preprocessed/`.
   - Mengekstrak representasi visual mendalam menggunakan Pretrained CNN (MobileNetV2).
   - Menerapkan *L2 Normalization* dan reduksi dimensi *PCA* ke 50 komponen utama.
   - Menyimpan matriks fitur numerik langsung ke `output_data/fitur_pca.csv`.
3. **Jalankan `kode_program/03_pengujian_clustering.ipynb`**:
   - Membaca `output_data/fitur_pca.csv`.
   - Melakukan pencarian parameter optimal (Elbow Method, Dendrogram Ward, K-Distance DBSCAN).
   - Menghitung metrik internal (Silhouette Score, DBI, CHI).
   - Mencocokkan klaster dengan label asli menggunakan *Hungarian Algorithm* untuk menghitung Akurasi/Purity (%), Precision, Recall, F1-Score, Confusion Matrix, dan scatter plot 2D t-SNE.

