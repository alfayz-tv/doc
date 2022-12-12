# Setting and Install Golang on Ubuntu 18.04/20.04/22.04 Server

## 1. Install Golang
```sh
# install
sudo apt update
sudo apt upgrade -y

# download from golang github
curl -OL https://golang.org/dl/go1.19.4.linux-amd64.tar.gz

# install
rm -rf /usr/local/go && tar -C /usr/local -xzf go1.19.4.linux-amd64.tar.gz

# add gopath
export PATH=$PATH:/usr/local/go/bin
source .bashrc

# check version
go version

```

## 2. Install Project
```sh

# pull project
git clone {{git_url}}

cd project_name

# build
go mod tidy
go build main.go
```

## 3. Make Service for project
### make service
```sh
sudo nano /lib/systemd/system/goweb.service

```

### with value
```sh
[Unit]
Description=goweb

[Service]
Type=simple
Restart=always
RestartSec=5s
ExecStart=/home/user/go/go-web/main

[Install]
WantedBy=multi-user.target

```

### run service
```sh
# run
sudo service goweb start

# check
sudo service goweb status
```

## 4. Setting Nginx / reverse proxy

```sh
# add conf
sudo nano /etc/nginx/sites-available/example.test

# enable
sudo ln -s /etc/nginx/sites-available/example.test /etc/nginx/sites-enabled/


# reload
sudo nginx -s reload
```


> example nginx conf
```sh
server {
    server_name example.com www.example.com;
    location / {
        proxy_pass http: //localhost:9990;
    }
}

```