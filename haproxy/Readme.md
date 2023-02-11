# install haproxy on mac
```sh
brew install haproxy
```

# install haproxy on ubuntu
```sh
sudo apt update
sudo apt install haproxy -y    

```
# setting cfg file
```sh
# make cfg file
# if in ubuntu place on /etc/haproxy/haproxy.cfg
# if on mac place on /opt/homebrew/etc/haproxy.cfg


# bellow example cfg file

global
    log /dev/log    local0
    log /dev/log    local1 notice
    chroot /var/lib/haproxy
    stats socket /run/haproxy/admin.sock mode 660 level admin expose-fd listeners
    stats timeout 30s
    user haproxy
    group haproxy
    daemon
 
    # Default SSL material locations
    ca-base /etc/ssl/certs
    crt-base /etc/ssl/private
 
    # Default ciphers to use on SSL-enabled listening sockets.
    # For more information, see ciphers(1SSL). This list is from:
    #  https://hynek.me/articles/hardening-your-web-servers-ssl-ciphers/
    # An alternative list with additional directives can be obtained from
    #  https://mozilla.github.io/server-side-tls/ssl-config-generator/?server=haproxy
    ssl-default-bind-ciphers ECDH+AESGCM:DH+AESGCM:ECDH+AES256:DH+AES256:ECDH+AES128:DH+AES:RSA+AESGCM:RSA+AES:!aNULL:!MD5:!DSS
    ssl-default-bind-options no-sslv3
 
defaults
    log     global
    mode    http
    option  httplog
    option  dontlognull
    timeout connect 5000
    timeout client  50000
    timeout server  50000
    errorfile 400 /etc/haproxy/errors/400.http
    errorfile 403 /etc/haproxy/errors/403.http
    errorfile 408 /etc/haproxy/errors/408.http
    errorfile 500 /etc/haproxy/errors/500.http
    errorfile 502 /etc/haproxy/errors/502.http
    errorfile 503 /etc/haproxy/errors/503.http
    errorfile 504 /etc/haproxy/errors/504.http
 
frontend http_front
   bind *:80
   mode http
   default_backend http_back
 
backend http_back    
    mode http
    balance roundrobin
    option forwardfor
    http-request set-header X-Forwarded-Port %[dst_port]
    http-request add-header X-Forwarded-Proto https if { ssl_fc }
    option httpchk HEAD / HTTP/1.1rnHost:localhost
    server node1 10.130.127.167:80
    server node2 10.130.128.35:80
 
listen stats 
    bind *:1234
    stats enable
    stats hide-version
    stats refresh 30s
    stats show-node
    stats auth username:password
    stats uri /stats 
```

```sh
# example another haproxy.cfg
##based on Mesosphere Marathon's servicerouter.py haproxy config

global
  daemon
  log 127.0.0.1 local0
  log 127.0.0.1 local1 notice
  maxconn 4096
  tune.ssl.default-dh-param 2048

defaults
  log               global
  retries           3
  maxconn           2000
  timeout connect   5s
  timeout client    50s
  timeout server    50s

listen stats
  bind 127.0.0.1:9090
  balance
  mode http
  stats enable
  stats auth admin:admin

frontend microservice_http_in
  bind *:80
  mode http

frontend microservice_http_appid_in
  bind *:81
  mode http
  acl app__accountCreationService hdr(x-microservice-app-id) -i /accountCreationService
  acl app__profileEditingService hdr(x-microservice-app-id) -i /profileEditingService
  use_backend accountCreationService_10000 if app__accountCreationService
  use_backend profileEditingService_20000 if app__profileEditingService

frontend microservice_https_in
  bind *:443 ssl crt /etc/ssl/yourCertificate
  mode http

frontend accountCreationService_10000
  bind *:10000
  mode http
  use_backend accountCreationService_10000

frontend profileEditingService_20000
  bind *:20000
  mode http
  use_backend profileEditingService_20000

backend profileEditingService_20000
  balance roundrobin
  mode http
  option forwardfor
  http-request set-header X-Forwarded-Port %[dst_port]
  http-request add-header X-Forwarded-Proto https if { ssl_fc }
  server 151_256_250_152_35900 151.256.250.152:35900
  # additional servers here

backend accountCreationService_10000
  balance roundrobin
  mode http
  option forwardfor
  http-request set-header X-Forwarded-Port %[dst_port]
  http-request add-header X-Forwarded-Proto https if { ssl_fc }
  server 101_206_200_192_31900 101.206.200.192:31900
  # additional servers here
```

```
global
  daemon
  log 127.0.0.1 local0
  log 127.0.0.1 local1 notice
  maxconn 4096
  tune.ssl.default-dh-param 2048

defaults
  log               global
  retries           3
  maxconn           2000
  timeout connect   5s
  timeout client    50s
  timeout server    50s

listen stats
  bind 127.0.0.1:9090
  balance
  mode http
  stats enable
  stats auth admin:admin

```

## load file configuration
```
# on ubuntu
haproxy -c -f /etc/haproxy/haproxy.cfg

# on mac
haproxy -c -f /opt/homebrew/etc/haproxy/haproxy.cfg
```

## restart haproxy
```
systemctl restart haproxy
```