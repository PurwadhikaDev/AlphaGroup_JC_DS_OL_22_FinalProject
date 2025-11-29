Final Project by Alpha Team

Ade Liesly
Ulfah Rosdiana
Aditya Eka Putra

# E-Commerce Customer Churn Analysis & Prediction

## Executive Summary — Proyek Prediksi Customer Churn

Proyek ini bertujuan mengembangkan solusi analitik prediktif untuk mengidentifikasi pelanggan Perusahaan XYZ yang berisiko melakukan churn. Meskipun perusahaan memiliki data historis yang komprehensif—meliputi Tenure, SatisfactionScore, riwayat Complain, OrderCount, hingga DaySinceLastOrder—saat ini belum terdapat mekanisme sistematis untuk mendeteksi pelanggan berisiko secara dini. Akibatnya, alokasi anggaran retensi menjadi kurang efektif dan banyak pelanggan bernilai tinggi hilang tanpa intervensi tepat waktu.

---

### Ringkasan Permasalahan
Perusahaan XYZ belum memiliki sistem untuk mengenali pelanggan dengan risiko churn secara proaktif, sehingga:

- Tim Marketing dan Retensi tidak dapat memprioritaskan pelanggan yang membutuhkan intervensi segera.
- Anggaran retensi digunakan untuk promosi umum yang tidak tepat sasaran.
- Pelanggan loyal dan bernilai tinggi (high CLV) hilang tanpa langkah pencegahan.

Model prediktif berbasis machine learning diperlukan untuk mendukung pengambilan keputusan berbasis data dan strategi retensi yang lebih terarah.

---

### Tujuan Proyek (Project Goals)

#### 1. Tujuan Bisnis Strategis
- Menurunkan tingkat churn dalam 6 bulan ke depan.
- Meningkatkan loyalitas pelanggan dan memperpanjang Customer Lifetime Value (CLV).

#### 2. Tujuan Analitis & Model
- Mengidentifikasi faktor-faktor utama yang menjadi pendorong churn.
- Membangun dan mengevaluasi sejumlah model klasifikasi untuk memilih model dengan performa terbaik.

#### 3. Tujuan Operasional
- Menghasilkan *Churn Risk Score* untuk setiap pelanggan aktif.
- Mendukung tim bisnis dalam memprioritaskan pelanggan yang membutuhkan intervensi cepat.

#### 4. Tujuan Finansial
- Mengoptimalkan anggaran retensi dengan menargetkan pelanggan berisiko tinggi dan bernilai tinggi.
- Meningkatkan ROI dari program retensi dan promosi.

---

### Pendekatan Analitis (Analytical Approach)

Dalam konteks bisnis, kesalahan prediksi berupa *False Negative* (pelanggan berisiko namun tidak terdeteksi) jauh lebih merugikan dibanding *False Positive*. Oleh karena itu, analisis ini menempatkan **Recall** sebagai prioritas utama.

Untuk mencapainya, proyek ini menggunakan **F2-Score** sebagai metrik utama karena memberikan bobot lebih besar pada Recall dibanding Precision.

#### Penggunaan F2-Score:
1. **Model Selection**  
   Digunakan untuk memilih model dengan sensitivitas tertinggi terhadap kasus churn.

2. **Threshold Optimization**  
   Ambang prediksi (threshold) disesuaikan berdasarkan nilai F2-Score tertinggi agar model mampu menangkap sebanyak mungkin pelanggan berisiko churn.

Pendekatan ini memastikan model yang dikembangkan selaras dengan tujuan bisnis: meminimalkan pelanggan yang terlepas dari deteksi (missed churn) dan meningkatkan efektivitas intervensi retensi.

---

## Data Understanding & Data Cleaning

### 1. Data Understanding

Dataset yang digunakan dalam proyek prediksi customer churn ini berisi informasi perilaku dan karakteristik pelanggan Perusahaan XYZ. Tujuan utama tahap *data understanding* adalah memahami struktur data, kualitas data, serta peran potensial setiap fitur terhadap prediksi churn.

Dataset mencakup variabel-variabel inti berikut:

- **Tenure** — Lama waktu pelanggan telah menggunakan platform.
- **SatisfactionScore** — Tingkat kepuasan pelanggan terhadap layanan.
- **Complain** — Indikator apakah pelanggan pernah melakukan komplain.
- **DaySinceLastOrder** — Jumlah hari sejak transaksi terakhir.
- **OrderCount** — Total jumlah pesanan sepanjang periode observasi.
- **HourSpendOnApp** — Total jam penggunaan aplikasi.
- **PreferredLoginDevice** — Perangkat yang digunakan pelanggan.
- **CityTier** — Kategori wilayah domisili pelanggan.
- **WarehouseToHome** — Jarak atau estimasi waktu pengantaran.
- **PreferedOrderCat** — Kategori pesanan yang paling sering dipilih.
- **CouponUsed** — Jumlah kupon yang digunakan.
- **NumberOfDeviceRegistered** — Jumlah perangkat yang terdaftar.
- **Churn (Target Variable)** — 1 untuk churn, 0 untuk tidak churn.

Dataset bersifat multivariat dan mencakup data numerik, kategorikal, serta biner sehingga memerlukan penanganan preprocessing yang tepat agar model dapat mempelajari pola secara optimal.

---

### 2. Data Cleaning

Tahap *data cleaning* dilakukan untuk memastikan kualitas data optimal sebelum masuk ke tahap pemodelan. Proses pembersihan mencakup hal-hal berikut:

#### Missing Value Treatment
- Numerik → imputasi mean/median.
- Kategorikal → imputasi modus.

#### Data Type Correction
- Tenure & WarehouseToHome → dikonversi ke numerik.
- Perangkat & kategori → diubah menjadi kategori/string.

#### Outlier Handling
- Outlier ekstrem dianalisis menggunakan boxplot dan IQR.
- Outlier yang tidak relevan ditangani dengan winsorizing.

#### Categorical Normalization
- Menyeragamkan format teks (mis. “mobile” → “Mobile”).

#### Data Deduplication
- Menghapus record duplikat berdasarkan ID unik pelanggan.

#### Encoding
- One-Hot Encoding → fitur non-ordinal.
- Label Encoding → fitur ordinal atau untuk model tertentu.

---

## Exploratory Data Analysis (EDA)

Tahap *Exploratory Data Analysis (EDA)* dilakukan untuk memahami struktur data, pola perilaku pelanggan, serta potensi masalah yang dapat memengaruhi kualitas model prediksi churn. Proses EDA pada proyek ini mencakup beberapa langkah berikut:

---

### **1. Analisis Struktur Dataset**
- Dataset terdiri dari fitur numerik, kategorikal, dan biner yang merepresentasikan profil pelanggan, perilaku transaksi, serta interaksi mereka dengan perusahaan.
- Contoh fitur numerik: `Age`, `Tenure`, `SatisfactionScore`, `PointsInWallet`.
- Contoh fitur kategorikal: `Gender`, `MaritalStatus`, `MembershipCategory`.
- Contoh fitur biner: `Complain`, `Churn`.

---

### **2. Distribusi Variabel Target (Churn)**
- Analisis distribusi menunjukkan bahwa kelas `Churn` tidak seimbang (class imbalance).
- Proporsi pelanggan yang churn lebih rendah dibanding non-churn.
- Ketidakseimbangan ini menjadi dasar penggunaan teknik seperti **SMOTE** pada tahap modeling.

---

### **3. Statistik Deskriptif**
Pemeriksaan nilai minimum, maksimum, mean, dan standar deviasi dilakukan untuk:
- Mengidentifikasi nilai anomali,
- Mendeteksi outlier,
- Menilai skewness pada fitur numerik.

Hasil awal menunjukkan:
- Fitur seperti `PointsInWallet` dan `Tenure` memiliki distribusi skewed.
- Tidak ada nilai yang tampak tidak masuk akal.

---

### **4. Analisis Missing Value**
- Sebagian fitur memiliki missing value dalam jumlah kecil.
- Missing value dapat ditangani dengan imputasi menggunakan median (untuk numerik) atau modus (untuk kategorikal).
- Tidak ditemukan kolom dengan missing value ekstrem.

---

### **5. Korelasi Antar Fitur**
- Matriks korelasi digunakan untuk melihat hubungan antar fitur numerik.
- Ditemukan korelasi yang cukup kuat pada:
  - `DaySinceLastOrder` ↔ `Tenure`
  - `SatisfactionScore` ↔ `Complain`
- Tidak ada indikasi multikolinearitas berlebih yang perlu dihilangkan.

---

### **6. Analisis Distribusi Fitur Kategorikal**
- Distribusi kategori dianalisis menggunakan visualisasi seperti countplot.
- Beberapa kategori tidak seimbang, khususnya:
  - `MembershipCategory`
  - `Gender`
- Variabel `Complain` menunjukkan perbedaan distribusi yang cukup jelas antara pelanggan yang churn dan non-churn.

---

### **7. Hubungan Fitur dengan Churn**
Analisis univariate dan bivariate menunjukkan pola berikut:
- Pelanggan dengan **SatisfactionScore rendah** cenderung lebih sering churn.
- Pelanggan dengan **tenure lebih pendek** memiliki risiko churn lebih tinggi.
- Pelanggan yang **mengajukan komplain** dalam periode tertentu lebih rentan churn.
- `PointsInWallet` dan `NumberOfDeviceRegistered` menunjukkan pengaruh moderat terhadap churn.

---

### **8. Deteksi Outlier**
- Outlier diperiksa menggunakan boxplot dan metode **IQR**.
- Outlier muncul pada `PointsInWallet` dan `OrderAmountHikeFromlastYear`.
- Outlier tidak dihapus, tetapi distabilkan menggunakan metode

---

## Data Cleaning

Tahap *Data Cleaning* dilakukan untuk memastikan data yang digunakan pada proses pemodelan bebas dari inkonsistensi, missing value, dan anomali yang dapat mengganggu performa model prediksi churn. Proses pembersihan data dilakukan melalui langkah-langkah berikut:

---

### **1. Penanganan Missing Value**
Missing value ditangani berdasarkan tipe datanya:

- **Fitur numerik**  
  Missing value diisi menggunakan **median** untuk menjaga robustnes terhadap outlier.  
  Contoh kolom: `PointsInWallet`, `Tenure`, `SatisfactionScore`.

- **Fitur kategorikal**  
  Missing value diisi menggunakan **modus** (frekuensi terbanyak).  
  Contoh kolom: `Gender`, `MaritalStatus`, `MembershipCategory`.

Pendekatan ini digunakan agar dataset tetap konsisten tanpa menghilangkan terlalu banyak observasi.

---

### **2. Penanganan Outlier**
Outlier dideteksi menggunakan metode **Interquartile Range (IQR)** dan visualisasi seperti boxplot.

Kolom dengan outlier signifikan:
- `PointsInWallet`
- `OrderAmountHikeFromlastYear`
- `Tenure`

Strategi penanganan:
- Outlier **tidak dihapus**, melainkan ditangani dengan **winsorization**, yaitu membatasi nilai ekstrem pada percentile tertentu (misal 1%–99%).  
- Pendekatan ini menjaga distribusi tanpa distorsi besar dan mencegah model menjadi bias.

---

### **3. Normalisasi & Standarisasi Fitur**
Beberapa algoritma machine learning sensitif terhadap perbedaan skala fitur, terutama model seperti:
- Logistic Regression  
- SVM  
- KNN  

Maka dilakukan:
- **StandardScaler (Z-score scaling)** untuk fitur numerik kontinyu.  
- Penskalaan dilakukan hanya pada fitur input (X), bukan variabel target.

---

### **4. Konsistensi Format & Encoding**
Beberapa tipe data tidak konsisten dan perlu perbaikan:

- Kolom kategorikal dipastikan bertipe *object* atau *category*.
- Nilai string seperti `"Male"` dan `"male"` dinormalisasi agar konsisten.
- Fitur kategorikal kemudian di-encode menggunakan:
  - **One-Hot Encoding** untuk kolom nominal.
  - **Label Encoding** bila diperlukan (misal untuk variabel biner).

---

### **5. Penanganan Class Imbalance**
Distribusi target `Churn` tidak seimbang (minoritas = churn).  
Untuk meningkatkan kemampuan model mengenali kasus churn, dilakukan:

- **SMOTE (Synthetic Minority Over-sampling Technique)**  
  Digunakan pada tahap *modeling* untuk membuat jumlah data churn lebih seimbang dengan non-churn.

Teknik ini penting agar model tidak terlalu bias ke kelas mayoritas.

---

### **6. Feature Selection Awal**
Selama proses cleaning, beberapa fitur dengan karakteristik berikut dipertimbangkan untuk dieliminasi:

- Memiliki varians sangat rendah (*low variance features*).
- Redundansi tinggi dengan fitur lain.
- Tidak relevan berdasarkan domain knowledge.

Hasil akhir memastikan hanya fitur informatif yang dipertahankan dalam dataset final untuk modeling.

---
## Feature Engineering

Tahap *Feature Engineering* dilakukan untuk mengolah data mentah menjadi fitur yang lebih informatif tanpa menggunakan teknik Normalization atau Standard Scaling, karena model utama yang digunakan (Random Forest, XGBoost, Decision Tree) bersifat *tree-based* dan tidak memerlukan scaling.

---

### **1. Transformasi Fitur (Feature Transformation)**

Beberapa fitur numerik memiliki distribusi yang sangat skewed, sehingga dilakukan transformasi agar pola data lebih mudah ditangkap oleh model:

- **Log Transformation** pada:
  - `DaySinceLastOrder`
  - `OrderAmountHikeFromlastYear`
  - `PointsInWallet`

Tujuan:
- Mengurangi skewness  
- Memperbaiki distribusi fitur  
- Menstabilkan pengaruh outlier  

Karena model tree-based tidak sensitif terhadap skala, transformasi ini lebih ditujukan untuk meningkatkan stabilitas dan generalisasi.

---

### **2. Feature Encoding untuk Data Kategorikal**

Model hanya menerima data numerik, sehingga fitur kategorikal dikonversi menggunakan:

- **One-Hot Encoding**  
  Untuk fitur nominal:
  - `PreferredLoginDevice`
  - `PreferedOrderCat`
  - `PreferredPaymentMode`
  - `CityTier`
  - `Gender`

- **Label Encoding**  
  Untuk fitur biner:
  - `Complain`
  - `Churn` (target)

Encoding dilakukan pada dataset training agar menghindari data leakage.

---

### **3. Pembuatan Fitur Baru (Feature Creation)**

Beberapa fitur baru dibuat untuk memperkaya informasi dan membantu model menemukan pola tambahan:

- **ActivityScore**  
  Kombinasi dari:
  - `OrderCount`
  - `HourSpendOnApp`
  - `DaySinceLastOrder`

- **LoyaltyIndex**  
  Kombinasi dari:
  - `Tenure`
  - `CouponUsed`
  - `NumberOfDeviceRegistered`

- **RecentActivityFlag**  
  Menandai pelanggan yang terakhir order < 7 hari.

- **HighSpenderFlag**  
  Berdasarkan peningkatan belanja tahun sebelumnya.

Fitur-fitur ini memperkuat representasi perilaku pelanggan.

---

### **4. Feature Selection**

Seleksi fitur dilakukan menggunakan pendekatan berikut:

#### **a. Korelasi antar variabel numerik**
Digunakan untuk mendeteksi multikolinearitas. Fitur dengan korelasi > 0.90 dievaluasi untuk di-drop.

#### **b. Feature Importance (Model Tree-Based)**
Menggunakan:
- Random Forest
- XGBoost  

Untuk mengidentifikasi fitur paling berpengaruh terhadap churn.

Fitur yang paling penting:
- `SatisfactionScore`
- `DaySinceLastOrder`
- `Tenure`
- `Complain`

#### **c. RFE (Recursive Feature Elimination)**
Diuji untuk model Logistic Regression, namun tidak dipilih sebagai model utama.

---

Karena model utama bersifat *tree-based*, proses *Feature Engineering* difokuskan pada transformasi distribusi, encoding kategorikal, dan pembuatan fitur baru—tanpa melakukan normalisasi atau standardisasi.

## Modelling

Tahap pemodelan mengeksplorasi berbagai algoritma klasifikasi untuk mencari model terbaik dalam memprediksi churn, dengan mempertimbangkan karakteristik data dan kebutuhan bisnis (prioritaskan recall / F2-Score).  

---

### 1. Split Data: Train dan Test  
- Dataset dibagi: **80% train**, **20% test**  
- Pembagian dilakukan dengan `stratify=y` agar proporsi churn / non-churn tetap terjaga  

---

### 2. Daftar Model Kandidat  

Beberapa model diuji, antara lain:

- **Model linear / sederhana:**  
  - Logistic Regression  
- **Model berbasis jarak / instance-based:**  
  - K-Nearest Neighbors (KNN)  
- **Model tree / ensemble:**  
  - Decision Tree Classifier  
  - Random Forest Classifier  
  - AdaBoost Classifier  
  - Gradient Boosting Classifier  
  - XGBoost Classifier  
  - LightGBM Classifier  

Penggunaan keragaman model ini memungkinkan perbandingan performa dari sudut pandang bias-variance dan kompleksitas data. :contentReference[oaicite:3]{index=3}

---

### 3. Pipeline & Penanganan Imbalance  
- Preprocessing + encoding dilakukan sesuai kebutuhan fitur (numerik & kategorikal)  
- Untuk menangani imbalance kelas (churn minoritas), digunakan teknik oversampling seperti SMOTE sebelum pelatihan model  

---

### 4. Hyperparameter Tuning & Cross-Validation  
- Untuk model berbasis ensemble (misalnya Random Forest, XGBoost, LightGBM), digunakan **GridSearchCV** atau **RandomizedSearchCV**  
- Skema cross-validation (misalnya 5-fold) untuk memastikan generalisasi model  

Hyperparameter yang disetel misalnya:
- `n_estimators`, `max_depth`, `learning_rate`, `subsample`, `colsample_bytree` (untuk boosting)  
- `n_neighbors` (untuk KNN)  
- parameter tree (untuk Decision Tree / Random Forest)  

---

### 5. Evaluasi & Pemilihan Model dengan Metrik Bisnis-Driven  
Karena kehilangan pelanggan (False Negative) sangat merugikan, maka metrik utamanya adalah **F2-Score** (Recall lebih ditekankan). Model dibandingkan berdasarkan:

- F2-Score (utama)  
- Recall / Precision / Confusion Matrix  
- ROC-AUC sebagai referensi  

Model dengan F2 tertinggi dianggap paling sesuai bisnis.

---

### 6. Threshold Tuning  
Setelah model terbaik dipilih berdasarkan F2 di CV, dilakukan tuning threshold probabilitas untuk memaksimalkan F2 pada data test. Threshold rendah bisa meningkatkan recall → sesuai prioritas bisnis.

---

### 7. Final Model & Output Risk Score  
Model terpilih dipakai untuk menghasilkan **Churn Risk Score** (probabilitas) untuk setiap pelanggan. Output ini bisa digunakan tim bisnis untuk prioritisasi intervensi.

---

### 8. Dokumentasi & Interpretabilitas  
- Feature importance (untuk tree-based) diambil untuk memahami driver churn.  
- Jika perlu, teknik interpretabilitas seperti SHAP bisa dipertimbangkan agar keputusan bisa dijelaskan ke stakeholders.  


### 9. Deploy Model to Web Base

Deployment Machine Learning adalah proses membuat model machine learning yang telah dilatih (trained) dapat digunakan dalam aplikasi yang dapat diakses oleh pengguna akhir secara online.

Deployment machine learning memiliki peran penting dalam pemanfaatan model machine learning secara optimal dalam berbagai aplikasi yang dapat mempermudah dan mempercepat pengambilan keputusan.

1. Exploratory Data Analyst

<img width="2940" height="1912" alt="image" src="https://github.com/user-attachments/assets/33950f9a-ede4-45f3-ade2-1da40c284c7c" />
<img width="2940" height="1912" alt="image" src="https://github.com/user-attachments/assets/3c07a373-971d-4565-942b-840a7b21a72f" />

2. Prediksi Churn
   
<img width="2940" height="1912" alt="image" src="https://github.com/user-attachments/assets/9994785d-afa8-445b-8f91-032f73da29e9" />
<img width="2940" height="1912" alt="image" src="https://github.com/user-attachments/assets/e24f9c6d-3a9a-4044-9cc0-fb290c531672" />


Link Colab: https://colab.research.google.com/drive/1YAvCTn4oH1kR3xowhnPpUE3af3ERNo-W <br>
Link Tableu: https://www.kaggle.com/ankitverma2010/ecommerce-customer-churn-analysis-and-prediction <br>
Link download material: https://www.kaggle.com/ankitverma2010/ecommerce-customer-churn-analysis-and-prediction <br>
Link demo / deployment: https://alphafinprojectchurnanalysisprediction.streamlit.app <br>
