# Postgresql

## mac
```sh
# install
brew install postgresql

# start service
brew services start postgresql

# to postgresql
psql postgres


# create database
CREATE DATABASE your_database_name;

# create user
CREATE USER your_username WITH PASSWORD 'your_password';

# grant access for db
GRANT ALL PRIVILEGES ON DATABASE your_database_name TO your_username;

# grant for super user

```