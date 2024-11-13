Untuk menginstal Zonemaster CLI, Engine, GUI, dan iDNS pada Ubuntu, Anda perlu mengikuti beberapa langkah untuk memastikan semua komponen diinstal dengan benar. Berikut adalah panduan instalasi lengkap:

### 1. **Perbarui Sistem**
Pastikan sistem Ubuntu Anda sudah diperbarui:

```bash
sudo apt update && sudo apt upgrade
```

### 2. **Instal Prasyarat**
Zonemaster memerlukan beberapa dependensi seperti `perl`, `libtool`, dan lain-lain:

```bash
sudo apt install build-essential libtool libssl-dev libpcap-dev automake autoconf
```

Anda juga memerlukan modul Perl untuk Zonemaster Engine dan CLI. Instal modul-modul Perl berikut:

```bash
sudo apt install libnet-libidn-perl libjson-perl libfile-slurp-perl libreadonly-perl libtest-exception-perl libtest-differences-perl
```

### 3. **Instal Zonemaster Engine**
Unduh dan instal Zonemaster Engine dari GitHub.

```bash
# install on ubuntu
sudo apt update && sudo apt upgrade
curl -LOs https://package.zonemaster.net/setup.sh
sudo sh setup.sh
sudo apt install libzonemaster-engine-perl

```

### 4. **Instal Zonemaster CLI**
Setelah engine terinstal, instal Zonemaster CLI.

- Clone repository Zonemaster CLI:

```bash
git clone https://github.com/zonemaster/zonemaster-cli.git
cd zonemaster-cli
```

- Instal dependensi dan Zonemaster CLI:
## Installation on Debian and Ubuntu

Using pre-built packages is the preferred method for Debian and Ubuntu.

#### Installation from pre-built packages

1) Add Zonemaster packages repository to repository list
   ```sh
   curl -LOs https://package.zonemaster.net/setup.sh
   sudo sh setup.sh
   ```
2) Install Zonemaster CLI
   ```sh
   sudo apt install zonemaster-cli
   ```
3) Update configuration of "locale"

   ```sh
   sudo perl -pi -e 's/^# (da_DK\.UTF-8.*|en_US\.UTF-8.*|es_ES\.UTF-8.*|fi_FI\.UTF-8.*|fr_FR\.UTF-8.*|nb_NO\.UTF-8.*|sv_SE\.UTF-8.*)/$1/' /etc/locale.gen
   sudo locale-gen
   ```

   After the update, `locale -a` should at least list the following locales:
   ```
   da_DK.utf8
   en_US.utf8
   es_ES.utf8
   fi_FI.utf8
   fr_FR.utf8
   nb_NO.utf8
   sv_SE.utf8
   ```


### 5. **Instal Zonemaster iDNS**
Untuk menginstal iDNS, Anda memerlukan prasyarat lain seperti modul DNS. Ikuti langkah berikut:

- Clone repository Zonemaster iDNS:

```bash
git clone https://github.com/zonemaster/zonemaster-idns.git
cd zonemaster-idns
```

- Instal dependensi dan Zonemaster iDNS:

```bash
sudo cpanm --installdeps .
sudo perl Makefile.PL
sudo make
sudo make install
```

### 6. **Instal Zonemaster GUI**
Zonemaster GUI menggunakan framework berbasis web. Anda dapat menginstal Zonemaster GUI dengan mengkloning repositori dan mengikuti langkah-langkah berikut.

- Clone repository Zonemaster GUI:

```bash
git clone https://github.com/zonemaster/zonemaster-gui.git
cd zonemaster-gui
```

- Instal dependensi PHP dan ekstensi lainnya:

```bash
sudo apt install php php-json php-xml php-mbstring
```

- Konfigurasikan web server (misalnya, Apache):

```bash
sudo apt install apache2
```

- Salin file Zonemaster GUI ke direktori root web server (misalnya, `/var/www/html`):

```bash
sudo cp -r * /var/www/html/
```

- Pastikan konfigurasi PHP dan izin file sudah sesuai dengan pengaturan Zonemaster.

### 7. **Verifikasi Instalasi**
- Jalankan perintah berikut untuk memverifikasi instalasi Zonemaster CLI:

```bash
zonemaster-cli --version
```

- Akses Zonemaster GUI melalui browser dengan menuju ke `http://localhost/`.

Setelah mengikuti langkah-langkah ini, Zonemaster CLI, Engine, iDNS, dan GUI harus siap digunakan di sistem Ubuntu Anda.