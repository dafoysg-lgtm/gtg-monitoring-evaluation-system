# Gate-to-Gate Monitoring & Evaluation System

**PT Indocement Tunggal Prakarsa Tbk. — Divisi Central Dispatch, Departemen Logistik**
Inisiatif mandiri selama periode Kerja Praktik (Juni–Juli 2026)

Proyek ini adalah *business process improvement* dua tahap terhadap sistem monitoring Gate to Gate (G2G) — indikator lama proses kendaraan dari masuk hingga keluar area operasional, yang jika melebihi standar dikategorikan sebagai Over Time (OT).

Keduanya lahir dari observasi mandiri di lapangan, bukan penugasan resmi. Setelah memahami alur kerja operator, gap-nya diidentifikasi sendiri, konsepnya diajukan ke mentor lapangan, baru dikembangkan.

## Struktur Proyek

### [v1 — Daily Monitoring Improvement](./v1-daily-monitoring)
Perbaikan workbook monitoring harian: redesign layout, standardisasi kategori investigasi lewat dropdown (Data Validation), indikator visual status investigasi (Conditional Formatting), dan dashboard KPI harian — seluruhnya dibangun dengan formula Excel Online tanpa VBA/macro.

### [v2 — Monthly Evaluation Dashboard](./v2-monthly-evaluation)
Lanjutan dari v1. Mengonsolidasikan 31 sheet monitoring harian ke dalam satu database terpusat dan dashboard evaluasi bulanan, lengkap dengan ringkasan naratif yang ter-generate otomatis dari data.

## Alur Pengembangan

```

v1: Benerin proses monitoring harian yang scroll panjang, gak ada indikator visual, kategori manual
↓
v2: Begitu harian stabil, gap berikutnya kelihatan — belum ada cara cepat lihat performa SEBULAN
↓
Hasil: sistem monitoring dua lapis, harian buat operasional, bulanan buat evaluasi

```

## Prinsip Utama

Kedua versi sengaja dibangun **tanpa mengubah workflow operasional** yang sudah berjalan dan **tanpa VBA/macro**, karena workbook dipakai di Microsoft Excel Online (bukan desktop) — jadi solusinya harus murni formula-driven agar konsisten di semua environment.

Detail teknis, formula, dan studi kasus lengkap ada di masing-masing folder v1 dan v2.
