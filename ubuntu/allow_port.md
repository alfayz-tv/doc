# open port on ubuntu
```sh
sudo apt install ufw
sudo ufw enable
#allow port 80
sudo ufw allow 80
sudo ufw reload

# check port allow
sudo ufw status
```

# deny port on ubuntu
```sh
sudo ufw enable
sudo ufw deny 80
sudo ufw reload

```