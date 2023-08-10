# Queue with supervisor

```sh
# install
sudo apt install supervisor

# make queue
cd /etc/supervisor/conf.d
sudo touch <name-queue>.conf

# reload supervisor
sudo supervisorctl reread
sudo supervisorctl update

# start all
sudo supervisorctl start all

# start 1
sudo supervisorctl start process_name

# status
sudo supervisorctl status
```


```sh
sudo supervisorctl stop all

```