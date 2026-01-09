# SYN flood network attack analysis

## 📑 Table of contents

1. [Introduction](#introduction)
2. [Incident scenario](#scenario)
3. [Objective](#objective)
4. [Cybersecurity incident report](#report)
5. [Conclusion](#conclusion)

---

## 👋 Introduction <a name="introduction">

Studi kasus ini dibuat sebagai bagian dari latihan cybersecurity yang berfokus pada pemahaman gangguan keamanan jaringan pada layanan website. Skenario yang digunakan menggambarkan situasi ketika pelanggan dan karyawan mengalami kesulitan mengakses website akibat aktivitas jaringan yang tidak berjalan normal.

Latihan ini disusun berdasarkan pembelajaran dalam program Google Cybersecurity Professional Certificate, khususnya pada course Connect and Protect: Networks and Network Security. Pembahasan difokuskan pada upaya mengenali jenis serangan jaringan yang mungkin terjadi, memahami dampaknya terhadap kinerja website, serta melihat pengaruhnya terhadap operasional organisasi secara keseluruhan.

---

## 💭 Incident scenario <a name="scenario">

Saya bekerja sebagai analis keamanan siber di sebuah agen perjalanan yang mengandalkan website perusahaan untuk menampilkan promosi dan paket liburan. Website ini digunakan setiap hari oleh tim internal untuk membantu pelanggan memilih layanan yang sesuai dengan kebutuhan mereka.

Suatu sore, sistem monitoring mengirimkan peringatan adanya gangguan pada server web. Saat saya mencoba mengakses website secara langsung, halaman tidak berhasil terbuka dan browser menampilkan pesan connection timeout. Situasi ini mulai menghambat aktivitas karyawan yang sangat bergantung pada website tersebut.

Saya meninjau lalu lintas jaringan yang masuk dan keluar dari server web untuk mencari sumber masalah. Hasil pengamatan menunjukkan lonjakan permintaan TCP SYN dalam jumlah tidak wajar yang berasal dari alamat IP eksternal yang tidak dikenal. Server web terlihat kewalahan memproses permintaan tersebut hingga tidak mampu merespons koneksi secara normal.

Langkah awal yang diambil adalah mematikan server web sementara agar sistem kembali stabil. Firewall kemudian disesuaikan untuk memblokir alamat IP yang mengirimkan permintaan SYN secara berlebihan. Pendekatan ini disadari bersifat sementara karena penyerang dapat mengganti atau memalsukan alamat IP. Temuan ini segera disampaikan kepada manajer untuk menentukan langkah lanjutan dalam menghentikan serangan dan mencegah kejadian serupa di masa mendatang.

Berikut adalah log TCP dan HTTP yang direkam menggunakan Wireshark:

<img width="1077" height="2751" alt="image" src="https://github.com/user-attachments/assets/ea23fe59-4463-4385-8a61-1c57eadc53d6" />

---

## 🎯 Objective <a name="objective">

Studi kasus ini disusun untuk memahami dan menganalisis gangguan jaringan yang berdampak langsung pada akses website. Fokus utama berada pada upaya menelusuri sumber masalah melalui pengamatan pola lalu lintas jaringan selama insiden berlangsung.

Tujuan analisis meliputi:
- Mengidentifikasi jenis serangan jaringan berdasarkan pola trafik TCP dan HTTP yang terpantau
- Menganalisis karakteristik lalu lintas jaringan yang tidak normal serta dampaknya terhadap kinerja server web
- Menjelaskan bagaimana serangan tersebut memicu gangguan akses website hingga terjadi koneksi timeout
- Memahami dampak serangan jaringan terhadap operasional organisasi dan aktivitas karyawan
- Menyusun dasar analisis untuk menentukan langkah penanganan dan pencegahan insiden di masa mendatang

---

## 📋 Cybersecurity incident report <a name="report">

### Analyze network attack

|Bagian 1: Identifikasi jenis serangan|
|---|
|Hasil pengamatan lalu lintas jaringan menunjukkan bahwa gangguan akses website yang ditandai dengan pesan connection timeout mengarah pada serangan Denial of Service (DoS). Trafik memperlihatkan lonjakan permintaan TCP SYN dalam jumlah besar yang datang dari alamat IP tidak dikenal dalam waktu singkat. Permintaan koneksi tersebut tidak pernah diselesaikan melalui proses TCP secara normal. Server web terus menerima paket SYN tanpa adanya penyelesaian koneksi hingga tahap akhir, sehingga resource server terkuras. Pola ini sesuai dengan karakteristik serangan SYN Flood, di mana server dibanjiri permintaan koneksi palsu untuk mengganggu ketersediaan layanan.

|Bagian 2: Cara serangan menyebabkan gangguan website|
|---|
|Akses ke server web pada kondisi normal selalu diawali dengan proses TCP three-way handshake. Client mengirim paket SYN sebagai permintaan koneksi. Server membalas dengan SYN-ACK sebagai tanda persetujuan dan menyiapkan resource. Client kemudian mengirim ACK untuk menyelesaikan koneksi. Serangan SYN Flood terjadi saat penyerang mengirim paket SYN dalam jumlah besar tanpa melanjutkan proses handshake hingga selesai. Server tetap merespons setiap permintaan tersebut dengan menyediakan resource, meskipun koneksi tidak pernah terbentuk sepenuhnya. Kapasitas server pun cepat habis dan tidak mampu melayani koneksi baru. Log jaringan menunjukkan server web akhirnya gagal merespons permintaan dari pengguna yang sah. Koneksi tidak pernah terbentuk dengan baik dan pengguna menerima pesan connection timeout saat mengakses website. Kondisi ini berdampak langsung pada ketersediaan layanan dan menghambat aktivitas operasional yang bergantung pada website.|

---

## 🏁 Conclusion <a name="conclusion">

Hasil analisis lalu lintas jaringan menunjukkan bahwa gangguan akses website dipicu oleh serangan SYN Flood yang termasuk dalam kategori Denial of Service (DoS). Serangan ini terlihat dari lonjakan permintaan TCP SYN dalam jumlah tidak wajar yang datang dari alamat IP tidak dikenal, hingga server web kewalahan dan tidak mampu membangun koneksi dengan pengguna yang sah.

Dampaknya terasa langsung pada ketersediaan layanan website. Resource server terkuras untuk menangani permintaan koneksi palsu, sehingga koneksi yang sah tidak pernah terbentuk dan berakhir dengan pesan connection timeout. Kondisi ini menghambat aktivitas karyawan dan pelanggan yang bergantung pada website, sekaligus menurunkan keandalan layanan perusahaan.

Studi kasus ini menegaskan pentingnya pemantauan lalu lintas jaringan dan kemampuan mengenali pola serangan sejak awal. Pemahaman terhadap karakteristik serangan SYN Flood membantu organisasi menyiapkan langkah pencegahan yang lebih tepat, seperti mitigasi serangan dan penguatan kontrol keamanan jaringan, agar risiko gangguan serupa dapat ditekan di masa mendatang.

