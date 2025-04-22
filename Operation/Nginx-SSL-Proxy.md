# Nginx Proxy Set Domain and SSL

[Home](../README.md)

## App Instalation dependency

### Install nginx 

```sh
sudo apt install nginx -y
```

### Install certbot

```sh
sudo apt install certbot python3-certbot-nginx -y
```

## Configure with subdomain

### Setup CNAME

**Type**: A

**Name**: tes

**Value**: Public IP

**TTL**: leave as default


### Create Config file for subdomain

```sh
sudo nano /etc/nginx/sites-available/app.example.com
```

### Configuration for reading html file

```nginx
server {
    listen 80;
    server_name example.com;
    root /var/www/app;

    index index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

### Configuration as proxy

```nginx
server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

## Enable Configuration

### Enable SSL

```sh
sudo certbot --nginx -d tes.example.com
```

### Create Symbolic Link

```sh
sudo ln -s /etc/nginx/sites-available/tes.example.com /etc/nginx/sites-enabled/
```

### Test and Reload Nginx

```sh
sudo nginx -t
sudo systemctl reload nginx
```
