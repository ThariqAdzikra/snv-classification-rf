# Pengembangan Model Klasifikasi Single Nucleotide Variant Menggunakan Algoritma Random Forest

Repositori ini memuat kode komputasi dan alur kerja (*pipeline*) penelitian skripsi mengenai klasifikasi patogenisitas *Single Nucleotide Variant* (SNV) berbasis pembelajaran mesin (*Random Forest*), serta implementasi skrining varian novel pada data *Whole Genome Sequencing* (WGS).

---

## Informasi Skripsi

- **Judul Skripsi:** Pengembangan Model Klasifikasi Single Nucleotide Variant Menggunakan Algoritma Random Forest
- **Peneliti / Penulis:** M. Thariq Adzikra
- **NIM:** 2303113029
- **Dosen Pembimbing:** Tisha Melia, B.Sc., M.Sc., Ph.D.
- **Program Studi / Fakultas:** Jurusan Ilmu Komputer, Fakultas Matematika dan Ilmu Pengetahuan Alam
- **Perguruan Tinggi:** Universitas Riau

---

## Ringkasan Penelitian

Identifikasi dampak fungsional mutasi titik tunggal (*Single Nucleotide Variant* / SNV) memegang peranan krusial dalam bioinformatika klinis. Proyek ini mengembangkan model prediktif berbasis *Random Forest* untuk membedakan varian genomik patogenik (*disease-causing*) dan jinak (*benign*).

### Karakteristik Utama Pipeline:
1. **Penyusunan Data Latih:** Data varian terkurasi dari basis data **NCBI ClinVar** yang dianotasi menggunakan metrik fungsional *in-silico* dari **dbNSFP v5.3a** (pada koordinat perakitan genom GRCh38).
2. **7 Fitur Prediktor Inti:**
   - Konservasi evolusioner & fungsional: `CADD`, `SIFT`, `PolyPhen-2`, `REVEL`, `MetaSVM`, `PhyloP`.
   - Frekuensi populasi: `MAF` (*Minor Allele Frequency*).
3. **Skenario Eksperimen:**
   - **Klasifikasi 2-Kelas:** *Pathogenic* vs *Benign*.
   - **Klasifikasi 4-Kelas:** *Pathogenic*, *Likely pathogenic*, *Benign*, *Likely benign*.
   - Evaluasi penyeimbangan kelas: Tanpa SMOTE (*imbalanced baseline*) vs Menggunakan SMOTE (*Synthetic Minority Over-sampling Technique*).
4. **Skrining Varian Novel:** Inferensi model terhadap varian baru non-ClinVar dari 227 sampel WGS populasi Jawa Barat (*West Java cohort*).
5. **Analisis Spasial PCA Terpadu:** Proyeksi koordinat varian novel ke dalam ruang koordinat referensi ClinVar (*Unified Coordinate Space*) untuk menganalisis keterpisahan sebaran secara visual.

---

## Alur Kerja Notebook (`notebooks/`)

Seluruh kode eksperimen disusun secara modular dan berurutan di dalam direktori `notebooks/`:

| No. | File Notebook | Deskripsi Tahapan |
| :---: | :--- | :--- |
| **01** | [`01_download_genomic_data.ipynb`](notebooks/01_download_genomic_data.ipynb) | Otomasi pengunduhan berkas genomik referensi (ClinVar, dbNSFP v5.3a, dan WGS West Java VCF). |
| **02** | [`02_pre_processing_clinvar.ipynb`](notebooks/02_pre_processing_clinvar.ipynb) | Penyaringan awal ClinVar, pemfilteran *assembly* GRCh38, dan isolasi varian tipe SNV. |
| **03** | [`03_annotate_dbnsfp.ipynb`](notebooks/03_annotate_dbnsfp.ipynb) | Proses anotasi fitur fungsional dari dbNSFP berdasarkan pencocokan posisi kromosom dan alel. |
| **04** | [`04_machine_learning_2class_nosmote.ipynb`](notebooks/04_machine_learning_2class_nosmote.ipynb)<br>[`04c_machine_learning_2class_smote.ipynb`](notebooks/04c_machine_learning_2class_smote.ipynb)<br>[`04_machine_learning_4class_nosmote.ipynb`](notebooks/04_machine_learning_4class_nosmote.ipynb)<br>[`04_machine_learning_4class_smote.ipynb`](notebooks/04_machine_learning_4class_smote.ipynb) | Pelatihan algoritma *Random Forest*, seleksi fitur, evaluasi performa metrik (Akurasi, F1-Score, ROC-AUC), dan penyimpanan model. |
| **05** | [`05_inference_2class_nosmote.ipynb`](notebooks/05_inference_2class_nosmote.ipynb) | Ekstraksi SNV dari VCF WGS Jawa Barat, anotasi cepat, prediksi patogenisitas, dan pemeringkatan *Risk Score*. |
| **06** | [`06_pca_analysis.ipynb`](notebooks/06_pca_analysis.ipynb) | Analisis reduksi dimensi PCA gabungan (proyeksi WGS ke ruang acuan ClinVar) dengan visualisasi *overlay* dan *side-by-side*. |

---

## Sumber Data (*Data Sources*)

Penelitian ini menggunakan dataset publik terverifikasi yang dapat diakses secara terbuka:

1. **NCBI ClinVar**
   - Basis data variasi genomik dan hubungannya dengan kesehatan manusia.
   - Berkas: `variant_summary.txt.gz` (GRCh38)
   - Tautan Unduh: `https://ftp.ncbi.nlm.nih.gov/pub/clinvar/tab_delimited/variant_summary.txt.gz`
2. **dbNSFP (Database for Functional Annotation of Non-synonymous SNVs)**
   - Pangkalan data anotasi fungsional untuk varian *missense* / non-sinonim.
   - Versi: v5.3a untuk genom GRCh38
   - Tautan Unduh: `https://dist.genos.us/academic/b2fd38/`
3. **Whole Genome Sequencing (WGS) Jawa Barat**
   - Data pengurutan genom 227 individu populasi Indonesia.
   - Nomor Aksesi: European Nucleotide Archive (ENA) `ERZ25039609`
   - Tautan Unduh: `https://ftp.sra.ebi.ac.uk/vol1/analysis/ERZ250/ERZ25039609/WGS_WestJava_227.vcf.gz`

*Catatan: Berkas mentah dataset genomik berukuran sangat besar (puluhan GB) sehingga tidak disimpan di repositori Git (dikecualikan lewat `.gitignore`). Seluruh data dapat diunduh otomatis melalui notebook tahap 01.*

---

## Struktur Repositori

```text
snv-classification-rf/
├── notebooks/
│   ├── 01_download_genomic_data.ipynb
│   ├── 02_pre_processing_clinvar.ipynb
│   ├── 03_annotate_dbnsfp.ipynb
│   ├── 04_machine_learning_2class_nosmote.ipynb
│   ├── 04c_machine_learning_2class_smote.ipynb
│   ├── 04_machine_learning_4class_nosmote.ipynb
│   ├── 04_machine_learning_4class_smote.ipynb
│   ├── 05_inference_2class_nosmote.ipynb
│   └── 06_pca_analysis.ipynb
├── .gitignore
├── LICENSE
├── README.md
└── requirements.txt
```

---

## Petunjuk Penggunaan

### 1. Kebutuhan Sistem
- Python versi 3.9 ke atas (disarankan 3.10 atau 3.11).
- Ruang penyimpanan kosong sekurang-kurangnya 50 GB jika ingin menyimpan dataset ClinVar dan dbNSFP secara lokal.
- Disarankan menggunakan Google Colab / server komputasi dengan RAM memadai untuk pemrosesan file genomik dbNSFP.

### 2. Instalasi Lingkungan
```bash
# Salin repositori
git clone https://github.com/USERNAME/snv-classification-rf.git
cd snv-classification-rf

# Buat dan aktifkan virtual environment
python -m venv .venv
# Pada Windows:
.venv\Scripts\activate
# Pada Linux / macOS:
source .venv/bin/activate

# Pasang pustaka dependensi
pip install -r requirements.txt
```

### 3. Menjalankan Kode
Buka antarmuka Jupyter:
```bash
jupyter lab
```
Jalankan berkas notebook secara berurutan mulai dari `01` hingga `06`. Setiap notebook telah dilengkapi mekanisme *checkpoint* penyimpanan parsial untuk menjaga persistensi data saat proses komputasi panjang.

---

## Lisensi

Proyek kode ini dilisensikan di bawah ketentuan [MIT License](LICENSE).
