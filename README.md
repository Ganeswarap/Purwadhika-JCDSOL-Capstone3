# Purwadhika-JCDSOL-Capstone3
## Business Problem
Sebuah perusahaan di bidang telekomunikasi ingin menganalisis tingkat *customer churn*-nya, yaitu seberapa banyak pelanggan yang berhenti berlangganan atau membeli produk dari perusahaan kita. Perusahaan memiliki data *customer churn* dan ingin menggunakannya untuk memprediksi pelanggan mana yang berisiko berhenti berlangganan, sehingga perusahaan dapat mengambil langkah pencegahan untuk menjaga *customer churn* tetap rendah ke depannya.

Cara yang paling efektif untuk menjaga *customer churn* yang rendah adalah dengan mencegah pelanggan untuk berhenti berlangganan. Analisis *customer churn* dapat memperlihatkan faktor-faktor yang mungkin mempengaruhi keputusan konsumen untuk berhenti berlangganan, yang bisa membantu perusahaan untuk menentukan langkah preventif yang tepat. Membuat model untuk prediksi *churn* juga dapat membantu perusahaan untuk menentukan segmen konsumen yang berisiko untuk berhenti berlangganan, sehingga keputusan yang diambil bisa diterapkan dengan efektif.

Perusahaan ingin memiliki kemampuan untuk memprediksi pelanggan yang berisiko untuk berhenti berlangganan, sehingga dapat memfokuskan layanan dan pemasaran kepada segmen konsumen tersebut. Perusahaan juga ingin mengetahui kriteria apa saja yang mungkin menyebabkan seorang pelanggan berhenti berlangganan, sehingga bisa membuat strategi yang tepat untuk menjaga pelanggan agar tetap berlangganan.

Akan dilakukan analisis data untuk menentukan faktor-faktor yang menyebabkan seorang pelanggan berisiko untuk berhenti berlangganan. Kemudian menggunakan pembelajaran mesin, akan dibangun model klasifikasi yang akan digunakan untuk memprediksi apakah seorang pelanggan berisiko berhenti berlangganan atau tidak. Dengan fokus mendeteksi pelanggan berisiko sebaik mungkin, metrik yang akan kita gunakan adalah recall, PR-AUC dan ROC-AUC.

Dataset yang digunakan berisi 4930 baris yang berupa informasi pelanggan perusahaan yang mungkin berhubungan dengan keputusan pelanggan untuk berhenti berlangganan atau tidak (perhatikan bahwa tidak ada fitur yang jelas tidak memiliki pengaruh seperti ID atau nama pelanggan). Masing-masing baris merepresentasikan informasi dari seorang pelanggan. 19 kolom dataset ini terdiri dari:

1.  `Dependents`: Apakah pelanggan memiliki tanggungan.
2.  `tenure`: Lamanya pelanggan telah berlangganan dalam bulan.
3.  `OnlineSecurity`: Apakah pelanggan menerima perlindungan *online*.
4.  `OnlineBackup`: Apakah pelanggan menerima layanan *backup online*.
5.  `InternetService`: Apakah pelanggan berlangganan layanan internet.
6.  `DeviceProtection`: Apakah pelanggan menerima proteksi perangkat.
7.  `TechSupport`: Apakah pelanggan menerima *tech support*.
8.  `Contract`: Tipe kontrak berdasarkan durasi.
9.  `PaperlessBilling`: Apakah tagihan dikirimkan tanpa kertas.
10. `MonthlyCharges`: Tagihan bulanan (diasumsikan dalam dollar).
11. `Churn`: Apakah pelanggan berhenti berlangganan.

Kolom `Churn` akan menjadi kolom target  dengan ketentuan sebagai berikut:

yes (positif) : Pelanggan berhenti berlangganan.

no (negatif)&nbsp;: Pelanggan masih berlangganan.
## Data Analysis
Pada bagian Exploratory Data Analysis akan dilihat hubungan masing-masing fitur dengan fitur target, yaitu kolom `Churn`, khususnya persentase pelanggan yang berhenti berlangganan berdasarkan nilai unik masing-masing kolom, serta visualisasinya. Pada bagian ini, didapat bahwa kriteria pelanggan yang berhenti berlangganan diantaranya pelanggan dengan tanggungan, pelanggan yang berlangganan layanan internet tapi tidak berlangganan produk-produk lain yang menjadi fitur di dataset, pelanggan yang berlangganan internet dengan serat optik, pelanggan dengan tipe kontrak *month-to-month8, dan pelanggan yang menerima menerima tagihan tidak dalam bentuk kertas. Ditemukan juga bahwa dataset yang digunakan bersifat tiak seimbang, dengan label positif hanya mencapai 26.7%.
## Machine Learning Modeling
Proses pembuatan pembelajaran mesin adalah sebagai berikut:

1.   Melakukan data preprocessing,
2.   Memilih model yang akan digunakan,
3.   Melihat pengaruh penggunaan oversampling,
4.   Melakukan hyperparameter tuning.

*Data preprocess* yang dilakukan adalah minmax scaler untuk fitur numerik (`tenure` dan `MonthlyCharges`) dan one hot encoding untuk fitur kategorik.

Pilihan model yang akan digunakan sebagai *benchmark* adalah regresi logistik, k-nearest neighbors, decision tree, random forest, gradient booosting, adaptive boosting, dan extreme gradient boosting, masing-masing dengan parameter *default*-nya. Model yang dipilih adalah adaptive boosting.

Teknik oversampling yang digunakan adalah random oversampling, dan terlihat bahwa model dengan oversampling memiliki performa yang lebih baik.

Hyperparameter tuning dilakukan untuk menentukan parameter `n_estimator`, `learning_rate`, dan `algorithm`. Parameter terbaik untuk meningkatkan recall yang kita dapatkan adalah `n_estimator` 75, `learning_rate` 0.001, dan `algorithm` SAMME, dengan recall 98.86%, tetapi PR-AUC dan ROC-AUC-nya turun menjadi 34.35% dan 65.19% berturut-turut. Parameter terbaik untuk meningkatkan PR-AUC adalah `n_estimator` 75, `learning_rate` 0.05, dan `algorithm` SAMME.R, namun perubahan metrik-metrik evaluasinya tidak signifikan. Karena tujuan utama model adalah memprediksi pelanggan yang berisiko berhenti sebaik mungkin, maka dipilih model pertama yang memberikan recall tertinggi.

Ketika dilihan *feature importance*-nya, ditemukan bahwa model hanya menggunakan fitur `Contract`. Hal ini dikarenakan *churn rate* pelanggan dengan kontrak 2 tahun hanya bernilai 2.43%.
## Kesimpulan
Model yang dibuat bisa memprediksi sekitar 99% pelanggan yang berisiko berlangganan. Namun, model memprediksi 68% pelanggan yang tidak berisiko sebagai pelanggan berisiko. Untuk tujuan utama dibangunnya model ini, mendeteksi pelanggan yang berisiko berhenti, model ini memiliki performa yang sangat baik. Sayangnya, model memiliki 2 kekurangan yang mungkin fatal: tingginya tingkat *false positive* dan hanya mempertimbangkan fitur `Contract`. Model yang dibuat memiliki performa yang sangat baik, bahkan mendekati ajaib, tapi hanya jika kita ingin mendeteksi pelanggan yang berisiko berhenti tanpa memedulikan kasus *false positive*, dan mungkin hanya untuk dataset dimana persentase pelanggan dengan kontrak 2 tahun yang berisiko berhenti sangat kecil.

## Saran
Rekomendasi yang diberikan adalah sebagai berikut:

*   Gunakan model ini hanya jika perusahaan merasa bahwa salah mendeteksi pelanggan yang masih ingin berlanggan sebagai pelanggan berisiko bukan masalah.
*   Menambah fitur yang mungkin memengaruhi kepuasan pelanggan seperti produk lain perusahaan jika ada.
*   Menambah data, khususnya pelanggan dengan kontrak 2 tahun yang memutuskan untuk berhenti berlangganan jika ada.
*   Mencoba model-model lain yang tidak kita coba, seperti SVM, neural network, naive bayes, dan lainnya, serta mencoba melakukan hyperparameter tuning pada model yang tidak terpilih seperti extreme gradient boost.
*   Meliti lebih lanjut penggunaan teknik oversampling lain seperti SMOTENC.
