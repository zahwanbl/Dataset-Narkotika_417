<div align="center">

# ⚖️ Dataset Putusan Narkotika — PN Tangerang

**50 putusan pengadilan tingkat pertama perkara Narkotika dan Psikotropika, siap dipakai untuk eksperimen *Information Retrieval*.**

![Dokumen](https://img.shields.io/badge/dokumen-50%20PDF-blue)
![Pengadilan](https://img.shields.io/badge/pengadilan-PN%20Tangerang-green)
![Klasifikasi](https://img.shields.io/badge/klasifikasi-Pidana%20Khusus%20·%20Narkotika-orange)
![Bahasa](https://img.shields.io/badge/bahasa-Indonesia-red)
![Mata%20Kuliah](https://img.shields.io/badge/UTS-Temu%20Kembali%20Informasi-purple)

</div>

---

## 📖 Tentang Dataset

Repositori ini berisi kumpulan **50 dokumen putusan** dari **Pengadilan Negeri Tangerang** pada direktori **Pidana Khusus → Narkotika dan Psikotropika**, yang diunduh secara manual dari [Direktori Putusan Mahkamah Agung RI](https://putusan3.mahkamahagung.go.id/direktori.html).

Dataset disusun untuk **UTS mata kuliah Temu Kembali Informasi (Information Retrieval)**, Fakultas Teknik, Universitas Muhammadiyah Malang (semester ganjil 2026/2027), dan dapat dipakai untuk:

- 🔎 membangun mesin pencari dokumen hukum (*inverted index*, TF-IDF, BM25)
- 🧹 latihan *preprocessing* teks hukum berbahasa Indonesia
- 🕷️ latihan *web crawling* dan ekstraksi informasi (regex / *information extraction*)
- 📊 analisis awal perkara narkotika (jenis barang bukti, amar putusan, dll.)

## 🗂️ Struktur Repositori

```
Dataset-Narkotika_417/
├── Dataset/
│   └── Narkotika.zip        # 50 putusan dalam format PDF
├── Overview/
│   └── Overview.xlsx        # ringkasan 50 putusan (5 kolom)
└── README.md
```

## 📦 Isi Dataset

| Aspek | Keterangan |
|---|---|
| Jumlah dokumen | 50 file PDF (± 11,7 MB) |
| Total halaman | ± 1.734 halaman (13–115 halaman per putusan, median 32) |
| Pengadilan | PN Tangerang |
| Jenis perkara | Pidana Khusus — Narkotika dan Psikotropika (nomor perkara `Pid.Sus`) |
| Tingkat proses | Pertama |
| Tahun nomor perkara | 2024 (29 putusan) dan 2026 (21 putusan) |
| Bahasa | Indonesia |
| Format | PDF dengan *text layer* (dapat diekstrak tanpa OCR) |

### Gambaran jenis barang bukti
Jumlah dokumen yang menyebut tiap jenis narkotika (satu dokumen bisa menyebut lebih dari satu jenis):

| Jenis | Dokumen |
|---|---:|
| Sabu / metamfetamin | 36 |
| Tembakau sintetis | ± 31 |
| Ganja | 8 |
| Ekstasi / MDMA | 7 |

> Angka dihitung dengan pencocokan kata kunci pada teks putusan, sehingga bersifat perkiraan.

## 📑 Struktur `Overview.xlsx`

| Kolom | Isi |
|---|---|
| **No** | Nomor urut 1–50 (sesuai awalan nama file PDF) |
| **No Putusan** | Nomor perkara, contoh `1768/Pid.Sus/2024/PN Tng` |
| **Lembaga Peradilan** | Pengadilan yang memutus (PN Tangerang) |
| **Barang Bukti** | Barang bukti sebagaimana ditetapkan dalam amar putusan |
| **Amar Putusan** | Bagian amar (setelah kata *MENGADILI*) sampai sebelum penutup putusan |

Nama file PDF mengikuti pola `NN_putusan_<nomor>_pid.sus_<tahun>_pn_tng_<timestamp>.pdf`, dengan `NN` sama dengan kolom **No** pada Overview.

## 🛠️ Cara Pengumpulan & Pembuatan

1. Seluruh putusan diunduh **manual** dari Direktori Putusan MA (Pidana Khusus → Narkotika dan Psikotropika → PN Tangerang).
2. Putusan yang dipilih adalah putusan tingkat pertama yang belum berstatus berkekuatan hukum tetap (sesuai ketentuan tugas).
3. Kolom *Barang Bukti* dan *Amar Putusan* diekstrak dari teks PDF memakai **regex**: amar diambil dari kata `MENGADILI` hingga kalimat penutup `Demikianlah diputuskan…`; barang bukti dari butir amar `Menetapkan/Memerintahkan/Menyatakan … barang bukti berupa`.
4. Noise watermark dan *footer* halaman MA dibersihkan sebelum ekstraksi.

## 🚀 Contoh Penggunaan

```python
import zipfile, pandas as pd

# 1) ekstrak dataset
with zipfile.ZipFile("Dataset/Narkotika.zip") as z:
    z.extractall("data_putusan")

# 2) baca ringkasan
df = pd.read_excel("Overview/Overview.xlsx")
print(df[["No", "No Putusan", "Lembaga Peradilan"]].head())

# 3) cari putusan yang memuat kata "ganja" pada barang bukti
print(df[df["Barang Bukti"].str.contains("ganja", case=False, na=False)]["No Putusan"].tolist())
```

## ⚠️ Catatan & Keterbatasan

- Hasil ekstraksi berbasis regex; beberapa putusan dengan format tidak baku dapat menghasilkan potongan teks yang kurang rapi. Sisa watermark MA (potongan huruf) kadang terselip pada teks.
- Barang bukti pada putusan dengan banyak terdakwa/barang bukti (mis. No. 48) sangat panjang.
- Status "berkekuatan hukum tetap" suatu putusan dapat berubah setelah diunggah; periksa kembali pada situs MA bila diperlukan.

## 🔐 Etika & Sumber Data

Putusan bersumber dari publikasi resmi **Direktori Putusan Mahkamah Agung RI** dan bersifat terbuka untuk publik. Dataset ini dibagikan **hanya untuk keperluan akademik dan pembelajaran**. Putusan memuat identitas pihak perkara; gunakan secara bertanggung jawab dan jangan dipakai untuk menyerang atau mempermalukan individu.

## 👩‍💻 Penyusun

**Siti Zahwa Nabila Putri** — NIM 202310370311417  
Informatika, Universitas Muhammadiyah Malang  
Mata kuliah: Temu Kembali Informasi (Dosen: Galih W. W.)  
GitHub: [@zahwanbl](https://github.com/zahwanbl)

---
<div align="center">Dibuat untuk UTS Temu Kembali Informasi · UMM 2026/2027</div>
#
