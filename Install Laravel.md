1. install nginx
```
sudo apt install nginx

```
2. install php 
```
sudo apt-get install php7.4 libapache2-mod-php7.4 php7.4-curl php-pear php7.4-gd php7.4-dev php7.4-zip php7.4-mbstring php7.4-mysql php7.4-xml curl -y

sudo apt install php7.4-zip php7.4-common php7.4-fpm php7.4-cli unzip
```

3. install composer
```
sudo curl -s https://getcomposer.org/installer | php

sudo mv composer.phar /usr/local/bin/composer
```

4. install mariaDB
```
sudo apt install -y mariadb-client mariadb-server
sudo systemctl enable --now mariadb.service
sudo mysql_secure_installation

```
5. install project
6. install node js
```
# node js
sudo apt install nodejs

# npm
sudo apt install npm

```

7. Setting virtual host
```
#add host in /etc/hosts
nano /etc/hosts
#add domain and ip
127.0.0.1 example.test

```
```
# add user premision
chmod -R 777 /project_dir

#add nginx permision
sudo chown -R www-data:www-data /project_dir
```
```
# add conf 
sudo nano /etc/nginx/sites-available/example.test
```
```
# Enable the Nginx configuration.

$ sudo ln -s /etc/nginx/sites-available/example.test /etc/nginx/sites-enabled/
```
```
# Remove the default configuration file.

$ sudo rm /etc/nginx/sites-enabled/default

#Restart Nginx.

$ sudo systemctl restart nginx
```
Test that your Laravel application loads properly in a web browser.

http://example.test/

```
Note: 
server {
    listen 80;
    server_name example.com;
    root /var/www/html/example/public;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-XSS-Protection "1; mode=block";
    add_header X-Content-Type-Options "nosniff";

    index index.html index.htm index.php;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    error_page 404 /index.php;

    location ~ \.php$ {
        fastcgi_pass unix:/run/php/php7.4-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```
>Note:
>> Agar pas pemberian akses tidak merubah git
lakukan:
```
git config --global core.filemode false

# Remove your local configuration to make the global configuration take effect:

git config --unset core.filemode
```
