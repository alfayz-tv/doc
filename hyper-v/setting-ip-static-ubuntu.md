# In your ubuntu server open your network netplan
```
cd /etc/netplan
sudo nano <your-net-plan>

```
# Type the following into your netplan
```yaml
network:
  version: 2
  renderer: networkd  
  ethernets:
    eth0:
      addresses:
      - 192.168.0.2/24
      gateway4: 192.168.0.1
      nameservers:
        addresses:
        - 8.8.8.8
        - 8.8.4.4
      dhcp4: no

```

# Then
```
sudo netplan apply

```