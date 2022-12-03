# Install
## Requirements
- Server with a fresh install of Centos 7.x, Centos 8.x, Ubuntu 18.04, Ubuntu 20.04, AlmaLinux 8
- Python 3.x
- 1024MB RAM, or higher
- 10GB Disk Space

### Install On Ubuntu
```sh
sudo apt update && sudo apt upgrade -y
sh <(curl https://cyberpanel.net/install.sh || wget -O - https://cyberpanel.net/install.sh)

# or

sudo su - -c "sh <(curl https://cyberpanel.net/install.sh || wget -O - https://cyberpanel.net/install.sh)"
```