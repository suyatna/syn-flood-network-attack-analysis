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

Saya bekerja sebagai analis keamanan siber di sebuah agen perjalanan yang mengandalkan website perusahaan untuk menampilkan promosi dan paket liburan. Website ini digunakan setiap hari oleh karyawan untuk membantu pelanggan memilih layanan. Suatu sore, sistem monitoring mendeteksi gangguan pada server web. Website tidak dapat diakses dan browser menampilkan pesan connection timeout. Kondisi ini mulai menghambat aktivitas operasional.

Peninjauan lalu lintas jaringan menunjukkan lonjakan permintaan TCP SYN dalam jumlah tidak wajar dari alamat IP eksternal yang tidak dikenal. Server web kewalahan memproses permintaan tersebut hingga gagal merespons koneksi normal. Server web dimatikan sementara untuk menstabilkan sistem, lalu firewall dikonfigurasi untuk memblokir sumber permintaan berlebih. Langkah ini bersifat sementara dan segera dilaporkan kepada manajer untuk menentukan penanganan lanjutan.

Berikut adalah log TCP dan HTTP yang direkam menggunakan Wireshark:

<img width="824" height="1890" alt="image" src="https://github.com/user-attachments/assets/65f6341f-486d-41c5-ab11-8640dd2aac93" />

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

### Bagian 1: Identifikasi jenis serangan

Hasil pengamatan lalu lintas jaringan menunjukkan bahwa gangguan akses website yang ditandai dengan pesan connection timeout mengarah pada serangan Denial of Service (DoS). Trafik memperlihatkan lonjakan permintaan TCP SYN dalam jumlah besar yang datang dari alamat IP tidak dikenal dalam waktu singkat.

Permintaan koneksi tersebut tidak pernah diselesaikan melalui proses TCP secara normal. Server web terus menerima paket SYN tanpa adanya penyelesaian koneksi hingga tahap akhir, sehingga resource server terkuras. Pola ini sesuai dengan karakteristik serangan SYN Flood, di mana server dibanjiri permintaan koneksi palsu untuk mengganggu ketersediaan layanan.

### Bagian 2: Cara serangan menyebabkan gangguan website

Akses ke server web pada kondisi normal selalu diawali dengan proses TCP three-way handshake. Client mengirim paket SYN sebagai permintaan koneksi. Server membalas dengan SYN-ACK sebagai tanda persetujuan dan menyiapkan resource. Client kemudian mengirim ACK untuk menyelesaikan koneksi.
- Serangan SYN flood terjadi saat penyerang mengirim paket SYN dalam jumlah besar tanpa melanjutkan proses handshake hingga selesai.
- Server tetap merespons setiap permintaan tersebut dengan menyediakan resource, meskipun koneksi tidak pernah terbentuk sepenuhnya.
- Kapasitas server pun cepat habis dan tidak mampu melayani koneksi baru.

Log jaringan menunjukkan server web akhirnya gagal merespons permintaan dari pengguna yang sah. Koneksi tidak pernah terbentuk dengan baik dan pengguna menerima pesan connection timeout saat mengakses website. Kondisi ini berdampak langsung pada ketersediaan layanan dan menghambat aktivitas operasional yang bergantung pada website.

---

## 🏁 Conclusion <a name="conclusion">

Hasil analisis lalu lintas jaringan menunjukkan bahwa gangguan akses website disebabkan oleh serangan TCP SYN flood yang termasuk dalam kategori Denial of Service (DoS). Serangan ini terlihat dari lonjakan permintaan TCP SYN dalam jumlah tidak wajar yang datang dari alamat IP tidak dikenal, sehingga server web kewalahan dan gagal menyelesaikan proses koneksi dengan pengguna yang sah.

Dampak serangan ini langsung terasa pada ketersediaan layanan website. Sumber daya server terkuras untuk menangani permintaan koneksi palsu, membuat koneksi baru tidak pernah terbentuk dengan sempurna. Kondisi ini memicu pesan connection timeout yang dialami karyawan dan pelanggan, serta mengganggu aktivitas bisnis yang bergantung pada akses website.

Studi kasus ini menegaskan pentingnya pemantauan lalu lintas jaringan dan kemampuan mengenali pola serangan sejak awal. Pemahaman terhadap karakteristik SYN flood dan dampaknya membantu organisasi mengambil langkah pencegahan yang lebih tepat, seperti penerapan mekanisme mitigasi serangan dan penguatan kontrol keamanan jaringan, agar risiko gangguan serupa dapat ditekan di kemudian hari.
