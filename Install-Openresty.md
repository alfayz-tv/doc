1. Install Openresty in Ubuntu
```
# if nginx is already installed and running, try disabling and stopping it before installing openresty like below:

sudo systemctl disable nginx
sudo systemctl stop nginx

```
- Step 1: we should install some prerequisites needed by adding GPG public keys (could be removed later):
```
sudo apt-get -y install --no-install-recommends wget gnupg ca-certificates
```
- Step 2: import our GPG key:
```
wget -O - https://openresty.org/package/pubkey.gpg | sudo apt-key add -
```
- Step 3: then add the our official APT repository.
```
# For x86_64 or amd64 systems:

echo "deb http://openresty.org/package/ubuntu $(lsb_release -sc) main" \
    | sudo tee /etc/apt/sources.list.d/openresty.list

# And for arm64 or aarch64 systems:

echo "deb http://openresty.org/package/arm64/ubuntu $(lsb_release -sc) main" \
    | sudo tee /etc/apt/sources.list.d/openresty.list
```

- Step 4: update the APT index:
```
sudo apt-get update
sudo apt-get -y install openresty
```

2. Run Openresty
```
# export path
PATH=/usr/local/openresty/nginx/sbin:$PATH
export PATH

# look service openresty runing or not
sudo service --status-all

# if openresty not runing
sudo service openresty start

# run nginx with config
nginx -p `pwd`/ -c my_config.conf
```

3. If failed
```
# openresty need 443 and 80 port open

# look port (install first : sudo apt-get install net-tools)
sudo netstat -lpn |grep :80

# look process id use port 80
sudo lsof -t -i:80

# kill procces
sudo kill number_process

```
```
Notes:
#if can't to run nginx and error lua_ssl you can run with sbin directly

/usr/local/openresty/nginx/sbin/nginx -p `pwd`/ -c your_config.conf
```

#note : pastikan path nginx openresty sudah di export dengan:
```
echo $PATH
```