# Proyek Pipeline End-to-End Databricks (Tugas 2 Big Data)

Repository ini berisi semua *notebook* yang digunakan untuk memenuhi tugas "End-to-End Databricks Platform". Proyek ini mendemonstrasikan pembangunan *pipeline* data lengkap, mulai dari ingesti data mentah, ETL, pelatihan model, *deployment* API *real-time*, hingga *dashboarding*.

**Studi Kasus:** Prediksi *Customer Churn* (Telco)
**Nilai Tambah (Opsional):** Klasifikasi Gambar Dokumen (KTP & Tagihan)

---

## 🚀 Komponen & Teknologi Utama yang Digunakan

* **Delta Live Tables (DLT):** Untuk membangun *pipeline* ETL (Bronze, Silver, Gold) yang deklaratif dan inkremental.
* **Databricks Jobs:** Untuk otomatisasi *pipeline*, termasuk penjadwalan berbasis waktu dan pemicu "File Arrival".
* **MLflow:** Untuk *Experiment Tracking*, *logging* metrik/plot, dan *Model Registry* (Versioning & Aliasing).
* **Databricks Model Serving:** Untuk mendeploy dua model (prediksi *churn* dan klasifikasi gambar) sebagai *endpoint* API REST *serverless*.
* **Databricks Volumes:** Untuk menyimpan *dataset* gambar (data tidak terstruktur).
* **Scikit-learn & PyTorch:** Untuk melatih model *machine learning* (Logistic Regression) dan *deep learning* (ResNet).
* **Postman:** Untuk pengujian *endpoint* API yang sudah di-*deploy*.

---

## 📁 Deskripsi Notebook

Repository ini berisi empat *notebook* utama:

### 1. `dlt_pipeline_churn.ipynb`
* **Tujuan:** Mendefinisikan *pipeline* ETL.
* **Isi:** Berisi kode Python dan dekorator `@dlt.table` untuk Arsitektur Medallion.
    * **Bronze Layer:** Membaca data CSV mentah secara inkremental dari *storage* menggunakan *Auto Loader* (`cloudFiles`).
    * **Silver Layer:** Membersihkan data (misal: konversi tipe data, menangani *null*) dan menerapkan aturan *data quality* (`@dlt.expect`).
    * **Gold Layer:** Mengagregasi data bersih menjadi tabel yang siap untuk *dashboarding*.

### 2. `Model_ML_Churn.ipynb`
* **Tujuan:** Pelatihan, evaluasi, dan *deployment* model prediksi *churn*.
* **Isi:** *Pipeline* MLOps lengkap.
    * **Pemuatan Data:** Membaca data bersih dari tabel `churn_silver` DLT.
    * **EDA Visual:** Analisis data eksploratif dengan *heatmap* korelasi, *boxplot* *outlier*, dan plot fitur kategorikal.
    * **Preprocessing:** Membuat `Pipeline` sklearn dengan `ColumnTransformer` untuk menangani fitur numerik (StandardScaler) dan kategorikal (OneHotEncoder).
    * **Pelatihan & Evaluasi:** Melatih model `LogisticRegression` dan mencatat metrik lengkap (Classification Report, Confusion Matrix, ROC-AUC Curve) ke MLflow.
    * **Logging:** Mendaftarkan model terbaik ke *Model Registry* dengan *signature* yang benar.

### 3. `Model_Gambar_Churn.ipynb`
* **Tujuan:** Tugas nilai tambah (opsional) untuk klasifikasi gambar.
* **Isi:**
    * **Pemuatan Data:** Membaca data gambar (`.jpg`) dari **Databricks Volume** (`/Volumes/workspace/churn_analytics/image_store`).
    * **Pelatihan:** Menggunakan `PyTorch` dan `torchvision` untuk melatih model klasifikasi gambar (ResNet18) dengan teknik *transfer learning*.
    * **Deployment Wrapper:** Berisi *class* `ImageModelWrapper` kustom untuk menangani input `Base64` dari API.
    * **Logging:** Mendaftarkan model *deep learning* ke *Model Registry*.

### 4. `Exploratory Data.ipynb`
* **Tujuan:** Eksplorasi data awal.
* **Isi:** Berisi kueri `%sql` sederhana yang digunakan untuk memvalidasi dan menjelajahi data mentah `wa_fn_use_c_telco_customer_churn` sebelum *pipeline* DLT dibuat.

---

## 📋 Cara Menggunakan

*Notebook* ini dirancang untuk diimpor dan dijalankan di dalam Databricks Workspace.

1.  Impor *notebook* `.ipynb` ini ke Databricks Workspace Anda.
2.  (Opsional) Ekspor sebagai *file* `.py` (Source File) jika Anda ingin menjalankannya sebagai *job* atau DLT *pipeline*.
3.  Pastikan *path* (seperti `csv_source_path` atau `IMAGE_PATH`) dan nama tabel (`workspace.churn_analytics...`) disesuaikan dengan konfigurasi *schema* Anda.

---

## 👥 Tim Proyek

* **Balqis Eka Nurfadisyah** (1202220223)
* **Alisha Deanova Oemar** (1202223105)
* **Fathya Ariyani** (102022400096)

**Mata Kuliah:** Big Data
**Kode Dosen:** FQH
