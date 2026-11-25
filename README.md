# Eco Smart School — YPIK PAM JAYA

Aplikasi web responsif (HTML5/PWA) pemantauan pemilahan sampah sekolah lintas jenjang (**TK**, **SD**, **SMP**, dan **SMA/SMK**) yang terintegrasi dengan **Smart Waste Station (IoT/RFID)**, **Layar Tap Kartu RFID**, notifikasi otomatis tong penuh (`>80%`), verifikasi **Eco Ranger**, serta **Laporan Analisis Data Bulanan** (PDF, CSV, dan HTML).

## Fitur Utama

- **Pemilihan Tingkat / Level Sekolah**: Filter dan penyesuaian mode untuk jenjang `TK`, `SD`, `SMP`, dan `SMA/SMK`.
- **Layar Tap Kartu RFID**: Pemindaian kartu RFID pelajar maupun Kartu Kelas kolektif (`TK` & `SD 1–3`) secara langsung, pemilihan kompartemen (Kompos, Daur Ulang, Residu), penimbangan, dan pembaruan kapasitas tong real-time.
- **Dashboard Real-Time & Notifikasi Tong Penuh**: Pemantauan berat terpilah, akurasi pemilahan, serta peringatan otomatis saat kapasitas kompartemen melewati `80%`.
- **Laporan Analisis Bulanan**: Tren 6 bulan, *diversion rate*, rasio lintas jenjang, *insight* PjBL otomatis, dan ekspor PDF/CSV/HTML.
- **Mode Kelas Ramah Anak (TK & SD 1–3)**: Tanpa poin numerik kompetitif untuk TK, dilengkapi PIN gambar untuk SD awal serta checklist observasi guru.

## Struktur Proyek

```text
├── api/index.ts         # Entry point Serverless Function Vercel (/api/*)
├── docs/                # Dokumen PRD (tidak ikut di-deploy ke publik)
├── public/manifest.json # Manifest PWA
├── server/app.ts        # Router & API Express (/api/*)
├── server/store.ts      # Penyimpanan data JSON (lokal ./data atau /tmp di Vercel)
├── src/                 # Frontend React (komponen, utilitas, tipe)
├── index.html
├── package.json / package-lock.json
├── server.ts            # Server Express + Vite untuk lokal
├── tsconfig.json
├── vercel.json          # Build SPA + routing API di Vercel
└── vite.config.ts
```

## Menjalankan di Lokal

Butuh **Node.js 20.11 atau lebih baru**.

```bash
npm install
npm run dev            # http://localhost:3000 (frontend + API)
```

Perintah lain:

| Perintah | Fungsi |
| --- | --- |
| `npm run lint` | Cek tipe TypeScript |
| `npm run build` | Build frontend ke folder `dist/` |
| `npm start` | Server produksi lokal (sajikan `dist/` + API) — jalankan `npm run build` dulu |

> Folder `data/` dibuat otomatis saat server pertama jalan (berisi `db.json`) dan sengaja tidak di-commit. Data awal (seed) ada di `server/store.ts`.

## Deploy ke GitHub & Vercel

```bash
git init
git add .
git commit -m "Initial commit: Eco Smart School"
git branch -M main
git remote add origin https://github.com/<username>/<nama-repo>.git
git push -u origin main
```

Lalu di Vercel: **Add New → Project → Import** repo tersebut → **Deploy**. Semua pengaturan build terbaca otomatis dari `vercel.json` (Vite, `npm run build`, output `dist`). Tidak ada environment variable yang wajib diisi.

| Variabel (opsional) | Keterangan |
| --- | --- |
| `DATA_DIR` | Path penyimpanan JSON kustom (default lokal `./data`, Vercel `/tmp/eco-smart-school-data`) |
| `PORT` | Port server lokal (default `3000`) |

> **Penting — penyimpanan di Vercel:** API berjalan sebagai serverless function, sehingga data yang ditulis (setoran, pengguna baru, dll.) disimpan di memori/`/tmp` dan **bisa hilang atau berbeda antar-instance** setiap kali function di-restart. Cocok untuk demo. Untuk pemakaian sungguhan, sambungkan `server/store.ts` ke database eksternal (mis. Supabase/Postgres atau Upstash Redis).
