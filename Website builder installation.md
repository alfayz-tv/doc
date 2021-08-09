Website Builder Installation

1. Install Nginx
```
sudo apt update
sudo apt install nginx


# note:
sudo systemctl stop nginx
sudo systemctl start nginx
sudo systemctl restart nginx
sudo systemctl reload nginx
sudo systemctl disable nginx
sudo systemctl enable nginx
```

2. Menyesuaikan Firewall

Sebelum menguji Nginx, perangkat lunak firewall perlu disesuaikan untuk mengizinkan akses ke layanan. Nginx mendaftarkan dirinya sendiri sebagai sebagai layanan dengan ufw pada saat instalasi, yang mempermudah untuk mengizinkan akses Nginx.

Buat daftar konfigurasi aplikasi yang mana ufw mengetahui cara bekerja sama dengannya dengan mengetik:
```
sudo ufw app list
```
Anda akan mendapat daftar profil aplikasi:
```
Output
Available applications:
  Nginx Full
  Nginx HTTP
  Nginx HTTPS
  OpenSSH
```
Seperti yang ditunjukkan oleh keluaran, ada tiga profil yang tersedia untuk Nginx:

- Nginx Full: Profil ini membuka baik porta 80 (lalu lintas web normal dan tidak terenkripsi) dan porta 443 (lalu lintas terenkripsi TLS/SSL)
- Nginx HTTP: Profil ini hanya membuka porta 80 (lalu lintas web normal dan tidak terenkripsi)
- Nginx HTTPS: Profil ini hanya membuka porta 443 (lalu lintas terenkripsi TLS/SSL)

Anda disarankan untuk mengaktifkan profil yang paling ketat yang masih akan mengizinkan lalu lintas yang telah Anda konfigurasikan. Saat ini, kita hanya perlu mengizinkan lalu lintas pada porta 80.

Anda dapat mengaktifkan ini dengan mengetik:
```
sudo ufw allow 'Nginx HTTP'
```
Anda dapat memverifikasi perubahan dengan mengetik:
```
sudo ufw status
```
Keluaran akan mengindikasikan lalu lintas HTTP mana yang diizinkan:
```
Output
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere                  
Nginx HTTP                 ALLOW       Anywhere                  
OpenSSH (v6)               ALLOW       Anywhere (v6)             
Nginx HTTP (v6)            ALLOW       Anywhere (v6)
```

3. Memeriksa Server Web Anda

Pada akhir proses instalasi, Ubuntu 20.04 memulai Nginx. Server web seharusnya sudah aktif dan berjalan.

Kita dapat memeriksa dengan sistem init systemd untuk memastikan layanan sedang berjalan dengan mengetik:
```
systemctl status nginx
```
Output
```
● nginx.service - A high performance web server and a reverse proxy server
   Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
   Active: active (running) since Fri 2020-04-20 16:08:19 UTC; 3 days ago
     Docs: man:nginx(8)
 Main PID: 2369 (nginx)
    Tasks: 2 (limit: 1153)
   Memory: 3.5M
   CGroup: /system.slice/nginx.service
           ├─2369 nginx: master process /usr/sbin/nginx -g daemon on; master_process on;
           └─2380 nginx: worker process
```
Sebagaimana dikonfirmasi oleh keluaran ini, layanan telah berhasil dimulai. Namun, cara terbaik untuk menguji ini adalah dengan benar-benar meminta suatu laman dari Nginx.

Anda dapat mengakses laman landas Nginx asali untuk mengonfirmasi bahwa perangkat lunak berjalan dengan baik dengan bernavigasi ke alamat IP server Anda. Jika Anda tidak mengetahui alamat IP server Anda, Anda dapat menemukannya dengan menggunakan alat icanhazip.com, yang akan memberi Anda alamat IP publik Anda sebagaimana diterima dari lokasi lain di internet:
```
curl -4 yourdomain.com
 ```
Saat Anda memiliki alamat IP server Anda, masukkan alamat itu ke dalam bilah alamat peramban Anda:
```
http://your_server_ip
```
Anda akan menerima laman landas Nginx asali:

Jika Anda berada di laman ini, server Anda berjalan dengan benar dan siap untuk dikelola.


4. Install Php
```
#php
sudo apt install -y php-mbstring php-xml php-fpm php-zip php-common php-fpm php-cli unzip

#php curl
sudo apt-get install php7.4-curl
sudo apt-get install php7.4-mysql

#composer
sudo curl -s https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer

```
5. Install Redis
```
# install redis
sudo apt install redis-server

# run redis
sudo service redis-server start

# if you want edit config redis
sudo nano /etc/redis/redis.conf

# restart redis
sudo systemctl restart redis.service

# test redis
redis-cli

# look service
sudo netstat -lnp | grep redis
```

6. install node js and npm
```
# node js
sudo apt install nodejs

# npm
sudo apt install npm
```

7. install project 
```
#clone project from your repo
mkdir var/www/html
cd var/www/html
git clone your_repo_here
cd project_name
composer diagnose
composer install
cp .env.example .env
php artisan key:generate

# install node dependency
npm install


# give access
sudo chmod -R 755 /var/www/html/your_project

# testing runing
php artisan serve --host=your_ip --port=8000

```
8. install mariaDB /Mysql
```
sudo apt install -y mariadb-client mariadb-server
sudo systemctl enable --now mariadb.service
sudo mysql_secure_installation

#config for your DB
#change root password
ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';
#create DB
CREATE DATABASE IF NOT EXISTS test;

#show databases;
show databases;

#create user
CREATE USER 'user1'@localhost IDENTIFIED BY 'password1';

#show all user
SELECT User FROM mysql.user;

#give grant all privileges
GRANT ALL PRIVILEGES ON *.* TO 'user1'@localhost IDENTIFIED BY 'password1';

#give grant privileges only for yourDB
GRANT ALL PRIVILEGES ON 'yourDB'.* TO 'user1'@localhost;

# commit config
FLUSH PRIVILEGES;
```
```
#show privilleges
SHOW GRANTS FOR 'user1'@localhost;

#configure mysql can access from public IP
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf

#change bind address
bind-address = 0.0.0.0

#check network mysql now not working in 127.0.0.1 
netstat -ant | grep 3306

# log in to mysql
mysql -u root -p

#make privileges
GRANT ALL ON nama_datbase.* to 'nama_user'@'%' IDENTIFIED BY 'password' WITH GRANT OPTION;

FLUSH PRIVILEGES;
exit;
#configure firewall
ufw allow 3306
ufw reload
```

9. install powerdns
```
https://computingforgeeks.com/install-powerdns-and-powerdns-admin-on-ubuntu-debian/

```

10. migrate and seed
```
php artisan migrate
php artisan db:seed
php artisan passport:install
```

11. give access
```
sudo chgrp -R www-data /var/www/html/example/

sudo chown -R www-data:www-data /nama_folder
sudo chmod -R 775 /var/www/html/example/storage


/home/networksupport/var/www/html/Website-builder-V2

```

12. change config .env
```
 php artisan config:cache
 php artisan config:clear

```
