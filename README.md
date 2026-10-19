# 🎓 Open University Learning Analytics & Student Performance Dashboard

![Excel](https://img.shields.io/badge/Microsoft_Excel-217346?style=for-the-badge&logo=microsoft-excel&logoColor=white)
![Power Query](https://img.shields.io/badge/Power_Query-F2C94C?style=for-the-badge&logo=microsoft-excel&logoColor=black)
![Power Pivot](https://img.shields.io/badge/Power_Pivot-0078D4?style=for-the-badge&logo=microsoft-excel&logoColor=white)

## 📌 Business Overview
Proyek ini bertujuan untuk menganalisis performa akademik, kepatuhan pengumpulan tugas (*assessment compliance*), serta faktor sosio-demografi mahasiswa pada *Open University Learning Analytics Dataset (OULAD)*. Melalui proyek ini, dikembangkan sebuah **Interactive Student Performance & Compliance Dashboard** untuk membantu institusi mengidentifikasi risiko kegagalan akademik dan dampak keterlambatan tugas secara dini.

> **Dataset Source:** [Kaggle - Open University Learning Analytics Dataset](https://www.kaggle.com/datasets/rocki37/open-university-learning-analytics-dataset)

---

## 🛠️ Key Technical Implementations & Workflow

### 1. Data Cleaning & Transformation (Power Query)
- **Text Manipulation:** Mengubah standar kapitalisasi (`UPPERCASE`) pada kolom `highest_education` & `region`, merapikan `imd_band`, serta membuat kunci unik `course_id` (`code_module` + `code_presentation`).
- **Date & Numeric Extraction:** Ekstraksi bulan pendaftaran, tanggal pengumpulan tugas, dan kalkulasi kolom kustom `selisih_hari` untuk mengukur tingkat keterlambatan penyerahan tugas.

### 2. Analytical Functions & Sensitivity Analysis
- **Logical & Lookup Functions:** Menggunakan `VLOOKUP` untuk menggabungkan durasi kursus serta fungsi logika `IF` untuk mengategorikan tingkat beban studi (*credits* > 200).
- **What-If Sensitivity Analysis:** Menyimulasikan dampak skenario kebijakan penalti keterlambatan tugas (pemotongan 0 hingga 5 poin/hari) terhadap penurunan rata-rata nilai kelas dengan rumusan `=MAX(0, IF([selisih_hari]>0, score - (selisih_hari * penalti), score))`.

### 3. Data Modeling & DAX Measures (Power Pivot)
- Menyusun **Relational Data Model** yang menghubungkan tabel master (`master_student`, `courses`, `vle`, `assessments`) dengan tabel fakta transaksi (`studentAssessment`, `studentRegistration`, `studentVle`).
- Merancang **DAX Measures & Cube Functions** untuk metrik kunci:
  - `Rata_Rata_Skor` = `=AVERAGE(studentAssessment[score])`
  - `Total_Tepat_Waktu` = `=CALCULATE(COUNTROWS(studentAssessment), studentAssessment[status_assessment]="Tepat Waktu")`
  - `Tingkat_Kepatuhan` = `=DIVIDE([Total_Tepat_Waktu], COUNTROWS(studentAssessment), 0)` *(Mencapai 71%)*
  - `Persentase_Kelulusan` & `Skor_Tugas_Terlambat`

---

## 📊 Interactive Dashboard & Key Insights

![Dashboard Preview](docs/dashboard_preview.png) *(Catatan: Unggah foto/screenshot dashboard-mu ke folder docs dan sesuaikan link-nya)*

### 🔍 Key Business Insights:
1. **Student Compliance:** 71% pengumpulan tugas dilakukan tepat waktu, sedangkan 29% mengalami keterlambatan.
2. **Correlation per Module:** Modul **BBB** mencatatkan volume keterlambatan tertinggi. Sebaliknya, modul **EEE** dengan tingkat keterlambatan paling minim berhasil mencatatkan rata-rata nilai akhir tertinggi.
3. **Socioeconomic Impact:** Mahasiswa pada tingkat kesejahteraan paling rendah (`imd_band` 0-10%) memiliki rasio kegagalan akademik tertinggi per kredit yang diambil, menunjukkan perlunya intervensi bantuan akademik khusus.
4. **Educational Background:** Mahasiswa dengan latar belakang *Post Graduate Qualification* memiliki rata-rata skor paling tinggi dibanding kualifikasi lainnya.

---

## 📁 Repository Structure
```text
├── dataset/            # File CSV / data mentah dari OULAD
├── docs/               # Screenshot dashboard & dokumentasi visual
├── reports/            # File Laporan Akhir (.pdf / .docx)
└── Student_Analytics.xlsx  # File Excel Utama (Power Query, Data Model, Dashboard)
