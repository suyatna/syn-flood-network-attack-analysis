<img width="137" height="168" alt="image" src="https://github.com/user-attachments/assets/7bcab26a-9b93-4980-9d8a-d993ccbca145" /># SYN flood network attack analysis

## 📑 Table of contents

1. [Introduction](#introduction)
2. [Incident scenario](#scenario)
3. [Objective](#objective)
4. [Cybersecurity incident report](#report)
5. [Next Steps](#nextsteps)
6. [Conclusion](#conclusion)

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

Berikut adalah log TCP dan HTTP yang direkam menggunakan Wireshark digunakan sebagai dasar analisis untuk mengenali pola serangan serta memahami dampaknya terhadap kinerja server web.

|No.|Time|Source|Destination|Protocol|Info|
|---|---|---|---|---|---|
|47|3.144521|198.51.100.23|192.0.2.1|TCP|42584->443 [SYN] Seq=0 Win-5792 Len=120...|
|48|3.195755|198.51.100.24|198.51.100.23|TCP|443->42584 [SYN, ACK] Seq=0 Win-5792 Len=120...|
|49|3.246989|198.51.100.25|192.0.2.1|TCP|42584->443 [ACK] Seq=1 Win-5792 Len=120...|
|50|3.298223|198.51.100.26|192.0.2.1|HTTP|GET  /sales.html HTTP/1.1|
|51|3.349457|198.51.100.27|198.51.100.23|HTTP|HTTP/1.1 200 OK (text/html)|
|52|3.390692|198.51.100.28|192.0.2.1|TCP|54770->443 [SYN] Seq=0 Win=5792 Len=0...|
|53|3.441926|198.51.100.29|203.0.113.0|TCP|443->54770 [SYN, ACK] Seq=0 Win-5792 Len=120...|
|54|3.49316|198.51.100.30|192.0.2.1|TCP|54770->443 [ACK Seq=1 Win=5792 Len=0...|
|...|...|...|...|...|...|
|73|6.230548|192.0.2.1|198.51.100.16|TCP|443->32641 [RST, ACK] Seq=0 Win-5792 Len=120...|



---
