# Analisis Perbandingan OLS, GWR, dan MGWR terhadap Variasi Spasial Kualitas Lingkungan Hidup di Pulau Jawa

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![Status](https://img.shields.io/badge/Status-Completed-green)
![Spatial Analysis](https://img.shields.io/badge/Focus-Spatial%20Data%20Science-orange)

Repositori ini berisi kode sumber (*source code*) dan data untuk penelitian berjudul **"Analisis Perbandingan OLS, GWR, dan MGWR terhadap Variasi Spasial Kualitas Lingkungan Hidup di Pulau Jawa"**.

Penelitian ini bertujuan untuk membandingkan performa model regresi global (*Ordinary Least Squares*) dengan model regresi spasial lokal (*Geographically Weighted Regression* dan *Multiscale GWR*) dalam memodelkan determinan Indeks Kualitas Lingkungan Hidup (IKLH) di 119 Kabupaten/Kota di Pulau Jawa.

## 📌 Latar Belakang
Pulau Jawa menghadapi tekanan lingkungan yang kompleks akibat urbanisasi dan industrialisasi. Pendekatan statistik konvensional (OLS) seringkali mengabaikan efek spasial (*spatial dependence*) dan heterogenitas lokal. Penelitian ini menerapkan pendekatan **MGWR** untuk menangkap variasi pengaruh variabel lingkungan yang berbeda-beda di setiap wilayah (skala lokal vs global).

## 📂 Dataset
Penelitian ini menggunakan data sekunder tahun 2024 dengan unit analisis 119 Kabupaten/Kota di Pulau Jawa.

| Tipe Variabel | Nama Variabel | Keterangan |
| :--- | :--- | :--- |
| **Dependen (Y)** | `IKLH` | Indeks Kualitas Lingkungan Hidup |
| **Independen (X1)** | `JumlahLuas_Hutan` | Luas tutupan lahan hutan (Ha) |
| **Independen (X2)** | `Akses_Sanitasi` | Persentase rumah tangga dengan sanitasi layak |
| **Independen (X3)** | `Jumlah_Kendaraan` | Jumlah kendaraan bermotor |
| **Independen (X4)** | `Timbulan_Sampah` | Volume timbulan sampah harian |

> **Catatan:** Data spasial (peta) menggunakan format GeoJSON `java_kabkota_fixed.geojson`.

## 🛠️ Metodologi
Analisis dilakukan menggunakan Python dengan tahapan sebagai berikut:
1.  **Pra-pemrosesan Data:** Pembersihan data, transformasi Logaritma natural (Ln) untuk menormalkan distribusi, dan penyelarasan nama wilayah antara CSV dan GeoJSON.
2.  **Regresi Global (OLS):** Estimasi awal dan uji asumsi klasik (VIF untuk multikolinearitas).
3.  **Uji Autokorelasi Spasial:** Menggunakan Indeks Moran (Moran's I) pada residual OLS untuk mendeteksi ketergantungan spasial.
4.  **Pemodelan Spasial:**
    * **GWR (Geographically Weighted Regression):** Menggunakan *Adaptive Bisquare Kernel*.
    * **MGWR (Multiscale GWR):** Mengizinkan *bandwidth* (skala pengaruh) yang berbeda untuk setiap variabel.
5.  **Evaluasi Model:** Menggunakan metrik AICc dan Adjusted R-squared.

## 📊 Hasil Utama
Berdasarkan analisis, model **MGWR** terbukti memiliki performa terbaik dibandingkan OLS dan GWR.

| Model | AICc | Adjusted R² | Keterangan |
| :--- | :--- | :--- | :--- |
| **OLS** | 270.01 | 0.4636 | Model Global (Baseline) |
| **GWR** | 262.68 | 0.5605 | Model Lokal (Single Bandwidth) |
| **MGWR** | **254.23** | **0.5859** | **Model Terbaik (Best Fit)** |

**Temuan Variasi Spasial:**
* **Jumlah Luas Hutan (Global/Stasioner):** Berpengaruh positif signifikan di seluruh wilayah Jawa secara konsisten.
* **Akses Sanitasi (Lokal):** Menunjukkan polarisasi; hanya signifikan di wilayah Barat (Banten, Jabar, Jakarta) dengan anomali pengaruh negatif di wilayah aglomerasi Jabodetabek ("Ilusi Akses").
* **Timbulan Sampah (Lokal):** Klaster kerentanan tertinggi ditemukan di wilayah Jawa Barat dan Tengah (misal: Sumedang, Cilegon).
* **Jumlah Kendaraan Bermotor (Global/Stasioner):** Dampak emisi bersifat *transboundary* (lintas batas) dan tidak menunjukkan klaster lokal yang signifikan.
