# this Configuration for Ubuntu 
1. Install Docker
```
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

#then Add Docker’s official GPG key:
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

#update again
sudo apt-get update

# install docker
sudo apt-get install docker-ce docker-ce-cli containerd.io

# if successfully you can check 
docker -v
```

2. install docker compose
```

sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

# if success you can check
docker-compose --version
```

3. add config for docker support Selinux
```
# add or edit daemon.json
sudo nano /etc/docker/daemon.json

```
```
# add this:
{
  "selinux-enabled": true
}
```

4. install mailcow
```
cd /opt
git clone https://github.com/mailcow/mailcow-dockerized
cd mailcow-dockerized
```

5. Setup and install mailcow
```
# you must on folder mailcow-dockerized
# and run
./generate_config.sh

#then run
docker-compose pull

# if all done, then run
docker-compose up -d
```
6. if finish you can check with go to browser and paste your ip

```
# you can login with
username : admin
password: moohoo
```

```
Notes:
# you can edit config mailcow 
sudo nano mailcow.conf

# and you can edit docker-compose.yml with your configuration

```