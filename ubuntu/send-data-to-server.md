# with rsync

```sh
# with port defined
rsync --progress -e 'ssh -p 2122' domain.sql user_ssh@127.0.0.1:/home/user_ssh
```