# Powerdns
Mengapa Menggunakan PowerDNS?

PowerDNS menyediakan dua solusi server nama:

- Server Resmi, yang menggunakan database untuk menyelesaikan kueri tentang domain.
- Recursor, yang berkonsultasi dengan server otoritatif lain untuk menyelesaikan kueri.

Server nama lain menggabungkan kedua fungsi tersebut secara otomatis. PowerDNS menawarkannya secara terpisah, dan memungkinkan perpaduan dua solusi secara mulus untuk pengaturan modular.

# Install Powerdns di Ubuntu 18.04, 20.04, & 22.04
## Step A: Install and Configure MariaDB Server

### 1. Update and upgrade system packages:
```sh
sudo apt update && sudo apt upgrade
```

### 2. Install the MariaDB server and client with:
```sh
sudo apt install mariadb-server mariadb-client
```

### 3. Connect to MariaDB with:
```
# login to mysql
sudo mysql
```

### 4. Create a database for the PowerDNS nameserver:
```sh
# create database with name powerdns
create database powerdns;
```

### 5. Grant all privileges to the pda user and provide the user password:
```sh
#create user
CREATE USER 'user'@localhost IDENTIFIED BY 'password1';

# give grant access
grant all privileges on powerdns.* TO 'user'@'localhost' identified by 'YOUR_PASSWORD_HERE';
# commit
flush privileges;

#show all user
SELECT User FROM mysql.user;
```

![image mysql powerdns setup](https://github.com/alfayz-tv/doc/blob/master/images/mysql_powerdns.png)
### 6. Connect to the database:
```sh
# use database powerdns
use powerdns;
```

### 7. Use the following SQL queries to create tables for the pda database:
```sql
CREATE TABLE domains (
  id                    INT AUTO_INCREMENT,
  name                  VARCHAR(255) NOT NULL,
  master                VARCHAR(128) DEFAULT NULL,
  last_check            INT DEFAULT NULL,
  type                  VARCHAR(6) NOT NULL,
  notified_serial       INT UNSIGNED DEFAULT NULL,
  account               VARCHAR(40) CHARACTER SET 'utf8' DEFAULT NULL,
  PRIMARY KEY (id)
) Engine=InnoDB CHARACTER SET 'latin1';
CREATE UNIQUE INDEX name_index ON domains(name);
CREATE TABLE records (
  id                    BIGINT AUTO_INCREMENT,
  domain_id             INT DEFAULT NULL,
  name                  VARCHAR(255) DEFAULT NULL,
  type                  VARCHAR(10) DEFAULT NULL,
  content               VARCHAR(64000) DEFAULT NULL,
  ttl                   INT DEFAULT NULL,
  prio                  INT DEFAULT NULL,
  change_date           INT DEFAULT NULL,
  disabled              TINYINT(1) DEFAULT 0,
  ordername             VARCHAR(255) BINARY DEFAULT NULL,
  auth                  TINYINT(1) DEFAULT 1,
  PRIMARY KEY (id)
) Engine=InnoDB CHARACTER SET 'latin1';
CREATE INDEX nametype_index ON records(name,type);
CREATE INDEX domain_id ON records(domain_id);
CREATE INDEX ordername ON records (ordername);
CREATE TABLE supermasters (
  ip                    VARCHAR(64) NOT NULL,
  nameserver            VARCHAR(255) NOT NULL,
  account               VARCHAR(40) CHARACTER SET 'utf8' NOT NULL,
  PRIMARY KEY (ip, nameserver)
) Engine=InnoDB CHARACTER SET 'latin1';
CREATE TABLE comments (
  id                    INT AUTO_INCREMENT,
  domain_id             INT NOT NULL,
  name                  VARCHAR(255) NOT NULL,
  type                  VARCHAR(10) NOT NULL,
  modified_at           INT NOT NULL,
  account               VARCHAR(40) CHARACTER SET 'utf8' DEFAULT NULL,
  comment               TEXT CHARACTER SET 'utf8' NOT NULL,
  PRIMARY KEY (id)
) Engine=InnoDB CHARACTER SET 'latin1';
CREATE INDEX comments_name_type_idx ON comments (name, type);
CREATE INDEX comments_order_idx ON comments (domain_id, modified_at);
CREATE TABLE domainmetadata (
  id                    INT AUTO_INCREMENT,
  domain_id             INT NOT NULL,
  kind                  VARCHAR(32),
  content               TEXT,
  PRIMARY KEY (id)
) Engine=InnoDB CHARACTER SET 'latin1';
CREATE INDEX domainmetadata_idx ON domainmetadata (domain_id, kind);
CREATE TABLE cryptokeys (
  id                    INT AUTO_INCREMENT,
  domain_id             INT NOT NULL,
  flags                 INT NOT NULL,
  active                BOOL,
  content               TEXT,
  PRIMARY KEY(id)
) Engine=InnoDB CHARACTER SET 'latin1';
CREATE INDEX domainidindex ON cryptokeys(domain_id);
CREATE TABLE tsigkeys (
  id                    INT AUTO_INCREMENT,
  name                  VARCHAR(255),
  algorithm             VARCHAR(50),
  secret                VARCHAR(255),
  PRIMARY KEY (id)
) Engine=InnoDB CHARACTER SET 'latin1';
CREATE UNIQUE INDEX namealgoindex ON tsigkeys(name, algorithm);

```

### 8. Confirm the tables have been created with:
```sh
show tables;

# And then exit mysql
exit;
```

![image mysql powerdns tables](https://github.com/alfayz-tv/doc/blob/master/images/mysql_showtables.png)
