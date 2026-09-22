# Tugas 1 — Analisis Pitfall FoodGo

**Kelompok:** [nama kelompok]

| Nama | NIM | Kontribusi |
|---|---|---|
| [Delon] | [103072400156] | [Arsitektur Monolitik dan Single Point of Failure] |
| [Delon] | [103072400156] | [Double Payment] |
| [nama 3] | [nim] | [pitfall/bagian yang dikerjakan] |

## Pitfall 1: [Arsitektur Monolitik dan Single Point of Failure] — ditulis oleh [Delon]

**Bukti di skenario:** Saat trafik naik,satu server yang menangani semua modul (pesanan, pembayaran, notifikasi kurir) kewalahan karena semuanya berjalan di satu proses monolitik yang sama, hingga server kadang crash total dan perlu di-restart manual.

**Kenapa ini keliru:** Penggunaan satu server untuk menangani seluruh fungsi sistem menjadi masalah ketika jumlah pengguna dan trafik semakin tinggi. Semua proses saling bergantung pada server yang sama. Jadi, ketika server mengalami masalah atau kehabisan sumber daya, seluruh bagian sistem juga ikut terdampak. Hal ini membuat sistem sulit untuk tetap berjalan ketika terjadi lonjakan trafik.

**Dampak ke FoodGo:** Karena menggunakan sumber daya dari satu server yang sama untuk semua modul, tidak ada pembagian beban atau pemisahan layanan, server bisa kehabisan kapasitas. Akibatnya sistem crash secara keseluruhan.

**Solusi desain awal:** Solusi yang dapat diterapkan adalah memisahkan modul-modul menjadi layanan terpisah (microservices) yang berjalan di server berbeda. Dengan begitu, jika satu layanan mengalami masalah, layanan lainnya tetap bisa berjalan. Selain itu, bagian yang memiliki beban tinggi, seperti layanan pesanan, bisa ditambah kapasitasnya secara mandiri tanpa harus ikut meningkatkan seluruh sistem.

**Trade-off:** Pemecahan sistem menjadi layanan terpisah meningkatkan kompleksitas pengelolaan infrastruktur, komunikasi jaringan antar-service, serta kesulitan dalam proses debugging dan deployment.

---

## Pitfall 2: [Double Payment] — ditulis oleh [Delon]

**Bukti di skenario:** Pada saat trafik sedang tinggi, sistem mengalami timeout dan respons yang lambat. Kondisi ini bisa membuat pengguna menekan tombol bayar lagi atau aplikasi melakukan retry secara otomatis tanpa mengecek terlebih dahulu apakah transaksi sebelumnya sudah berhasil.

**Kenapa ini keliru:** Kesalahan ini terjadi karena sistem menganggap satu kali klik tombol bayar pasti hanya menghasilkan satu permintaan. Padahal, dalam sistem terdistribusi, gangguan jaringan bisa membuat request yang sama dikirim lebih dari satu kali. Jika sistem tidak bisa membedakan request yang sama, transaksi tersebut bisa diproses berulang.

**Dampak ke FoodGo:** Misalnya pengguna sudah menekan tombol "Bayar", tetapi karena jaringan lambat tidak ada respons dari server. Pengguna kemudian menekan tombol tersebut lagi. Jika pembayaran pertama sebenarnya sudah berhasil, tetapi sistem belum memberikan respons, pembayaran kedua akan dianggap sebagai transaksi baru. Akibatnya, saldo pengguna bisa terpotong dua kali dan pengguna akan mengalami kerugian serta melakukan komplain.

**Solusi desain awal:** FoodGo dapat menggunakan Idempotency Key, yaitu token unik yang dibuat untuk setiap transaksi. Ketika request pembayaran dikirim lagi dengan token yang sama, sistem akan mengenali bahwa transaksi tersebut sudah pernah diproses. Sistem cukup mengembalikan hasil transaksi sebelumnya tanpa memproses pembayaran untuk kedua kalinya.

**Trade-off:** Penggunaan Idempotency Key membutuhkan penyimpanan tambahan untuk menyimpan token dan status transaksi. Data tersebut juga perlu memiliki waktu kedaluwarsa (TTL) agar penyimpanan tidak terus bertambah dan memenuhi memori.

---

## Pitfall 3: [nama pitfall] — ditulis oleh [nama]


---

## Kesimpulan Kelompok

[Ringkasan: jika FoodGo memperbaiki ketiga pitfall ini, apa arsitektur yang disarankan secara garis besar? Kaitkan dengan Tugas 2.]
