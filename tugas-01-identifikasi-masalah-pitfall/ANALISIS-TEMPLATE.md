<!-- # Tugas 1 — Analisis Pitfall FoodGo

**Kelompok:** Kelompok 13

| Nama | NIM | Kontribusi |
|---|---|---|
| steven indramer | 103072400070 | Latencty is Zero |
| delon nichollas hermawan | [nim] |  

## Pitfall 1: [nama pitfall] — ditulis oleh [nama]

**Bukti di skenario:** [kutip/paraphrase bagian skenario]

**Kenapa ini keliru:** [penjelasan]

**Dampak ke FoodGo:** [mekanisme kegagalan konkret]

**Solusi desain awal:** [usulan solusi]

**Trade-off:** [apa yang dikorbankan/risiko dari solusi ini]

---

## Pitfall 2: Latencty is Zero — ditulis oleh Steven Indramer

- **Bukti di skenario:** "Aplikasi jadi sangat lambat, beberapa permintaan timeout modul pesanan memanggil modul pembayaran dan menunggu tanpa batas waktu."

- **Kenapa ini keliru:** Proses pemanggilan fungsi dalam memori lokal (in-memory function call) membutuhkan waktu dalam hitungan nanodetik, sedangkan pemanggilan layanan melalui jaringan memerlukan waktu milidetik (bahkan detik jika terdistribusi antar wilayah/cloud). Mengabaikan akumulasi waktu transit data di jaringan memicu desain sistem yang mengandalkan pemanggilan synchronous berantai (tight coupling).

- **Dampak ke FoodGo:** Pengembang menulis alur bisnis secara synchronous yang berasumsi balasan dari modul pembayaran dan notifikasi kurir akan langsung diterima seketika. Pada lonjakan trafik, latensi jaringan meningkat tajam. Karena modul pesanan menunggu balasan dari modul pembayaran secara sinkron sebelum bisa membalas aplikasi pengguna, waktu respons (latency) ujung-ke-ujung bagi pengguna terakumulasi menjadi sangat tinggi hingga melampaui batas timeout di sisi aplikasi seluler/klien.

- **Solusi desain awal:** 
- Mengubah komunikasi berantai dari synchronous menjadi Asynchronous Event-Driven Architecture menggunakan Message Broker (seperti RabbitMQ atau Apache Kafka).
- Ketika pesanan dibuat, modul pesanan menerbitkan event OrderCreated ke broker data dan langsung mengembalikan respons 202 Accepted ke aplikasi pengirim, lalu pemrosesan pembayaran serta notifikasi kurir dilakukan secara asynchronous di latar belakang (background job).
- **Trade-off:** Arsitektur asynchronous meningkatkan kompleksitas sistem secara keseluruhan dan mengorbankan konsistensi seketika (immediate consistency) menjadi eventual consistency. Tim engineering perlu menangani skenario batas (edge cases), seperti penanganan konsistensi state jika pembayaran ternyata gagal setelah pesanan dikonfirmasi sementara ke pengguna.
---

## Pitfall 3: [nama pitfall] — ditulis oleh [nama]

(ulangi struktur di atas)

---

## Kesimpulan Kelompok

[Ringkasan: jika FoodGo memperbaiki ketiga pitfall ini, apa arsitektur yang disarankan secara garis besar? Kaitkan dengan Tugas 2.] -->
