# Install Grafana

## 1. Install
```sh
sudo apt-get install -y adduser libfontconfig1
wget https://dl.grafana.com/oss/release/grafana_9.3.1_amd64.deb
sudo dpkg -i grafana_9.3.1_amd64.deb

# start service
systemctl start grafana-server
```

## 2. Akses on Browser
```sh
http://localhost:3000/login

# default username / password : admin / admin 
```