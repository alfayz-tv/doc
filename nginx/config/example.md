```sh
server {
    listen 80;
    server_name _;
    root /var/www/html/example/public;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-XSS-Protection "1; mode=block";
    add_header X-Content-Type-Options "nosniff";

    index index.html index.htm index.php;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    error_page 404 /index.php;

    location ~ \.php$ {
        fastcgi_pass unix:/run/php/php7.4-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}

server {
    listen 80;s
    server_name *.webisa.id;
    root /var/www/html/example/public;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-XSS-Protection "1; mode=block";
    add_header X-Content-Type-Options "nosniff";

    index index.html index.htm index.php;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    error_page 404 /index.php;

    location ~ \.php$ {
        fastcgi_pass unix:/run/php/php7.4-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```

```sh
# Default server block for non-matching requests
server {
    listen 80 default_server;
    server_name _;
    return 404;
}

# Main server block for your_domain.com
server {
    listen 80;
    server_name your_domain.com www.your_domain.com;

    root /path/to/your_domain/public;
    index index.html;

    location / {
        # Additional configuration for your_domain.com
        # ...
    }
}

# Subdomain server block for subdomain1.your_domain.com
server {
    listen 80;
    server_name subdomain1.your_domain.com;

    root /path/to/subdomain1/public;
    index index.html;

    location / {
        # Additional configuration for subdomain1.your_domain.com
        # ...
    }
}

# Subdomain server block for subdomain2.your_domain.com
server {
    listen 80;
    server_name subdomain2.your_domain.com;

    root /path/to/subdomain2/public;
    index index.html;

    location / {
        # Additional configuration for subdomain2.your_domain.com
        # ...
    }
}

# Additional subdomain server blocks can be added as needed

# SSL Configuration (if you want to support HTTPS)
server {
    listen 443 ssl;
    server_name your_domain.com www.your_domain.com subdomain1.your_domain.com subdomain2.your_domain.com;

    ssl_certificate /path/to/ssl_certificate.crt;
    ssl_certificate_key /path/to/ssl_certificate_key.key;

    # Additional SSL configuration
    # ...

    location / {
        # Additional configuration for HTTPS
        # ...
    }
}

```