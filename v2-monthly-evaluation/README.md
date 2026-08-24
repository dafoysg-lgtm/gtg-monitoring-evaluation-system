# Monthly Gate-to-Gate Monitoring & Evaluation Dashboard

**PT Indocement Tunggal Prakarsa Tbk. — Divisi Central Dispatch, Departemen Logistik**
Pengembangan mandiri, diselesaikan sebelum akhir periode Kerja Praktik (Juli 2026)

> Lanjutan dari [v1 — Daily Monitoring Improvement](../v1-daily-monitoring).

---

## Latar Belakang

Monitoring G2G harian sudah berjalan menggunakan workbook yang diperbaiki di v1. Tapi data harian tersebar di sheet-sheet terpisah per tanggal — untuk lihat performa satu bulan, supervisor harus buka dan bandingkan sheet satu per satu.

Gak ada yang minta ini dibenerin. Gap-nya ketahuan sendiri pas mengamati cara kerja tim: workbook harian sudah cukup rapi, tapi belum ada lapisan evaluasi bulanan. Konsepnya diajukan ke mentor lapangan, disetujui, baru dikembangkan.

## Masalah

- Data performa bulanan gak ada di satu tempat — harus buka 31 sheet manual buat lihat tren.
- Gak ada cara cepat buat tau plant mana yang paling sering OT sepanjang bulan.
- Gak ada ringkasan naratif — semua angka mentah, supervisor yang harus menyimpulkan sendiri.

## Solusi

Arsitektur 3 layer:

```

31 Sheet Harian (Tanggal 1–31)
↓
Database (konsolidasi KPI harian)
↓
Dashboard Bulanan (visual + insight otomatis)

```

**Daily Layer** — 31 sheet, satu per tanggal, struktur seragam mengikuti template harian dari v1.

**Database Layer** — Menarik hasil KPI dari tiap sheet harian ke satu tabel konsolidasi (Total Alert, Rata-rata Menit, Plant Impacted, Completion, per tanggal).

**Dashboard Layer** — Agregasi bulanan: Total Alert, Average Minutes, Plant Impacted, Trend Alert, Trend Minutes, Top Plant, Cause Distribution, plus **ringkasan naratif otomatis**.

## Detail Teknis

Pendekatan formula bersifat **hybrid** — ini keputusan yang disadari, bukan keterbatasan:

- **31 baris di Database di-mapping manual** ke sheet tanggal masing-masing (`='1'!$K$37`, `='6'!$K$37`, dst). Trade-off-nya: kalau struktur sheet harian berubah, 31 reference ini perlu disesuaikan satu-satu — risikonya rendah karena template harian dari v1 sudah stabil.
- **Layer kategori/summary pakai `INDIRECT`** untuk membangun referensi sheet secara dinamis dari nomor tanggal, jadi gak perlu retype formula 31 kali untuk perhitungan kategori.
- Kombinasi `INDEX` + `MATCH` + `MAX` + `TEXT` + concatenation buat generate insight naratif otomatis, contoh:

  ```
  ="• Alert tertinggi terjadi pada tanggal "&INDEX(Database!$C$3:$C$33,
     MATCH(MAX(Database!$D$3:$D$33),Database!$D$3:$D$33,0))&
     " dengan "&MAX(Database!$D$3:$D$33)&" Alert."
  ```

- `AVERAGEIF(...,">0")` dibungkus `IFERROR` supaya hari tanpa alert (nilai 0) gak ngerusak rata-rata bulanan.
- Tanpa VBA/macro — konsisten dengan keputusan di v1, karena workbook dipakai di Excel Online (bukan desktop), jadi macro-based automation gak bisa diandalkan lintas environment.

## Sebelum vs Sesudah

| | Sebelum | Sesudah |
|---|---|---|
| Lihat performa bulanan | Buka 31 sheet manual | 1 dashboard terpusat |
| Identifikasi plant bermasalah | Baca satu-satu | Top Plant otomatis ranking |
| Ringkasan performa | Gak ada | Narasi otomatis dari data |
| Update saat data berubah | Manual | Otomatis (formula-driven) |

## Validasi

Workbook diuji menggunakan data operasional yang sama dengan sistem lama, untuk membandingkan konsistensi output — bukan mengukur data berbeda. Fokus validasi: akurasi kalkulasi KPI, konsistensi agregasi bulanan, dan kesesuaian hasil dashboard dengan data sumber.

*(Belum ada pengukuran kuantitatif seperti persentase penghematan waktu — kalau ke depan mau diukur, ini yang direkomendasikan sebagai lanjutan.)*

## Kontribusi Personal

- Identifikasi gap monitoring harian → evaluasi bulanan (inisiatif mandiri, bukan penugasan)
- Desain arsitektur Daily → Database → Dashboard
- Pengembangan seluruh formula (agregasi, lookup, insight generation)
- Testing terhadap data operasional aktual
- Presentasi & pengajuan konsep ke mentor lapangan untuk approval

## Skill yang Terdemonstrasi

Business Process Analysis · Data Consolidation · Excel Formula Engineering (`INDIRECT`, `INDEX/MATCH`, `AVERAGEIF`, `TEXT`) · Dashboard Design · Stakeholder Communication

## Catatan

Inisiatif mandiri mahasiswa selama Kerja Praktik, disetujui secara informal oleh mentor lapangan dan mendapat apresiasi dari Section Head serta tim Central Dispatch. Data operasional yang ditampilkan pada screenshot bukan data sensitif dan sudah disetujui untuk ditampilkan secara publik.

## Screenshots

## Screenshots

**Dashboard Evaluasi Bulanan**
![Dashboard Evaluasi Bulanan](./screenshots/01%20dasboard%20evaluasi%20bulanan.png)

**Database Konsolidasi (31 Sheet Harian → 1 Tabel)**
![Database Consolidation View](./screenshots/02%20database%20consolidation%20view.png)

**Formula Bridge Database (Hybrid: Manual Mapping + INDIRECT)**
![Bridge Formula Database](./screenshots/03%20brigde%20formula%20database.png)
