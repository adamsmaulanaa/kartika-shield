# Deploy Kartika Shield ke Render Free

## Tujuan
Menjalankan dashboard dan Collector dalam satu layanan Node.js. Setelah deploy, pengguna cukup membuka satu URL `https://....onrender.com`; dashboard akan otomatis memakai Collector pada domain yang sama.

## Langkah deploy
1. Pastikan perubahan dari paket ini sudah di-commit/push ke repository GitHub `noobagain/kartika-shield` pada branch `main`.
2. Buka `https://dashboard.render.com/blueprint/new?repo=https://github.com/noobagain/kartika-shield`.
3. Login/daftar Render, lalu pilih **Apply** / **Deploy Blueprint**.
4. Pastikan service `kartika-shield` menggunakan plan **Free**.
5. Tunggu sampai status deploy **Live**. Buka URL `onrender.com` yang diberikan Render.
6. Tes `https://NAMA-SERVICE.onrender.com/health`. Respons JSON dengan `uptimeSec` menandakan server hidup.
7. Di dashboard, buka Feed dan pastikan status Collector tersambung. RSS dipolling tiap 5 menit; bot Bluesky otomatis mencari sesuai interval 10 menit.

## Konfigurasi Render
- Build command: `echo "No npm dependencies required"`
- Start command: `npm run collector`
- Host: `0.0.0.0` (sudah diatur Blueprint)
- Port: jangan diisi manual; Render menyediakan `PORT` otomatis.
- Node.js: `package.json` meminta Node.js >= 22.

## Catatan paket gratis
Render Free menidurkan web service setelah 15 menit tanpa request masuk. Saat ada request baru, service akan bangun kembali (cold start). Filesystem layanan gratis bersifat sementara; arsip RSS yang ditulis saat runtime dapat hilang ketika service tidur/restart/redeploy. Karena itu paket ini cocok untuk demo/presentasi cepat, bukan jaminan kolektor 24/7 atau arsip permanen.

## File yang disesuaikan
- `collector/server.js`: menerima hostname dan origin yang otomatis diberikan Render; allowlist lokal tetap dipertahankan.
- `js/04-peta-dini.js`: default API ke domain dashboard yang sedang dibuka.
- `js/07-feed.js`: koneksi Feed/Bot otomatis ke domain dashboard yang sedang dibuka.
- `js/10-data.js`: default arsip berita ke domain dashboard yang sedang dibuka.
- `index.html`: placeholder alamat Collector diperjelas.
- `render.yaml`: konfigurasi deploy Render Free.

Tidak ada modul analisis, algoritma, atau sumber data yang diubah.
