# Uninstall Postgresql in Ubuntu
```sh
# Run the following commands to remove PostgreSQL:
sudo apt-get --purge remove postgresql postgresql-client postgresql-contrib
sudo apt-get autoremove
# If you want to remove the configuration files as well, you can run:
sudo rm -r /etc/postgresql/
```

# Install Postgresql in Ubuntu
## 1. install 
```sh
sudo apt update
sudo apt install postgresql postgresql-contrib

```

## 2. Runing Service
```sh
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

## 3. Create User 
```sh
# The default PostgreSQL installation on Ubuntu creates a system user named postgres. You can switch to this user to interact with PostgreSQL:
sudo -i -u postgres

# Now, you can access the PostgreSQL prompt by typing:
psql
```

## 4. Create Database
```sh
CREATE DATABASE your_database_name;

# list database
\l

```

## 5. Create a User (Optional)
```sh
CREATE USER your_username WITH PASSWORD 'your_password';
ALTER ROLE your_username SET client_encoding TO 'utf8';
ALTER ROLE your_username SET default_transaction_isolation TO 'read committed';
ALTER ROLE your_username SET timezone TO 'UTC';
GRANT ALL PRIVILEGES ON DATABASE your_database_name TO your_username;


# if you want exit from prompt
\q
exit
```