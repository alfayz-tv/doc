
### Cara Menggunakan `perlbrew` untuk Menjalankan Beberapa Versi Perl

1. **Instal `perlbrew`:**
   Jika belum diinstal, Anda bisa menginstal `perlbrew` seperti yang dijelaskan sebelumnya:
   ```bash
   curl -L https://install.perlbrew.pl | bash
   source ~/perl5/perlbrew/etc/bashrc
   ```

2. **Instal Perl 5.14 dan 5.34:**
   Anda bisa menginstal kedua versi Perl menggunakan `perlbrew`:
   ```bash
   perlbrew install perl-5.14.4
   perlbrew install perl-5.34.0
   ```

3. **Aktifkan Versi Perl yang Diinginkan:**
   Setelah instalasi, Anda dapat beralih antar versi Perl dengan mudah:
   - Untuk menggunakan Perl 5.14:
     ```bash
     perlbrew switch perl-5.14.4
     ```
   - Untuk menggunakan Perl 5.34:
     ```bash
     perlbrew switch perl-5.34.0
     ```

4. **Verifikasi Versi Perl Aktif:**
   Anda bisa memeriksa versi Perl yang sedang aktif dengan menjalankan:
   ```bash
   perl -v
   ```

5. **Menjalankan Skrip dengan Versi Perl Tertentu:**
   Jika Anda hanya perlu menjalankan skrip tertentu dengan versi Perl tertentu tanpa mengubah versi aktif secara global, Anda bisa menggunakan:
   ```bash
   perlbrew exec --with perl-5.14.4 perl your_script.pl
   ```




Dengan `perlbrew`, Anda dapat dengan mudah mengelola dan menjalankan berbagai versi Perl di sistem Anda sesuai kebutuhan.


Untuk menginstal Perl 5.14 beserta modul-modul yang Anda sebutkan di MacBook dengan chip M1, Anda bisa mengikuti langkah-langkah berikut:

### 1. Menginstal Perl 5.14
Versi default Perl yang tersedia di macOS mungkin lebih baru dari 5.14, jadi Anda mungkin perlu menginstal versi lama secara terpisah menggunakan `perlbrew`.

#### Langkah-langkah:
1. **Instal `perlbrew`:**
   ```bash
   curl -L https://install.perlbrew.pl | bash
   ```

2. **Tambahkan `perlbrew` ke PATH:**
   ```bash
   source ~/perl5/perlbrew/etc/bashrc
   ```

3. **Instal Perl 5.14:**
   ```bash
   perlbrew install perl-5.14.4
   ```

4. **Aktifkan Perl 5.14:**
   ```bash
   perlbrew switch perl-5.14.4
   ```

### 2. Menginstal Modul-Modul Perl
Setelah menginstal Perl 5.14, Anda dapat menginstal modul-modul Perl yang dibutuhkan menggunakan `cpanm`.

#### Langkah-langkah:
1. **Instal `cpanm`:**
   ```bash
   perlbrew install-cpanm
   ```

2. **Instal Modul-Modul Perl:**
   - Untuk menginstal setiap modul, jalankan perintah berikut:
     ```bash
     cpanm Config::IniFiles
     cpanm DateTime
     cpanm List::MoreUtils
     cpanm XML::LibXML
     cpanm XML::Simple
     ```

   - `cpanm` akan mengunduh dan menginstal modul-modul ini beserta dependensinya.

### 3. Mengatasi Masalah Dependensi
Jika ada modul yang membutuhkan pustaka sistem seperti `libxml2` untuk `XML::LibXML`, Anda mungkin perlu menginstalnya menggunakan Homebrew.

#### Langkah-langkah:
1. **Instal Homebrew:**
   Jika belum diinstal, Anda bisa menginstalnya dengan:
   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```

2. **Instal `libxml2`:**
   ```bash
   brew install libxml2
   ```

3. **Set Environment untuk `libxml2`:**
   ```bash
   export PKG_CONFIG_PATH="/usr/local/opt/libxml2/lib/pkgconfig"
   export CPATH="/usr/local/opt/libxml2/include/libxml2"
   ```

### 4. Verifikasi Instalasi
Setelah semua modul diinstal, Anda dapat memverifikasi instalasinya dengan mencoba mengimpor modul-modul tersebut di skrip Perl Anda:

```perl
use Config::IniFiles;
use DateTime;
use List::MoreUtils qw(any);
use XML::LibXML;
use XML::Simple;

print "Semua modul berhasil diinstal!\n";
```

Jika tidak ada error, instalasi berhasil.

Jika Anda menghadapi masalah atau membutuhkan bantuan lebih lanjut, silakan bertanya!