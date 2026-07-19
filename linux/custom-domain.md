## make sure domain is pingable
````
sudo nano /etc/hosts
#then add :  65.109.218.9	odoo360.ir

ping odoo360.ir

````

## Enable proxy mode in Odoo
Edit the Odoo config file (usually /etc/odoo/odoo.conf):
````
nano /etc/odoo/odoo.conf

# then add:
#[options]
#proxy_mode = True

#Then restart Odoo:
sudo systemctl restart odoo
````

## Install Nginx (if not already installed)
````
sudo apt update
sudo apt install nginx
````

## Create Nginx configuration for your domain

````
# Create a new server block:
sudo nano /etc/nginx/sites-available/odoo


### Paste this configuration:
# Upstream definitions (outside any server block)
upstream odoo {
    server 127.0.0.1:8069;
}

upstream odoochat {
    server 127.0.0.1:8072;
}

# HTTP server (port 80) – redirects to HTTPS for consistency during testing
server {
    listen 80;
    server_name odoo360.ir;

    return 301 https://$host$request_uri;  
}

# HTTPS server (port 443) with dummy/self-signed cert
server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    server_name odoo360.ir;   

    ssl_certificate     /etc/nginx/ssl/dummy/dummy.crt;
    ssl_certificate_key /etc/nginx/ssl/dummy/dummy.key;

    # Optional minimal security headers
    add_header X-Content-Type-Options nosniff;
    add_header X-Frame-Options SAMEORIGIN;

    location / {
        proxy_pass http://odoo;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Forwarded-Host $host;
        proxy_read_timeout 7200s;
        proxy_connect_timeout 7200s;
        proxy_send_timeout 7200s;
    }

    location /longpolling/ {
        proxy_pass http://odoochat;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    client_max_body_size 100M;
}
### Paste this configuration end

# enable site:
sudo ln -s /etc/nginx/sites-available/odoo /etc/nginx/sites-enabled/
sudo rm /etc/nginx/sites-enabled/default   # optional: remove default site

# test and reload
sudo nginx -t
sudo systemctl reload nginx
````

## Generate a self-signed dummy certificate for test
````
sudo mkdir -p /etc/nginx/ssl/dummy
sudo openssl req -x509 -nodes -days 3650 -newkey rsa:2048 \
  -subj "/CN=localhost" \
  -keyout /etc/nginx/ssl/dummy/dummy.key \
  -out /etc/nginx/ssl/dummy/dummy.crt
````


## Install free SSL certificate with Let's Encrypt (HTTPS)
````
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -v -d odoo360.ir
````











