# Mysql




# Create user with access from other (not localhost)
```sh
#example
CREATE USER 'new_user'@`%` IDENTIFIED BY 'password1';
# one db
GRANT ALL PRIVILEGES ON database_name.* TO 'new_user'@'%';
# all db
GRANT ALL PRIVILEGES ON *.* TO 'new_user'@'%';
FLUSH PRIVILEGES;

```

# delete user mysql
```sh
select User from mysql.user;

drop user new_user;

FLUSH PRIVILEGES;

```