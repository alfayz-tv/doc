1. Install supervisor
```
sudo apt-get install supervisor
```

2. add config in /etc/supervisor/conf.d 
```
cd /etc/supervisor/conf.d 
touch laravel-worker.conf

#and add config
```

```
# example global config

[program:laravel-worker]
process_name=%(program_name)s_%(process_num)02d
command=php /home/forge/app.com/artisan queue:work sqs --sleep=3 --tries=3 --daemon
autostart=true
autorestart=true
user=forge
numprocs=8
redirect_stderr=true
stdout_logfile=/home/forge/app.com/worker.log

```

```
# example specifik config 

[program:renew_download_ssl]
process_name=%(program_name)s_%(process_num)02d
command=php /home/networksupport/var/www/html/Website-builder-V2/artisan queue:work sqs --sleep=3 --tries=2 --queue=renew_download_ssl --daemon
autostart=true
autorestart=true
user=root
numprocs=8
redirect_stderr=true
stdout_logfile=/home/networksupport/var/www/html/Website-builder-V2/storage/logs/renew_download_ssl.log

```

3. reload supervisorctl
```
sudo supervisorctl reread
sudo supervisorctl update
sudo supervisorctl start laravel-worker:*

# if you want start all config
sudo supervisorctl reload
```
