# install opensatck on ubuntu
1. Add user 
```
sudo useradd -s /bin/bash -d /home/ghost/ -m -G sudo ghost
sudo passwd ghost

```
2. Next, run the command below to assign sudo privileges to the use
```
echo "alfa ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/alfa

```
1. clone project 
```
git clone https://opendev.org/openstack/devstack.git
```
2. add local config
```
cd devstack
vim local.conf

# insert below:

[[local|localrc]]

# Password for KeyStone, Database, RabbitMQ and Service
ADMIN_PASSWORD=admin
DATABASE_PASSWORD=$ADMIN_PASSWORD
RABBIT_PASSWORD=$ADMIN_PASSWORD
SERVICE_PASSWORD=$ADMIN_PASSWORD
```

3. install devstack
```


```