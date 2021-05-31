1. install nginx
```
sudo apt install nginx

```
2. install php 
```
sudo apt-get install php7.4 libapache2-mod-php7.4 php7.4-curl php-pear php7.4-gd php7.4-dev php7.4-zip php7.4-mbstring php7.4-mysql php7.4-xml curl -y
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
