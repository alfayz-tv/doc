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
sudo apt-get -y install openresty
```