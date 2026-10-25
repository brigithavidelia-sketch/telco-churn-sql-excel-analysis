### Analisis Prediksi Churn Pelanggan dan Retensi Pendapatan (Telco)

### 1. Latar Belakang & Masalah Bisnis

Mempertahankan pelanggan aktif jauh lebih hemat biaya daripada mengakuisisi pelanggan baru. Perusahaan telekomunikasi (Telco) ini menghadapi tantangan fluktuasi tingkat kehilangan pelanggan (churn rate). Proyek ini bertujuan untuk menganalisis karakteristik kontrak pelanggan yang paling rentan kabur serta menghitung dampak kerugian finansial secara bulanan (Revenue Loss) guna merumuskan strategi retensi yang tepat. 

### 2. Dataset yang Digunakan

Dataset bersumber dari IBM Telco Customer Churn di Kaggle, mencakup 7.043 data pelanggan dengan atribut seperti profil demografi, jenis kontrak (contract), biaya bulanan (monthly_charges), biaya total (total_charges), dan status kelayakan (churn). 

### 3. Proses Pembersihan Data & Kueri SQL Tingkat Lanjut

Proses *data cleaning* dan penyiapan tabel dilakukan di **PostgreSQL** dengan menangani spasi kosong pada kolom total_charges dan mengubah tipe data teks status *churn* menjadi format biner numerik (1 = Churn, 0 = Aktif) menggunakan fungsi CASE WHEN. 

sql

CREATE OR REPLACE VIEW cleaned_telco_churn AS
SELECT 
    customer_id, contract, monthly_charges,
    CASE WHEN TRIM(total_charges) = '' THEN 0 ELSE total_charges::NUMERIC(10,2) END AS total_charges,
    CASE WHEN churn = 'Yes' THEN 1 ELSE 0 END AS churn_status
FROM telco_churn;

Gunakan kode dengan hati-hati.

### Analisis Kunci Bisnis menggunakan SQL:

sql

-- Menghitung Rasio Churn Berdasarkan Kontrak
SELECT contract, COUNT(customer_id) AS total_pelanggan, SUM(churn_status) AS jumlah_churn,
ROUND((SUM(churn_status)::NUMERIC / COUNT(customer_id)::NUMERIC) * 100, 2) AS churn_rate_persen
FROM cleaned_telco_churn GROUP BY contract;

Gunakan kode dengan hati-hati.

### 4. Visualisasi & Temuan Utama (Insights)

### A. Analisis Pelaporan Ringkas (Microsoft Excel)

Melalui pengolahan data menggunakan *Pivot Table* dan *Pivot Chart* di Excel, ditemukan bahwa pelanggan dengan jenis kontrak **Month-to-month (Bulanan)** memiliki kecenderungan kabur paling tinggi dibandingkan dengan pelanggan kontrak jangka panjang (1-2 tahun). 

### B. Executive Dashboard Interaktif (Tableau Public)

Laporan data bersih dieksport ke Tableau Public untuk menyusun visualisasi interaktif yang mendalam guna memetakan korelasi antara jenis kontrak dengan total potensi kerugian keuangan perusahaan. 

### 5. Kesimpulan & Rekomendasi Strategis

1. **Insentif Migrasi Kontrak:** Karena pelanggan *Month-to-month* menyumbang angka *churn* dan kerugian pendapatan terbesar, tim pemasaran direkomendasikan memberikan promo potongan harga khusus jika pelanggan bersedia bermigrasi ke kontrak minimal 1 atau 2 tahun.
2. **Sistem Peringatan Dini (Early Warning System):** Memanfaatkan visualisasi data tren ini untuk memonitor pelanggan bulanan yang memasuki masa krusial (tenure pendek) guna diberikan penawaran retensi sebelum mereka memutuskan untuk *churn*.

### 📊 Tautan Proyek Lengkap

* 📊 [Lihat Dashboard Interaktif di Tableau Public](https://public.tableau.com/views/TelcoCustomerChurnAnalysis_17929109415620/Dashboard1?:language=en-US&publish=yes&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)
* 📂 [Unduh Laporan Pivot Tabel Excel (.xlsx)](telco_churn_excel_analysis.xlsx)
