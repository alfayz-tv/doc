# use certbot with letsencrypt

```sh
# install certbot
sudo apt install certbot

# make certificate
certbot certonly --agree-tos \
--manual \
--preferred-challenges dns \
--email rofai@pandi.id \
--server https://acme-v02.api.letsencrypt.org/directory \
-d "*.domain.net.id"


# add txt record on dns

# 
```