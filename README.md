Project ini berisi data dummy untuk sistem penyewaan game dan konsol.
Data dibuat menggunakan dua metode, yaitu pembuatan manual untuk tabel berukuran kecil dan pembuatan otomatis (generate) untuk tabel berukuran besar.
Tujuan utama pembuatan data ini adalah untuk pengujian relasi antar tabel serta performa sistem database.

🧩 Struktur Jumlah Data
Tabel	Jumlah Data	Metode	Keterangan
staff	10	Manual	Menyimpan data karyawan atau petugas
pelanggan	100	Manual	Menyimpan data pelanggan
game	1000	Otomatis	Menyimpan daftar game PS4 dan PS5
konsol	100	Manual	Menyimpan data unit konsol yang disewakan
sewa	10.000	Otomatis	Menyimpan transaksi penyewaan
detail_sewa	10.000	Otomatis	Menyimpan detail dari setiap transaksi sewa
pembayaran	10.000	Otomatis	Menyimpan data pembayaran transaksi sewa
✍️ Metode Pembuatan Data
1. Metode Manual

Metode manual dilakukan dengan menuliskan perintah INSERT INTO ... VALUES (...) secara langsung pada file SQL.
Digunakan untuk tabel dengan jumlah data kecil agar lebih mudah dikontrol dan diperiksa.

Contoh:

INSERT INTO staff (id_staff, nama_staff, username, password, role) VALUES
('ST00001','Rizky Saputra','rizky','123','Staff'),
('ST00002','Taufik Hidayat','taufik','123','Administrator');

2. Metode Otomatis (Generate Otomatis PostgreSQL)

Metode otomatis digunakan untuk tabel dengan jumlah data besar seperti game, sewa, detail_sewa, dan pembayaran.
Metode ini memanfaatkan fungsi generate_series() di PostgreSQL untuk menghasilkan data dalam jumlah besar dengan pola tertentu.

Contoh:

INSERT INTO game (id_game, nama_game, platform, kategori)
SELECT 
    'G' || LPAD(i::text, 5, '0'),
    'Game_' || i,
    CASE WHEN i % 2 = 0 THEN 'PS5' ELSE 'PS4' END,
    CASE 
        WHEN i % 5 = 0 THEN 'Action'
        WHEN i % 5 = 1 THEN 'Adventure'
        WHEN i % 5 = 2 THEN 'Racing'
        WHEN i % 5 = 3 THEN 'Sport'
        ELSE 'Horror'
    END
FROM generate_series(1,1000) AS s(i);

⚙️ Logika Metode Otomatis

generate_series(1, n)
Digunakan untuk membuat deretan angka dari 1 hingga n.
Misalnya, generate_series(1,1000) menghasilkan 1000 baris data numerik.

LPAD(i::text, 5, '0')
Mengubah angka menjadi teks dengan menambahkan nol di depan agar panjangnya lima digit (contoh: 00001).

'G' || LPAD(...)
Menggabungkan huruf dan angka untuk membentuk kode ID seperti G00001, G00002, dan seterusnya.

CASE WHEN ... THEN ... ELSE ... END
Memberikan variasi nilai agar data tidak identik, misalnya mengatur platform antara PS4 dan PS5 atau mengatur kategori game secara bergantian.

INSERT INTO ... SELECT ...
Menggunakan hasil dari perintah SELECT untuk langsung dimasukkan ke tabel tanpa menuliskan setiap nilai secara manual.

🧠 Alasan Penggunaan Metode Otomatis

Efisien: Mempercepat proses pengisian data dalam jumlah besar.

Konsisten: Format data lebih teratur dan mudah dikontrol.

Mudah Dimodifikasi: Jumlah data dapat diubah dengan mengganti angka pada generate_series().

Mendukung Pengujian Performa: Data berjumlah ribuan diperlukan untuk menguji efisiensi query dan kestabilan sistem.

📊 Kesimpulan

Penggunaan kombinasi metode manual dan otomatis mempermudah pembuatan data dummy dalam proyek ini.
Metode manual digunakan untuk tabel kecil yang memerlukan data tetap dan mudah dibaca,
sedangkan metode otomatis digunakan untuk tabel besar guna menghemat waktu dan menjaga konsistensi data.

Struktur data dummy ini dapat digunakan untuk simulasi sistem penyewaan game dan pengujian relasi antar tabel dalam database PostgreSQL.
