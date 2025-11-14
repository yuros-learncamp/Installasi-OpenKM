# Tutorial Instalasi OpenKM di Linux Mint 22
Tutorial ini memandu Anda melalui proses instalasi OpenKM menggunakan OpenKM Community-Tomcat Bundle di sistem operasi Linux Mint 22.
## 1. Persiapan dan Pembaruan Sistem
Pastikan sistem Anda sudah diperbarui sebelum memulai instalasi. 
```
# Perbarui daftar paket dan paket yang terinstal
sudo apt update
sudo apt upgrade -y
```
## 2. Instalasi Java (OpenJDK 8)
OpenKM versi lama kompatibel dengan Java 8. Kita akan menginstal OpenJDK 8 Headless (yang cukup untuk server).
```
# Instal OpenJDK 8 Headless
sudo apt install openjdk-8-jdk-headless -y
# Verifikasi instalasi Java
java -version
```
Output yang diharapkan harus menunjukkan Java 1.8.0 atau Java 8, misalnya:

`
openjdk version "1.8.0_302"
OpenJDK Runtime Environment (build 1.8.0_302-8u302-b08-0ubuntu1-22.04)
OpenJDK 64-Bit Server VM (build 25.302-b08, mixed mode)
`
## 3. Instalasi MariaDB Database (Jika belum ter-install)

OpenKM memerlukan database eksternal untuk menyimpan data. MariaDB adalah pilihan yang kuat dan sumber terbuka.

### A. Instalasi MariaDB Server
```
sudo apt install mariadb-server -y
```
### B. Konfigurasi MariaDB

Amankan instalasi MariaDB dan buat database serta user untuk OpenKM.

#### 1. Amankan Instalasi (Opsional tapi Direkomendasikan)
```
sudo mysql_secure_installation
```
Ikuti langkah-langkahnya (termasuk mengatur root password atau menggunakan otentikasi unix_socket).

#### 2. Buat Database dan User OpenKM

Masuk ke MariaDB sebagai root:
```
sudo mysql -u root -p
# atau jika menggunakan otentikasi unix_socket:
# sudo mysql -u root
```
Jalankan perintah SQL berikut untuk membuat database dan user:
```
-- Buat database
CREATE DATABASE openkm CHARACTER SET utf8 DEFAULT COLLATE utf8_bin;

-- Buat user 'openkm' dengan password 'lalalaou' (ganti dengan password kuat Anda)
CREATE USER 'openkm'@'localhost' IDENTIFIED BY 'lalalaou';

-- Berikan semua hak akses pada user 'openkm' untuk database 'openkm'
GRANT ALL ON openkm.* TO 'openkm'@'localhost' WITH GRANT OPTION;

-- Terapkan perubahan hak akses
FLUSH PRIVILEGES;

-- Keluar dari MariaDB
EXIT;
```
## 4. Instalasi OpenKM

Kita akan mengunduh dan menginstal OpenKM Community-Tomcat Bundle.

### A. Tambahkan User Khusus OpenKM (Opsional tapi Direkomendasikan)

Membuat user khusus untuk menjalankan OpenKM meningkatkan keamanan sistem.
```
# Buat user sistem 'openkm'
sudo adduser --system --no-create-home --group --disabled-login openkm
```
### B. Unduh OpenKM Bundle

Kita akan mengunduh bundle ke direktori `/opt.`

#### 1. Pindah ke Direktori Instalasi
```
cd /opt
```
#### 2. Unduh Bundle Terbaru (Ganti URL/Versi jika perlu)

Cari URL unduhan terbaru di OpenKM SourceForge page.

Contoh perintah unduhan (gunakan versi terbaru yang Anda temukan):
```
# Contoh untuk versi 6.8.0, ganti URL jika ada versi baru
sudo wget https://downloads.sourceforge.net/project/openkm/OpenKM%20Community%20Edition/6.8.0/openkm-6.8.0-community-tomcat-bundle.zip
```
#### 3. Ekstrak File
```
# Ganti nama file ZIP sesuai dengan yang Anda unduh
sudo unzip openkm-6.8.0-community-tomcat-bundle.zip
```
#### 4. Ubah Kepemilikan

Ubah kepemilikan direktori OpenKM ke user openkm yang baru dibuat.
```
# Ganti nama direktori yang diekstrak jika berbeda
sudo chown -R openkm:openkm /opt/openkm-6.8.0-community-tomcat-bundle/
```

## 5. Menjalankan OpenKM
### A. Jalankan OpenKM

Masuk sebagai user openkm dan jalankan Tomcat server.
```
# Masuk sebagai user 'openkm'
sudo su - openkm
# Pindah ke direktori bin Tomcat (Ganti path sesuai versi Anda)
cd /opt/openkm-6.8.0-community-tomcat-bundle/tomcat/bin
# Jalankan server
./startup.sh
```
### B. Verifikasi dan Akses

Tunggu beberapa saat hingga server selesai booting.

#### 1. Cek Log (Opsional)
```
# Buka terminal baru atau gunakan screen/tmux
tail -f /opt/openkm-6.8.0-community-tomcat-bundle/tomcat/logs/catalina.out
```
Cari pesan yang menunjukkan server sudah startup.

#### 2. Akses OpenKM Buka browser Anda dan kunjungi:
`http://localhost:8080/OpenKM`

* Username Default: okmAdmin
* Password Default: admin

**Selamat, OpenKM telah ter-install!**
