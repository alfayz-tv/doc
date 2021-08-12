1. Look Php on your system
```
#look php install in your system
sudo apt show php
#or 
sudo apt show php -a

```

2. Intsall php
```
# add repo
sudo apt install software-properties-common
sudo add-apt-repository ppa:ondrej/php
sudo apt update

```
```
# for nginx server
sudo apt install php5.6-fpm
sudo apt install php7.0-fpm
sudo apt install php7.1-fpm
sudo apt install php7.2-fpm
sudo apt install php7.3-fpm
sudo apt install php7.4-fpm
sudo apt install php8.0-fpm
```

```
# for apache server
sudo apt install php5.6 
sudo apt install php7.0 
sudo apt install php7.1 
sudo apt install php7.2 
sudo apt install php7.3 
sudo apt install php7.4 
sudo apt install php8.0 

```
3. look version php
```
sudo php -v

```
4. set default php
```
#if you want to set php 5.6
sudo update-alternatives --set php /usr/bin/php5.6
```

```
#for nginx server you must run php fpm 
# example with php 7.4
sudo service php7.4-fpm start

#Note: if php fpm not run in nginx it will show error 502
```

```
# for apache 
----------- Disable PHP Version ----------- 
$ sudo a2dismod php5.6
$ sudo a2dismod php7.0
$ sudo a2dismod php7.1
$ sudo a2dismod php7.2
$ sudo a2dismod php7.3
$ sudo a2dismod php7.4
$ sudo a2dismod php8.0

----------- Enable PHP Version ----------- 
$ sudo a2enmod php5.6
$ sudo a2enmod php7.1
$ sudo a2enmod php7.2
$ sudo a2enmod php7.3
$ sudo a2enmod php7.4
$ sudo a2enmod php8.0

----------- Restart Apache Server ----------- 
$ sudo systemctl restart apache2

#Note: choose version php do you use.
```

5. php extension
```
# sometime you need run project with extension like laravel project, you can install like below:

---------------------------example php 7.4----------------
sudo apt install php7.4-common php7.4-mysql php7.4-xml php7.4-xmlrpc php7.4-curl php7.4-gd php7.4-imagick php7.4-cli php7.4-dev php7.4-imap php7.4-mbstring php7.4-opcache php7.4-soap php7.4-zip php7.4-intl -y

```

6. configure php
```
# on apache
sudo nano /etc/php/7.4/apache2/php.ini
```
```
#on nginx
sudo nano /etc/php/7.4/fpm/php.ini
```

```
#example configure
upload_max_filesize = 32M 
post_max_size = 48M 
memory_limit = 256M 
max_execution_time = 600 
max_input_vars = 3000 
max_input_time = 1000
```