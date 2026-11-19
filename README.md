# Analisis Kualitas Tidur, Olahraga, dan Risiko Gangguan Panik dengan Decision Tree dan CRISP-DM

![Python](https://img.shields.io/badge/Python-3-blue)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626)
![pandas](https://img.shields.io/badge/pandas-Data_Preparation-150458)
![scikit-learn](https://img.shields.io/badge/scikit--learn-Decision_Tree-F7931E)
![CRISP-DM](https://img.shields.io/badge/Metode-CRISP--DM-informational)

Klasifikasi **diagnosis gangguan panik** (0 = tidak, 1 = ya) pada **100 partisipan** menggunakan algoritma **Decision Tree**, dengan alur kerja mengikuti kerangka **CRISP-DM**. Faktor gaya hidup, yaitu kualitas tidur dan olahraga, ikut dipakai sebagai salah satu fitur bersama riwayat keluarga, tingkat stres, gejala, dan dampak pada kehidupan.

> Proyek ini untuk pembelajaran data science dan **bukan alat diagnosis medis**.

---

## Dataset

File `DataMentah.csv` berisi **100 baris dan 17 kolom**:

`Participant ID`, `Age`, `Gender`, `Family History`, `Personal History`, `Current Stressors`, `Symptoms`, `Severity`, `Impact on Life`, `Demographics`, `Medical History`, `Psychiatric History`, `Substance Use`, `Coping Mechanisms`, `Social Support`, `Lifestyle Factors`, `Panic Disorder Diagnosis`

**Target:** `Panic Disorder Diagnosis` (0 / 1)

**Fitur yang dipakai (8 kolom):**

| Fitur | Nilai |
|---|---|
| Family History | Yes / No |
| Personal History | Yes / No |
| Current Stressors | Low / Moderate / High |
| Symptoms | Panic attacks, Chest pain, Dizziness, Fear of losing control |
| Severity | Mild / Moderate / Severe |
| Impact on Life | Mild / Moderate / Significant |
| Psychiatric History | Anxiety disorder, Depressive disorder, Bipolar disorder, No |
| Lifestyle Factors | Sleep quality / Exercise |

---

## Alur CRISP-DM

| Fase | Yang dilakukan |
|---|---|
| **1. Business Understanding** | Menilai apakah riwayat, stres, gejala, dan gaya hidup (kualitas tidur dan olahraga) dapat dipakai untuk memprediksi diagnosis gangguan panik |
| **2. Data Understanding** | Memuat data, mengecek missing values (kolom `Psychiatric History` kosong pada 28 baris) dan duplikasi (1 baris) |
| **3. Data Preparation** | Seleksi 8 fitur + target, isi missing `Psychiatric History` dengan `No`, hapus 1 baris duplikat, hapus data `Lifestyle Factors = Diet` agar analisis fokus pada tidur dan olahraga, ubah kolom ke tipe kategori lalu encoding ke angka |
| **4. Modeling** | `DecisionTreeClassifier` (parameter default), split stratified dengan `random_state=42` |
| **5. Evaluation** | Confusion matrix, accuracy, precision, recall, dan F1-score |
| **6. Deployment** | Belum dilakukan |

**Jumlah data per tahap:** 100 baris awal → 99 setelah hapus duplikat → **82** setelah hapus `Diet` → **32 data latih** dan **50 data uji**.

---

## Hasil Evaluasi

**Confusion matrix** (50 data uji):

| | Prediksi 1 | Prediksi 0 |
|---|---|---|
| **Aktual 1** | TP = 22 | FN = 2 |
| **Aktual 0** | FP = 3 | TN = 23 |

| Metrik | Nilai |
|---|---|
| Accuracy | 90,00% |
| Precision | 88,00% |
| Recall | 91,67% |
| F1-Score | 89,80% |

---

## Cara Menjalankan

### Opsi 1: Google Colab

1. Buka file `.ipynb` di Google Colab
2. Upload `DataMentah.csv` ke sesi Colab (ikon folder di sebelah kiri, lalu upload)
3. Klik **Runtime → Run all**

### Opsi 2: Komputer sendiri

```bash
git clone https://github.com/hdibilashafa/NAMA-REPO.git
cd NAMA-REPO
pip install pandas scikit-learn jupyter
jupyter notebook
```

Pastikan `DataMentah.csv` berada di **folder yang sama** dengan notebook, lalu jalankan semua sel secara berurutan.

---

## Struktur Repository

```
.
├── analisis_gangguan_panik_decision_tree.ipynb
├── DataMentah.csv
└── README.md
```

---

## Tech Stack

Python · pandas · scikit-learn (DecisionTreeClassifier, train_test_split, metrik klasifikasi) · Jupyter Notebook

---

## Keterbatasan

- **Data sangat kecil.** Setelah pembersihan hanya ada 82 data, dengan data latih 32 dan data uji 50 (`test_size=0.6`). Satu prediksi yang salah mengubah akurasi sebesar 2 poin persen, jadi hasilnya belum stabil
- **Decision Tree tanpa `random_state` dan tanpa tuning hyperparameter**, sehingga hasil bisa sedikit berbeda setiap notebook dijalankan ulang
- **Akurasi tinggi belum berarti tidur dan olahraga berpengaruh.** Fitur `Symptoms`, `Severity`, dan `Impact on Life` bisa jadi merupakan akibat dari gangguan panik, bukan faktor risikonya. Notebook belum menganalisis pentingnya tiap fitur secara terpisah
- **`Lifestyle Factors` adalah satu kolom kategori** (Sleep quality / Exercise), bukan ukuran durasi tidur atau frekuensi olahraga, dan tiap partisipan hanya punya satu nilai
- Fase **Deployment** CRISP-DM belum dikerjakan
- Hasil tidak boleh dipakai untuk diagnosis atau keputusan medis
