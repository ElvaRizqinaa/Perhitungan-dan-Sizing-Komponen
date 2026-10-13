# Perhitungan dan Sizing Komponen Sistem Kontrol Motor Listrik

Repositori ini berisi materi, panduan visual, serta referensi praktis mengenai perhitungan dan penentuan ukuran (*sizing*) komponen pada sistem kontrol motor listrik 3-fasa. Materi ini disusun untuk memenuhi tugas mata kuliah **Praktikum Mesin-Mesin Listrik**.

---

## Daftar Isi
1. [Deskripsi Materi](#-deskripsi-materi)
2. [Rumus Dasar Arus Beban Penuh ($I_{FL}$)](#-rumus-dasar-arus-beban-penuh-i_fl)
3. [Matriks Sizing Komponen (DOL vs. Star-Delta)](#-matriks-sizing-komponen-dol-vs-star-delta)
4. [Studi Kasus Perhitungan](#-studi-kasus-perhitungan)
5. [Praktik Terbaik Industri](#-praktik-terbaik-industri)
6. [Identitas Penyusun](#-identitas-penyusun)

---

## Deskripsi Materi
Dalam merancang panel kontrol kelistrikan di industri, pemilihan spesifikasi komponen yang tepat sangat penting untuk menjamin keamanan operasional, mencegah *trip* palsu (*nuisance tripping*), dan menghindari risiko kerusakan mesin atau kebakaran kabel. 

Repositori ini merangkum 4 modul utama:
* **Modul 1:** Konsep dan perhitungan dasar Arus Beban Penuh (*Full Load Current* / $I_{FL}$).
* **Modul 2:** *Sizing* komponen dasar untuk rangkaian *Direct-On-Line* (DOL).
* **Modul 3:** *Sizing* komponen lanjutan untuk rangkaian *Star-Delta* ($\text{Y}-\Delta$).
* **Modul 4:** Penggunaan *Sizing Cheat Sheet* dan tips desain panel kontrol.

---

## Rumus Dasar Arus Beban Penuh ($I_{FL}$)

Acuan utama dalam menentukan seluruh ukuran komponen panel adalah nilai **$I_{FL}$**. Nilai ini dapat dilihat langsung pada *nameplate* motor atau dihitung secara manual menggunakan rumus motor 3-fasa berikut:

### 1. Jika Daya dalam KiloWatt (kW)
$$I_{FL} = \frac{kW \times 1000}{V_L \times \sqrt{3} \times \cos\phi \times \eta}$$

### 2. Jika Daya dalam Horsepower (HP)
$$I_{FL} = \frac{HP \times 746}{V_L \times \sqrt{3} \times \cos\phi \times \eta}$$

**Keterangan:**
* $I_{FL}$ : Arus beban penuh (*Ampere*)
* $V_L$ : Tegangan *line-to-line* (misal: 380V / 400V)
* $\sqrt{3}$ : Konstanta sistem 3-fasa ($\approx 1.732$)
* $\cos\phi$ : Faktor daya (*Power Factor*)
* $\eta$ : Efisiensi motor (dalam bentuk desimal)

---

## Matriks Sizing Komponen (DOL vs. Star-Delta)

Berikut adalah tabel perbandingan faktor pengali untuk menentukan kapasitas minimal komponen antara sistem *Direct-On-Line* (DOL) dan *Star-Delta*:

| Komponen | Sistem DOL | Sistem Star-Delta | Keterangan |
| :--- | :---: | :---: | :--- |
| **Kabel Utama** | $1.25 \times I_{FL}$ | $1.25 \times I_{FL}$ | Menentukan nilai KHA (Kemampuan Hantar Arus) |
| **MCB / Breaker Utama** | $2.5 \times I_{FL}$ | $2.5 \times I_{FL}$ | Mengantisipasi lonjakan arus *starting* (*inrush current*) |
| **Kontaktor Main** | $\ge 1.0 \times I_{FL}$ | $0.58 \times I_{FL}$ | Wajib menggunakan kategori **Duty AC-3** |
| **Kontaktor Delta ($\Delta$)** | *N/A* | $0.58 \times I_{FL}$ | Memikul arus fasa ($\frac{I_{FL}}{\sqrt{3}}$) |
| **Kontaktor Star (Y)** | *N/A* | $0.33 \times I_{FL}$ | Hanya bekerja sesaat ketika *start-up* |
| **Setting TOR** | $1.0 \times I_{FL}$ | $0.58 \times I_{FL}$ | Pada Star-Delta, TOR dipasang di bawah Kontaktor Main |

---

## Studi Kasus Perhitungan

**Spesifikasi Nameplate Motor Induksi 3-Fasa:**
* **Daya ($P$):** 22.0 kW
* **Tegangan ($V$):** 400 V ($\Delta$)
* **Frekuensi:** 50 Hz
* **Faktor Daya ($\cos\phi$):** 0.83
* **Efisiensi ($\eta$):** 93.0% (0.93)
* **Arus Nominal ($I_{FL}$ Nameplate):** 41.0 A

### Hasil Perhitungan Komponen:
1. **Sistem DOL:**
   * **MCB/MCCB:** $2.5 \times 41.0\text{ A} = \mathbf{102.5\text{ A}}$ *(Gunakan ukuran standar 100 A / 125 A)*
   * **KHA Kabel:** $1.25 \times 41.0\text{ A} = \mathbf{51.25\text{ A}}$
   * **Kontaktor:** Minimal $\mathbf{41.0\text{ A}}$ *(Kategori AC-3)*
   * **Setting TOR:** Diatur pada $\mathbf{41.0\text{ A}}$

2. **Sistem Star-Delta:**
   * **Kontaktor Main & Delta:** $0.58 \times 41.0\text{ A} = \mathbf{23.78\text{ A}}$ *(Gunakan standar 25 A / 32 A)*
   * **Kontaktor Star:** $0.33 \times 41.0\text{ A} = \mathbf{13.53\text{ A}}$ *(Gunakan standar 18 A / 25 A)*
   * **Setting TOR (di bawah Main):** $0.58 \times 41.0\text{ A} = \mathbf{23.78\text{ A}}$

---

## Praktik Terbaik Industri
1. **Keamanan (*Upsize* Kabel):** Selalu naikkan satu ukuran luas penampang kabel jika jarak penarikan kabel dari panel ke motor sangat jauh untuk mencegah penurunan tegangan (*voltage drop* maksimal 3% - 5%).
2. **Keandalan (*Duty AC-3*):** Selalu gunakan kontaktor dengan standar *Duty AC-3* untuk beban motor induktif agar kontak internal tidak mudah aus atau meleleh.
3. **Efisiensi (Tata Letak Panel):** Pastikan sirkulasi udara dan *ducting* kabel di dalam panel tertata rapi untuk mencegah suhu ruang panel memanas yang dapat memicu TOR mengalami *trip* palsu.

---

## Identitas Penyusun

* **Nama:** Elva Rizqina Khoirun Nisa
* **NIM:** 40040325650031
* **Program Studi:** Teknologi Rekayasa Otomasi
* **Departemen:** Teknologi Industri
* **Fakultas:** Sekolah Vokasi - Universitas Diponegoro

**Dosen Pengampu:**
* Rofiq Cahyo Prayogo, S.T., M.T.
* Priyo Sasmoko, S.T., M.Eng.
