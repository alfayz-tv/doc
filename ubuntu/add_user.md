# add user
```sh
sudo adduser new_user

# add to sudo group
usermod -aG sudo new_user

#test user is sudo
sudo su
# insert your password
#then
su - new_user
```