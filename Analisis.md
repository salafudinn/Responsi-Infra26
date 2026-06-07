# demonstrasi aplikasi

<img width="1280" height="720" alt="2026-06-07 12-17-39" src="https://github.com/user-attachments/assets/0748eebd-b976-4f85-a3e2-37f94620d590" />

# Analisis Perbaikan

## Permasalahan 1

### Gejala

Muncul pesan error yaml: line 4, column 8: mapping values are not allowed in this context pas jalanin docker compose.

### Penyebab

services pada file docker-compose.yml kuraang titik 2 (:) menyebabkan configurasi docker-compose.yml tidaak berfungsi semua karena tidak mendeteksi configurasi dengan baik

### Solusi

nambahin titik 2 pda services jdi services: dn juga merapihkan bris kode ny

---

## Permasalahan 2

### Gejala

pas membuat kontainer gagal tulisanya gini
error unable to prepare context: path "/home/salaf/Responsi-Infra26/web33" not found.

### Penyebab

typo lagi pada context milik service web3 di file docker-compose.yml, yaitu /web33 yg harusny ./web3. itu yg mentebaabkan path yg sudah di konfigurasikan error dan tidak terbaca

### Solusi

ubah aja context milik service web3 yg awalny /web33 jadi ./web3

---

## Permasalahan 3

### Gejala

build gagal dan terjadi error pesannya
eror failed to resolve source metadata for docker.io/library/php:8.2-apach: not found.

### Penyebab

typo lagi di nama base image resmi pada baris pertama di dalam berkas Dockerfile milik folder web1, web2, dan web3, yg isiny
FROM php:8.2-apach typo kurang huruf e. alhasil tidak mendeteksi apache nya

### Solusi

ubah baris pertama di ketiga file Dockerfile tersebut biar g typo jadi php:8.2-apache.

---

## Permasalahan 4

### Gejala

kontainer berhasil di jalanin tapi pas mau uji tampiln menggunakan cmd curl http://localhost:8080 selalu membalas dengan error Connection refused.

### Penyebab
ada titik tiga ini ``` yg berada di atas sama paling bawah di file nginx.conf sama typo web11:80, web3:8080 menyebabkan koneksi tidak diterima

### Solusi

hapus aja titik 3 (```) nginx di atas sama bawah

sama jgn lupa benerin typo di file nginx.conf jadi kayak gini 
upstream backend {
    server web1:80;
    server web2:80;
    server web3:80;
}


---

## Permasalahan 5

### Gejala

Aplikasi web server cluster ga bisa terhubung ke database MySQL ga kebagian beban traffic dari Load Balancer Nginx secara merata.

### Penyebab

nemu typo lagi di konfigurasi variabel enviroment, jaringan, dan volume pada file docker-compose.yml:

1. DB_HOST milik web1 typo mengarah ke nama mysql seharusnya db.
2. DB_PASS milik web2 typo password database menjadi wrongpassword.
3. Service web3 kekurangan network - frontend sehingga tidak bisa dijangkau oleh Nginx.
4. volume global di baris paling bawah database-data gak sinkron dengan volume yang dipanggil oleh service db db-data karena typo.

### Solusi

Melakukan koreksi pada file docker-compose.yml dengan ubah DB_HOST menjadi db, 
ubah password web2 menjadi student123, 
nambahin network - frontend pada web3, 
lalu menyamakan teks volume global paling bawah menjadi 
volumes:
  db-data:
