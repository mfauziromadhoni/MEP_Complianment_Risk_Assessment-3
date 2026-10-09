# Internal Audit Analytics — MEP Deep-Dive Investigation & Financial Impact
### Bagian 3 dari 3: Dashboard Interaktif (Power BI)

Proyek ini adalah bagian ketiga dari rangkaian analisis audit internal terhadap peralatan **MEP (Mechanical, Electrical, Plumbing)** — Lift, Escalator, Sprinkler, Genset, dan HVAC. Setelah Bagian 1 (Python) mengidentifikasi site dan peralatan berisiko, dan Bagian 2 (Excel) mengukur efektivitas tindak lanjut, bagian ini menjawab dua fungsi inti Internal Audit lainnya menggunakan **Power BI**: **investigasi mendalam** terhadap kasus yang memerlukan audit lanjutan, dan **penilaian dampak finansial** sebagai dasar prioritas mitigasi.

## Latar Belakang Bisnis

Manajemen tidak cukup mengetahui *berapa banyak* temuan yang ada. Mereka perlu tahu **kasus mana yang masih menggantung**, **di mana uang paling berisiko**, dan **seberapa besar eksposur yang belum tertangani**. Dashboard ini menerjemahkan data audit menjadi angka yang dapat langsung dipakai untuk menentukan prioritas audit dan alokasi sumber daya.

## Dataset

| Detail | Keterangan |
|---|---|
| Sumber | `MEP_Internal_Audit_Dataset` (data sintetis untuk keperluan portofolio) |
| Jumlah observasi | 1.251 catatan audit, 217 unit peralatan, 10 site |
| Periode | Q1 2023 – Q4 2024 |
| Kolom kunci | `Requires_Investigation`, `Investigation_Status`, `Is_Repeat_Finding`, `Estimated_Financial_Exposure_IDR`, `Follow_Up_Status`, `Risk_Level` |

## Metodologi

1. **Power Query** — memastikan tipe data benar (tanggal, angka, teks) dan membuat kolom bantu `Risk_Sort_Order` agar urutan risiko tampil logis (Low → Medium → High → Critical), bukan alfabetis.
2. **DAX Measures** — dikelompokkan dalam tabel `_Measures`: `Total Audit`, `Total Financial Exposure`, `Unresolved Exposure`, `% Unresolved Exposure`, `Total Investigation Cases`, `Open Investigation Cases`, `% Open Investigation`.
3. **Dashboard 2 halaman** (canvas 1280 × 720 px, rasio 16:9) dengan palet warna korporat: biru navy `#1F4E78` sebagai warna utama, serta hijau–kuning–oranye–merah (`#70AD47`, `#FFC000`, `#ED7D31`, `#C00000`) khusus untuk tingkat risiko.
4. **Slicer interaktif** per `Site_Name` dan `Risk_Level` agar dashboard dapat dieksplorasi saat presentasi.

---

## Halaman 1 — Deep-Dive Investigation

| Visual | Fungsi | Insight Bisnis |
|---|---|---|
| **KPI Card:** Total Investigation Cases | Ukuran beban kerja investigasi | Terdapat **205 kasus** yang diklasifikasikan memerlukan audit investigasi, sekitar 16% dari seluruh catatan audit. |
| **KPI Card:** Open Investigation Cases | Backlog yang belum tuntas | **76 kasus masih Open**, sedangkan 129 sudah Closed. |
| **KPI Card:** % Open Investigation | Rasio backlog | **37,1%** kasus investigasi belum selesai — hampir 2 dari 5 kasus. |
| **Bar Chart:** Investigasi per Equipment Category | Menunjukkan peralatan yang paling sering memicu investigasi | **HVAC (68)** dan **Sprinkler (52)** mendominasi, disusul Lift (49), Genset (20), dan Escalator (16). Dua kategori teratas menyumbang lebih dari separuh seluruh kasus investigasi. |
| **Tren per Kuartal** | Menunjukkan apakah beban investigasi stabil atau melonjak | Jumlah kasus berkisar 19–34 per kuartal. Puncaknya terjadi di **Q1–Q2 2024 (34 dan 33 kasus)**, lalu turun ke 19 dan 22 kasus di semester kedua 2024. Perlu ditelusuri apakah penurunan ini hasil perbaikan proses atau sekadar cakupan audit yang lebih sedikit. |
| **Matrix:** Investigation Status × Is_Repeat_Finding | Menghubungkan status investigasi dengan temuan berulang | Dari 87 kasus temuan berulang, **28 masih Open (32,2%)**; dari 118 kasus temuan baru, **48 masih Open (40,7%)**. Temuan berulang tidak lebih lambat diinvestigasi secara proporsi, namun 28 kasus tersebut menandakan masalah yang muncul lebih dari sekali dan belum tuntas. |
| **Slicer:** Site_Name & Risk_Level | Eksplorasi interaktif | Memungkinkan auditor langsung menyaring kasus per site atau tingkat risiko saat presentasi. |

---

## Halaman 2 — Financial Impact Assessment

| Visual | Fungsi | Insight Bisnis |
|---|---|---|
| **KPI Card:** Total Financial Exposure | Skala potensi kerugian | Total eksposur **Rp 70,46 miliar** selama 2023–2024. |
| **KPI Card:** Unresolved Exposure | Eksposur yang belum ditutup | **Rp 38,23 miliar** berasal dari temuan yang belum berstatus Closed. |
| **KPI Card:** % Unresolved Exposure | Rasio eksposur yang mengambang | **54,3%** dari total eksposur masih terbuka. Ini angka paling penting untuk dibawa ke manajemen. |
| **Bar Chart:** Exposure per Site | Menunjukkan konsentrasi risiko finansial per lokasi | **RS Harapan Sehat (Rp 12,30 M)**, **Apartemen Green Valley (Rp 9,87 M)**, dan **Gudang Logistik Cikarang (Rp 9,35 M)** menyumbang **44,7%** dari total eksposur — site yang sama dengan hasil analisis Bagian 1 dan 2. |
| **Donut Chart:** Exposure per Equipment Category | Distribusi eksposur per jenis peralatan | **HVAC (30,7%)** dan **Sprinkler (27,5%)** menyumbang 58,2% dari total, disusul Lift (22,0%), Genset (10,6%), dan Escalator (9,2%). Selaras dengan konsentrasi kasus investigasi di Halaman 1. |
| **Bar Chart:** Exposure per Risk Level | Menunjukkan tingkat risiko mana yang menanggung nilai terbesar | **Critical (Rp 41,15 M, 58,4%)** dan **High (Rp 23,14 M, 32,8%)** menyumbang **91,2%** dari total eksposur. Temuan Low hanya 0,7%. Prioritas mitigasi cukup difokuskan pada dua level teratas. |
| **Column Chart:** Rata-rata Exposure — Repeat vs New Finding | Membandingkan nilai eksposur rata-rata per catatan | Grafik menampilkan **Rp 104,6 juta** (berulang) dibanding **Rp 42,4 juta** (baru). **Perlu dibaca hati-hati:** kelompok "baru" mencakup audit yang Compliant dengan eksposur Rp 0, sehingga rata-ratanya tertekan. Jika hanya membandingkan temuan Non-Compliant, rata-rata temuan baru justru **Rp 119,3 juta** dan temuan berulang **Rp 104,6 juta**. Artinya, data ini **tidak menunjukkan** bahwa temuan berulang lebih mahal per kasus. |
| **Slicer:** Site_Name | Eksplorasi interaktif | Seluruh visual di halaman ini ikut menyesuaikan site yang dipilih. |

> **Tips akurasi:** agar grafik Repeat vs New sebanding, tambahkan filter tingkat visual `Maintenance_Compliance = Non-Compliant`.

---

## Kesimpulan

1. **Masalah terkonsentrasi, bukan menyebar.** Tiga site (RS Harapan Sehat, Apartemen Green Valley, Gudang Logistik Cikarang) konsisten menjadi kontributor utama non-compliance, risiko, keterlambatan follow-up, dan kini **44,7% dari seluruh eksposur finansial**.
2. **Lebih dari separuh eksposur masih terbuka.** Rp 38,23 miliar (54,3%) belum berstatus Closed, sehingga penurunan `% Unresolved Exposure` layak dijadikan KPI audit periode berikutnya.
3. **Risiko finansial terpusat pada temuan Critical dan High** (91,2% dari total) serta pada HVAC dan Sprinkler (58,2%).
4. **Backlog investigasi masih besar.** 37,1% kasus belum selesai, termasuk 28 kasus temuan berulang yang perlu eskalasi.
5. **Nilai temuan berulang terletak pada pembiarannya, bukan nilai rupiah per kasus.** Dari Bagian 2, temuan berulang lebih lambat diselesaikan (rata-rata 23,1 vs 17,0 hari) dan jauh lebih sering belum ditindaklanjuti (35,0% vs 13,6% pada temuan Non-Compliant baru).

## Rekomendasi

1. Prioritaskan audit lanjutan dan alokasi mitigasi pada tiga site dengan eksposur terbesar.
2. Jadikan `% Unresolved Exposure` (saat ini 54,3%) sebagai KPI utama laporan ke manajemen.
3. Terapkan eskalasi otomatis untuk temuan berulang yang belum ditindaklanjuti.
4. Fokuskan sumber daya investigasi pada HVAC dan Sprinkler, serta telusuri penyebab lonjakan kasus di Q1–Q2 2024.

## Tools

- **Power BI Desktop** — Power Query, DAX (measures), Card, Bar/Column Chart, Donut Chart, Matrix, Line Chart, Slicer

## Proyek Terkait

| Bagian | Tools | Fokus Analisis |
|---|---|---|
| Bagian 1 | Google Colab (Python) | Kesesuaian implementasi prosedur & identifikasi risiko |
| Bagian 2 | Microsoft Excel | Konfirmasi pihak terkait & efektivitas tindak lanjut |
| **Bagian 3 (proyek ini)** | **Power BI** | **Investigasi mendalam & penilaian dampak finansial** |

## Batasan (Limitations)

- Dataset bersifat **sintetis**; pola dan angka dirancang untuk keperluan portofolio dan tidak mewakili kondisi perusahaan nyata.
- Estimasi eksposur finansial adalah perkiraan, bukan kerugian aktual, dan belum divalidasi dengan data biaya riil.
- Analisis bersifat deskriptif; belum ada uji statistik untuk menguji signifikansi perbedaan antar kelompok.
- Data tidak memuat tanggal pembukaan kasus investigasi, sehingga umur backlog (aging) belum dapat dihitung.
