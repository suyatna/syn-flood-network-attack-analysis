# SYN flood network attack analysis

## 📑 Table of contents

1. [Introduction](#introduction)
2. [Incident scenario](#scenario)
3. [Objective](#objective)
4. [Analyze network attack](#report)
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

## 🎯 Objective <a name="objective">

Studi kasus ini disusun untuk memahami dan menganalisis gangguan jaringan yang berdampak langsung pada akses website. Fokus utama berada pada upaya menelusuri sumber masalah melalui pengamatan pola lalu lintas jaringan selama insiden berlangsung.

Tujuan analisis meliputi:
- Mengidentifikasi jenis serangan jaringan berdasarkan pola trafik TCP dan HTTP yang terpantau
- Menganalisis karakteristik lalu lintas jaringan yang tidak normal serta dampaknya terhadap kinerja server web
- Menjelaskan bagaimana serangan tersebut memicu gangguan akses website hingga terjadi koneksi timeout
- Memahami dampak serangan jaringan terhadap operasional organisasi dan aktivitas karyawan
- Menyusun dasar analisis untuk menentukan langkah penanganan dan pencegahan insiden di masa mendatang

---

## 📋 Analyze network attack <a name="report">

|Section 1: Identify the type of attack|
|---|
|Hasil analisis lalu lintas jaringan dari log Wireshark menunjukkan bahwa gangguan akses website yang ditandai dengan pesan connection timeout mengarah pada serangan Denial of Service (DoS). Pola trafik memperlihatkan server web menerima lonjakan permintaan TCP SYN dalam jumlah tidak normal dari alamat IP yang tidak dikenal. Permintaan SYN tersebut dikirim secara terus-menerus dalam waktu singkat hingga server tidak lagi mampu menangani koneksi baru. Pola ini sesuai dengan karakteristik serangan SYN Flood, salah satu jenis DoS attack yang menargetkan proses awal pembentukan koneksi TCP pada server.|

| Section 2: Explain how the attack causes the website malfunction|
|---|
|Pada kondisi normal, koneksi ke server web dimulai melalui proses TCP three-way handshake. Proses ini melibatkan pengiriman paket SYN dari client, balasan SYN-ACK dari server, dan ACK terakhir untuk menyelesaikan koneksi. Pada serangan SYN Flood, pelaku membanjiri server dengan paket SYN tanpa pernah menyelesaikan proses handshake. Server tetap merespons setiap permintaan dan mengalokasikan resource untuk koneksi yang tidak pernah selesai. Kondisi ini membuat resource server cepat habis dan tidak tersedia bagi pengguna yang sah. Log Wireshark menunjukkan server web kewalahan menghadapi tingginya jumlah permintaan SYN dan gagal merespons koneksi normal. Situasi ini menyebabkan koneksi tidak pernah terbentuk dengan sempurna, sehingga pengunjung website dan karyawan perusahaan mengalami connection timeout saat mencoba mengakses situs.|

---

## 🏁 Conclusion <a name="conclusion">

Hasil analisis lalu lintas jaringan menunjukkan bahwa gangguan akses website dipicu oleh serangan Denial of Service dengan metode SYN Flood. Server web dibanjiri permintaan SYN dalam jumlah besar hingga proses pembentukan koneksi TCP terganggu. Kondisi ini membuat resource server cepat habis dan tidak mampu melayani koneksi dari pengguna yang sah.

Insiden ini memperlihatkan bahwa lonjakan trafik yang tidak wajar dapat langsung memengaruhi ketersediaan layanan web dan aktivitas kerja karyawan. Pemahaman terhadap pola serangan melalui analisis trafik jaringan membantu mengidentifikasi jenis serangan beserta dampaknya. Temuan ini menjadi dasar bagi organisasi untuk memperkuat keamanan jaringan dan mencegah kejadian serupa di kemudian hari.

