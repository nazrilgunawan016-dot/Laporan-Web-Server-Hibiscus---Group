# Laporan-Web-Server-Hibiscus---Group
Dibuat untuk memenuhi tugas KKTKJ (Pak Asep) mengenai materi Web Server berbasis Nginx.
LAPORAN PROYEK WEB SERVER NGINX – KELOMPOK HIBISCUS
1. Akses Server
A. Akses Menggunakan SSH Client
a) CMD (SSH)

Untuk masuk ke server, gunakan:

ssh kahiang@<ip-server>


Catatan:

Tidak bisa login sebagai root.

Gunakan sudo untuk tindakan yang membutuhkan akses root, misalnya:

sudo apt install php
sudo nano /var/www/html/index.php

b) WinSCP

Karena tidak dapat login sebagai root, akun kahiang perlu diset agar tetap bisa mengakses file root saat transfer.

Masukkan:

Host Name: Hibicus

User: kahiang

Password: @kahiang123

Lalu:
Advanced → Environment → SFTP
Isi SFTP Server dengan:

sudo /usr/lib/openssh/sftp-server


Klik OK, lalu Login.

c) CMD (Akses Biasa)

Sama seperti SSH biasa:

ssh kahiang@ipserver


Setiap perintah penting tetap menggunakan sudo.

B. WinSCP untuk Edit & Transfer File

WinSCP memudahkan kami meng-upload, memindahkan, atau mengedit file tanpa harus mengetik banyak perintah di terminal yang kadang bikin pusing 😅.

C. Instalasi & Koneksi Web Server

Pastikan Debian sudah punya IP dan bisa diakses.

Repository sudah aktif.

Tes koneksi server dari LAN.

2. Konfigurasi Dasar Nginx

Update package:

apt update && apt upgrade


Instal Nginx:

apt install nginx


Jalankan dan aktifkan:

systemctl start nginx
systemctl enable nginx


Cek status:

systemctl status nginx


Buka http://ip-server
Jika muncul Welcome to Nginx!, berarti server berhasil berjalan.

3. Instal & Uji PHP pada Nginx
A. Instalasi PHP-FPM
apt install php8.4-fpm php8.4-cli


Cek status:

systemctl status php8.4-fpm

B. Konfigurasi Nginx untuk PHP

Backup file bawaan:

mv /etc/nginx/sites-available/default /etc/nginx/sites-available/default.asli


Edit file baru:

nano /etc/nginx/sites-available/default


Isi konfigurasi seperti contoh (tetap versi lengkap, hanya dirapikan agar mudah dibaca).

Setelah itu:

nginx -t
systemctl restart nginx

C. Uji PHP

Buat file uji:

nano /var/www/html/info.php


Isi:

<?php phpinfo(); ?>


Buka browser:
http://ip-server/info.php

Jika informasi PHP muncul, berarti Nginx dan PHP sudah terhubung dengan baik.

4. Menambahkan SSL Self-Signed
A. Membuat Sertifikat
mkdir /etc/ssl/nginx
apt install openssl
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
-keyout /etc/ssl/nginx/selfsigned.key \
-out /etc/ssl/nginx/selfsigned.crt

B. Tambahkan SSL ke Konfigurasi Nginx

Edit kembali:

nano /etc/nginx/sites-available/default


Tambahkan konfigurasi untuk HTTPS (port 443).

Cek & restart:

nginx -t
systemctl restart nginx


Akses:
https://ip-server
 (103.59.94.221)
Jika muncul “Not Secure”, klik Advanced → Proceed.
Buat file index.php sebagai tampilan awal website.

5. Kelebihan & Kekurangan Nginx

Kelebihan:

🔹 Performa tinggi tapi ringan, cocok untuk koneksi banyak.

🔹 Sangat stabil (jarang perlu restart).

🔹 Cepat untuk file statis seperti HTML/CSS.

🔹 Hemat resource CPU & RAM.

Kekurangan:

🔸 Modul tidak bisa ditambah secara dinamis.

🔸 Kurva belajar cukup tinggi untuk pemula (kami merasakan betul ini 😅).

🔸 Fitur agak terbatas untuk aplikasi lama.

6. Perjalanan Project Website
A. Pembuatan Project Kelompok

Awalnya kami berdiskusi mencari tema. Setelah beberapa ide, kami memilih tema warna #006994, nuansa formal, dan sedikit animasi supaya tampilan tetap fresh tapi tetap resmi ✨.

Kami dibantu AI untuk ide desain, tapi pengetikan dan penyusunan syntax tetap kami lakukan sendiri.
Kesulitannya muncul saat mencoba mengedit file melalui WinSCP karena tidak langsung tersimpan—kami belum memahami cara kerjanya waktu itu.

Akhirnya kami pindah ke CMD dulu, sebelum akhirnya paham WinSCP berkat penjelasan teman lintas kelompok (makasih bangett 🫶).
Setelah semua proses itu, tampilan index akhirnya berhasil dibuat sesuai rencana—yeay! 🎉

B. Upload Project ke Server

Pemindahan file dilakukan via WinSCP agar lebih mudah. Ada tantangan kecil, tapi bisa diatasi.

C. Pembuatan Project Individu

Alya Destiani – 5

Nasya Adinda Rahayu – 21
Awalnya sempat bingung tentang konsep file pribadi tiap anggota agar bisa dipanggil di web. Untungnya, teman lintas kelompok menjelaskan dengan rinci sehingga kami langsung “ngeh”.

Nazril Gunawan – 22

Putri Marlina –

Setiap anggota kemudian membuat halaman dengan tema yang sama agar terlihat selaras (begitchuu 😆).

7. Kendala & Solusi yang Kami Alami
a. Kesulitan Memahami Perintah

Pada awalnya, beberapa perintah di tutorial LMS terasa asing, sehingga kami agak kebingungan.
Solusi: bertanya ke AI dan diskusi bareng teman sampai paham sedikit demi sedikit.

b. Bingung Menggunakan WinSCP

Kami sempat kesulitan saat mencoba edit file lewat WinSCP karena tidak langsung tersimpan.
Solusi: setelah dijelaskan oleh teman lintas kelompok, kami akhirnya paham fungsi WinSCP dan jadi lebih mudah.

c. Website Tidak Muncul Setelah Dipindah

Ternyata Apache masih aktif sehingga bentrok dengan Nginx.
Solusi: menonaktifkan Apache dan menjalankan kembali Nginx.

d. File Website Individu Tidak Terpanggil

Hal ini terjadi karena ada dua file dalam satu folder pribadi. Server bingung memanggil yang mana.
Solusi: menghapus file yang tidak dipakai.
Akhirnya bisa jalan berkat bantuan teman lintas kelompok (lagi?). ❤️
