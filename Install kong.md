1. Instalasi kong
```
#1. install kong
curl -Lo kong.2.4.1.amd64.deb "https://download.konghq.com/gateway-2.x-ubuntu-$(lsb_release -cs)/pool/all/k/kong/kong_2.4.1_amd64.deb"

sudo dpkg -i kong.2.4.1.amd64.deb

```
2. Install postgresql
```
#install postgresql
sudo apt-get install postgresql postgresql-contrib

# check postgresql is active
sudo systemctl status postgresql

# pindah ke user postgres (user sudah di buat ketika install postgresql)
sudo -i -u postgres

#kemudian masuk ke postgresql
psql

#setting db
#1. create user
CREATE USER kong;

#2. create database
CREATE DATABASE kong OWNER kong;

#3. setting password
ALTER USER kong WITH PASSWORD 'Alfa2021';

```

3. setting kong config
```
#1. salin kong config
sudo cp /etc/kong/kong.conf.default /etc/kong/kong.conf

#2. change config 
sudo nano /etc/kong/kong.conf

#3. copy code below
pg_user = kong
pg_password = Alfa2021
pg_database = kong

#4. run migration
kong migrations bootstrap [-c /etc/kong/kong.conf]

#5. start kong
kong start [-c /etc/kong/kong.conf]

#6. check kong
curl -i http://localhost:8001/
#or
#you can check in browser localhost:8001

```
