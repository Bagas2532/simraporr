# RuangRapor — Sistem Informasi Rapor (SMK Negeri 5 Surakarta)

Aplikasi e-rapor berbasis Laravel + MySQL: nilai akhir, total, **peringkat** dan **siswa terbaik** dihitung otomatis,
rapor bisa **dicetak** (A4 / simpan PDF dari browser).

## Masuk dan pendaftaran
| Peran | Alamat | Login |
|---|---|---|
| Semua peran | `/login` | Pilih siswa, guru/wali, atau administrator |
| Siswa | `/login` | NIS + password atau akun Google yang sudah ditautkan |
| Guru / Wali Kelas | `/guru/login` | username + password (wali kelas = guru yang ditunjuk di menu Kelas) |
| Administrator | `/admin/login` | username + password |

Siswa mendaftar melalui `/register` dengan NIS, NISN, dan password baru (minimal 8 karakter).
Jika data cocok dengan catatan sekolah, akun langsung dibuat dan siswa masuk ke dashboard dengan notifikasi berhasil.
Pendaftaran dengan Google juga tersedia; Google OAuth perlu dikonfigurasi melalui `GOOGLE_CLIENT_ID`,
`GOOGLE_CLIENT_SECRET`, dan `GOOGLE_REDIRECT_URI`.

## Fitur
**Administrator:** data siswa (CRUD, **Import & Export CSV**, template), guru, kelas & wali kelas, mata pelajaran, guru mapel, ekstrakurikuler, tahun akademik (semester aktif), jenis nilai + bobot, setting aplikasi (identitas sekolah/kepala sekolah di rapor), peringkat 1-3 tiap kelas & siswa terbaik, rekap & cetak rapor semua kelas, **Unduh Leger Nilai (CSV)**, sampah (pulihkan / hapus permanen), reset password, **Peminjaman Rapor Siswa** (verifikasi persetujuan/penolakan, serah terima berkas fisik, monitoring keterlambatan/overdue, pencatatan peminjaman offline, pengembalian & cek kondisi berkas, cetak surat tanda terima & rekapitulasi laporan TU).

**Guru:** input & ubah nilai per mapel (hanya mapel/kelas yang ia ajar), **Unduh Template CSV & Import Nilai CSV**, lihat hasil nilai, cetak nilai mapel.

**Wali Kelas (guru yang ditunjuk):** setting rapor siswa (kehadiran, catatan, ekstrakurikuler), data orang tua/wali murid, lihat detail nilai tiap siswa, nilai per mapel, **cetak rapor**, **Unduh Leger Nilai Kelas (CSV)**, cetak nilai per mapel, cetak total nilai & peringkat kelas.

**Siswa/orang tua:** melihat dan mencetak rapor miliknya sendiri; **Peminjaman Rapor Siswa** (mengajukan peminjaman buku rapor fisik untuk keperluan kuliah/kerja/beasiswa, melacak status verifikasi, mencetak bukti permohonan peminjaman, membatalkan permohonan); ganti password.

## Cara Menghitung
- Nilai akhir mapel = rata-rata berbobot dari jenis nilai yang sudah terisi (Tugas/UH/UTS/UAS, bobot diatur Admin).
- Total = jumlah nilai akhir semua mapel; rata-rata = total / jumlah mapel terisi.
- Peringkat kelas = urut total tertinggi; total sama = peringkat sama (1,1,3). Siswa terbaik = urut rata-rata seluruh sekolah.

## Cara Menjalankan

### Cara 1: Menggunakan Launcher 1-Klik (Windows)
Cukup klik ganda berkas launcher **`Jalankan-SIMRAPOR.bat`** (nama file lama). Pastikan MySQL sudah berjalan (di XAMPP, klik **Start** pada MySQL)
sebelum login; browser akan otomatis terbuka ke `http://localhost:8000`. PHP harus mengaktifkan ekstensi `mbstring`
dan `pdo_mysql`.
Untuk membuka dari HP Android atau komputer lain dalam Wi-Fi yang sama, jalankan `ipconfig` di komputer server,
lalu buka `http://IP-IPv4-KOMPUTER:8000` di perangkat tersebut. Launcher menerima koneksi jaringan lokal.
Jika Windows Firewall meminta izin, izinkan PHP hanya pada jaringan privat. HP dan komputer server harus memakai
jaringan Wi-Fi yang sama; jangan gunakan alamat `localhost` dari HP.
Untuk akses internet, gunakan hosting/VPS dengan domain dan HTTPS seperti dijelaskan di **PANDUAN-DEPLOY.md**;
jangan membuka server lokal/port development langsung ke internet.

### Cara 2: Manual lewat Terminal
```
composer update
copy .env.example .env      # lalu isi DB_*; pastikan CACHE_STORE=file
php artisan key:generate
php artisan migrate:fresh --seed   # PERINGATAN: menghapus semua data; seeder demo hanya di APP_ENV=local
php artisan serve --host=0.0.0.0 --port=8000
```
Pastikan server MySQL dengan database yang tercantum di `DB_*` sudah berjalan sebelum menjalankan aplikasi.
Untuk akses Android melalui jaringan lokal, cari IPv4 komputer server dengan `ipconfig`, pastikan port 8000
diizinkan hanya pada jaringan privat, lalu buka `http://IP-IPv4-KOMPUTER:8000` di HP yang terhubung ke Wi-Fi
yang sama. Perintah server di atas hanya untuk pengujian lokal, bukan hosting publik.
Akun demo (hanya lokal): admin `admin.ratna` / `password`; guru `guru.budi` (wali XII RPL A) dan `guru.sari` (wali XII RPL B) / `password`;
siswa NIS `22145` / `22145`. Di server sungguhan buat admin dengan `php artisan simrapor:buat-admin username`.

Tes otomatis: `php artisan test`. Panduan server sungguhan: **PANDUAN-DEPLOY.md**.
# simraporr
