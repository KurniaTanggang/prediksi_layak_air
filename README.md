# Prediksi Kelayakan Air

 
## Domain Proyek - Kesehatan
Air adalah salah satu sumber penting kehidupan makhluk hidup termasuk manusia. Air minum bermanfaat untuk mencegah dehidrasi, membantu sistem pencernaan, melindungi jaringan tubuh, hingga menjaga kesehatan tulang dan sendi. Meski demikian, perlu memperhatikan kualitas air yang bersih guna menghindari berbagai penyakit akibat mengonsumsi air yang tidak layak minum, mulai dari diare, kolera, tifus, hingga kanker ([aladokter.com](https://www.alodokter.com/belum-cukup-hanya-dengan-merebus-air)). Menteri Kesehatan telah menetapkan [standar baku](http://hukor.kemkes.go.id/uploads/produk_hukum/PMK_No._32_ttg_Standar_Baku_Mutu_Kesehatan_Air_Keperluan_Sanitasi,_Kolam_Renang,_Solus_Per_Aqua_.pdf) air bersih dengan parameter seperti pH, kekeruhan, zat padat, sulfat dan lain-lain. Berdasarkan parameter tersebut dan beberapa parameter tambahan akan dibuat prediksi apakah air itu adalah air layak minum atau tidak.
 
## Business Understanding
Memprediksi air layak minum dan tidak layak minum dengan menggunakan beberapa parameter. Dari parameter tersebut, model machine learning akan mengklasifikasikan mana yang layak dan mana yang tidak layak sehingga dapat digunakan untuk memprediksi kelayakan air.
 
### Problem Statements
- Apa saja parameter yang paling berpengaruh terhadap kelayakan air minum ?
- Bagaiaman membuat model yang dapat memprediksi kelayakan air ?
 
### Goals
- Mengetahui parameter yang paling berpengaruh terhadap kelayakan air.
- Membuat model machine learning yang dapat memprediksi kelayakan air minum.
 
### Solution statements
Untuk dapat mengetahui kelayakan air, maka dibuatlah prediksi air layak minum dan tidak layak minum berdasarkan parameter pH, Hardness, Solids, Chloramines, Sulfate, Conductivity, Organic_carbon, Trihalomethanes, Turbidity, dan Potability dengan **Algoritma  Random Forest**.
 
**Random Forest** termasuk ke dalam ensemble learning yang terdiri dari beberapa model yang bekerja bersamaan. Random forest tersusun dari banyak algoritma decision tree yang pembagian data dan fiturnya dipilih secara acak (random). 
 
Karena ini adalah kasus klasifikasi, maka akan digunakan Random Forest Classifier. Proses klasifikasi pada random forest berawal dari memecah data sampel yang ada kedalam decision tree secara acak. Setelah pohon terbentuk,maka akan dilakukan voting pada setiap kelas dari data sampel. Kemudian, mengkombinasikan vote dari setiap kelas kemudian diambil vote yang paling banyak ([dqlab.id](https://dqlab.id/4-rekomendasi-algoritma-machine-learning-untuk-klasifikasi))
 
 
## Data Understanding
Data yang akan digunakan adalah Dataset Water Potability yang diambil dari [Kaggle](https://www.kaggle.com/adityakadiwal/water-potability). 
 
Variabel-variabelnya adalah sebagai berikut:
1. ph : pH air (0 - 14)
2. Hardness : Kapasitar air untuk mengendapkan sabun (mg/L)
3. Solids : Total padatan terlarut (ppm)
4. Chloramines : Jumlah Chloramin (ppm)
5. Sulfate : Jumlah sulfat (mg/L)
6. Conductivity : Konduktivitas listrik pada air (μS/cm)
7. Organic_carbon : Jumlah organic carbon (ppm)
8. Trihalomethanes : Jumalah Trihalomethanes in (μg/L)
9. Turbidity : Kekeruhan
10. Potability : Kelayakan air. Layak = 1 dan tidak layak = 0
 
Dengan Exploratory Data Analysis diketahui bahwa dataset mempunyai *missing value*, sampel **potability** yang tidak seimbang, adanya *outliers* dan ternyata tidak adalah korelasi yang kuat antar fitur. Setelah diketahui dan untuk menghindari bias maka *missing value*-nya ditangani, sampel **potability** diseimbangkan, dan *outliers* ditangani dengan **IQR**.
## Data Preparation
- Membersihkan data, yaitu menangani *missing value* dengan mengganti *missing value* dengan nilai rata-rata, menyeimbangkan fitur potability, dan mengatasi *outliers* dengan IQR. Hal ini dilakukan untuk menghindari bias yang dapat mempengaruhi hasil prediksi oleh model.
- Melakukan Train-Test Split untuk membagi data latih dan data uji dengan komposisi 80:20. Data latih (train) akan berjumlah 80% dari data dan data uji (test) berjumlah 20% dari data. Hal ini dilakukan agar dapat melakukan pelatihan model pada test data dan mengujinya menggunakan test data. 
 
## Modeling
Algoritma Random Forest yang digunakan adalah ***RandomForestClassifier*** karena konteks dari data adalah klasifikasi. Menggunakan metric Classification Report untuk mengukur kualitas prediksi model. Hasil Classification Report memperlihatkan bahwa model memiliki kualitas yang cukup baik dengan precission, recall, f1-score, accuracy yang nilainya melebihi 0.8.
 
## Evaluation
Evaluasi menggunakan metrik Classification Report.  Classification Report dapat menunjukkan nilai dari *precission, recall, f1-score* dan *accuray* yang mana jika ketiga nilai tersebut semakin mendekati 1 maka semakin baik juga model yang dibangun.
Formula pada *precission, recall,* dan *f1_Score*  :
 
```sh
precission = tp / (tp + fp)
recall = tp / (tp + fn)
f1_score = 2 * (precision * recall) / (precision + recall)
```
tp = true positive
tp = false postive
fn = false negative
 
Dengan nilai terbaik adalah 1 dan nilai terburuk adalah 0.
 
Penerapan Classification Report pada code adalah :
```sh
# Split data
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=1)
```
```sh
# Modeling
rf = RandomForestClassifier()
rf.fit(X_train, y_train)
y_pred = rf.predict(X_test)
```
```sh
# Metric
from sklearn.metrics import classification_report
classification_report(y_test, y_pred_test)
```
 
Dari hasil Classification Report pada model Random Forest, terlihat model dibangun dengan cukup baik yang dibuktikan dengan *precission, recall, f1-score* dan *accuray* yang bernilai 0.8 atau mendekati 0.8. Seperti yang sudah dijelaskan sebelumnya, jika nilai semakin mendekati 1, maka model yang dibangung semakin baik. Saat dilakukan prediksi dengan 50 y_test, didapatkan bahwa Random Forest dapat memprediksi 44 sampel dengan benar. Hal ini berarti bahwa model sudah cukup baik untuk memprediksi kelayakan air.
 
Dari model yang dirancang dan evaluasi yang dilakukan, disimpulkan bahwa tidak ada paramater yang paling berpengaruh terhadap kelayakan air dan  model Random Forest cukup baik dan dapat digunakan untuk memprediksi kelayakan air.
 
