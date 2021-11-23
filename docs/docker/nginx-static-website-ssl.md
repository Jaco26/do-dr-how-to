# Containerized Static Website with `docker-compose`, `nginx` and `ssl`

## Useful docs
**_Nginx_**:
- https://docs.nginx.com/nginx/admin-guide/web-server/web-server/
- https://docs.nginx.com/nginx/admin-guide/web-server/serving-static-content/

## Background


If you're server is running in a digitalocean droplet, you'll need:
- An A record with example.com pointing to your server’s public IP address.
- An A record with www.example.com pointing to your server’s public IP address.


## Setting up the site

In the root of the the project, create a folder called `public` with a file named `index.html` inside whose contents look something like this:

```html
<!DOCTYPE html>
<html lang="en">
	<head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <title>My Static Site!</title>
    </head>
    <body>
        <p>
            I'm served by a Nginx from inside a docker container!
        </p>
    </body>
</html>
```
In the next section, setup **Nginx** to serve this file as the homepage for our site.

## Configure `nginx`

In the project root, create a folder called `nginx-config` and create a file inside called `nginx.conf`.

Our project structure now looks something like:
```
public/
    index.html
nginx-conf/
    nginx.conf
```
Add the following to `nginx-conf/nginx.conf` (replace `<domain>` with your own domain name)
```conf
server {
    listen 80;
    listen [::]:80;

    root /var/www/html;
    index index.html index.htm index.nginx-debian.html;

    server_name <domain>.com www.<domain>.com;

    location ~ /.well-known/acme-challenge {
        allow all;
        root /var/www/html;
    }
}
```

_refer to Nginx's [serving static content](https://docs.nginx.com/nginx/admin-guide/web-server/serving-static-content/#root-directory-and-index-files) docs for a deeper dive on the config options_ 



## Configure `docker-compose`

In the project root, create a file called `docker-compose.yml` and add the following content
```yml
version: '3.9'
services:
  web:
    image: nginx
    container_name: webserver
    restart: unless-stopped
    ports:
      - "80:80"
    volumes:
      - ./public:/var/www/html
      - ./nginx-conf:/etc/nginx/conf.d
```
