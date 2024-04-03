---
layout: post
title: Install Nextcloud and Onlyoffice with Dokcer
---

Nextcloud is like Google Drive, and OnlyOffice is like Google Docs. They can be installed together on a personal computer. Docker simplifies the installation and reinstallation. The figure below illustrates the working principle. All components are inside the Docker, which can be installed on both Windows and Linux. Clients interact with Nginx through web browsers or apps. If the client and the server are not on the same local network, either a public IP address or a tunneling service like ZeroTier is required.

![image](https://github.com/cover-me/cover-me.github.io/assets/22870592/3fbe75bb-f5cd-48ce-9875-94a37b413093)

## Install Docker

The installation of Docker on either Windows or Linux can be found elsewhere.

## Install MySQL

MySQL serves as the database for Nextcloud. To install MySQL, execute the following code line by line. Replace `[PASSWORD1]` and `[PASSWORD2]` with your desired passwords. Lines beginning with "#" are comments. The database user name `nextcloud` and its password `[PASSWORD2]` will be required during the initialization of Nextcloud. 

```console
# Create a MySQL containter named "mysql", with root password [PASSWORD1]
docker run -itd --name mysql -e MYSQL_ROOT_PASSWORD=[PASSWORD1] mysql
# Open the shell of the container
docker exec -it mysql /bin/bash
# Log in to the MySQL database
mysql -uroot -p'[PASSWORD1]'
# Create a database user named "nextcloud", with password [PASSWORD2]
create user nextcloud identified by '[PASSWORD2]';
# Grant privileges to the database user "nextcloud"
grant all privileges on nextcloud.* to nextcloud;
# print information (optional)
select host,user,plugin,authentication_string from mysql.user;
# Alter the authentication method of the database user "nextcloud" so that Nextcloud can log in this account using a password ([PASSWORD2]).
ALTER USER 'nextcloud'@'%' IDENTIFIED WITH mysql_native_password BY '[PASSWORD2]';
# print information (optional)
select host,user,plugin,authentication_string from mysql.user;
# Exit the database
exit
# Exit the shell of the container
exit
```

## Install Nginx

Nginx serves as the HTTP server for the server computer. To install Nginx, execute the following code line by line. Replace `[path to .key file]` and `[path to .crt file]` with your desired paths. 

```console
# Create an Nginx container named "nginx". Expose 443 and 80 ports for further communication.
docker run -itd --name nginx -p 443:443 -p 80:80 nginx
# Open the shell of the container
docker exec -it nginx bash
# Generate KEY and CRT files for HTTPS encryption. HTTPS enables encryption, which is safer than HTTP.
openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:4096 -keyout [path to .key file] -out [path to .crt file]
# install nano so we could edit the configuration file
apt update
apt install nano
```

Next, make changes to the configuration file of Nginx.

```console
nano /etc/nginx/conf.d/default.conf
```

Delete all content and replace it with the following code

```console
# generated 2023-01-02, Mozilla Guideline v5.6, nginx 1.23.3, OpenSSL 1.1.1n, modern configuration
# https://ssl-config.mozilla.org/#server=nginx&version=1.23.3&config=modern&openssl=1.1.1n&guideline=5.6

upstream docservice {
  server [IP of container onlyoffice];
}

map $http_host $this_host {
    "" $host;
    default $http_host;
}

map $http_x_forwarded_proto $the_scheme {
     default $http_x_forwarded_proto;
     "" $scheme;
}

map $http_x_forwarded_host $the_host {
    default $http_x_forwarded_host;
    "" $this_host;
}

map $http_upgrade $proxy_connection {
  default upgrade;
  "" close;
}

server {
    listen 80 default_server;
    listen [::]:80 default_server;

    location / {
        return 301 https://$host$request_uri;
    }
}

# Nextcloud
server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    server_name [server_domain1];

    ssl_certificate [path to .crt file];
    ssl_certificate_key [path to .key file];
    ssl_session_timeout 1d;
    ssl_session_cache shared:MozSSL:10m;  # about 40000 sessions
    ssl_session_tickets off;

    # modern configuration
    ssl_protocols TLSv1.3;
    ssl_prefer_server_ciphers off;

    # HSTS (ngx_http_headers_module is required) (63072000 seconds)
    add_header Strict-Transport-Security "max-age=63072000" always;

    client_max_body_size 4096M;

    location / {
        proxy_pass  http://[IP of container nextcloud]/;
        proxy_set_header X-Forwarded-Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto https;
    }

    # https://docs.nextcloud.com/server/25/admin_manual/configuration_server/reverse_proxy_configuration.html
    location /.well-known/carddav {
            return 301 $scheme://$host/remote.php/dav;
    }

    location /.well-known/caldav {
            return 301 $scheme://$host/remote.php/dav;
    }
}

# Onlyoffice
server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    server_name [server_domain2];
    server_tokens off;

    ssl_certificate [path to .crt file];
    ssl_certificate_key [path to .key file];
    ssl_verify_client off;

    ssl_ciphers "EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH";

    ssl_protocols  TLSv1 TLSv1.1 TLSv1.2;
    ssl_session_cache  builtin:1000  shared:SSL:10m;

    ssl_prefer_server_ciphers   on;
    add_header X-Content-Type-Options nosniff;

    location / {
            proxy_pass  http://docservice;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection $proxy_connection;
            proxy_set_header X-Forwarded-Host $the_host;
            proxy_set_header X-Forwarded-Proto $the_scheme;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```
The configuration includes three "server" blocks. The first block redirects all HTTP connections to HTTPS connections, which are safer. The second one is for Nextcloud. The last one is for Onlyoffice. The styles for the latter two blocks are different because I grabbed configurations from different places.

Replace `[path to .key file]` and `[path to .crt file]` with the actual paths.

Replace [IP of container onlyoffice] and [IP of container nextcloud] with corresponding IPs. We can predict the IPs before containers are created. The docker server itself typically has an IP address of 172.17.0.1. The first running container (mysql) has an IP address of 172.17.0.2, and so on. As Nextcloud and Onlyoffice will be created and run as the 3rd and 4th containers, their IP addresses should be 172.17.0.4 and 172.17.0.5, respectively.

Replace `[server_domain1]` and `[server_domain2]` with the actual domains. Thanks to the mDNS protocol, there are two "domains" available immediately without any configuration, i.e., the computer's hostname and "hostname.local". The hostname can be obtained by running `hostname` in the shell or command line. If the hostname is `server`, one can replace `[server_domain1]` and `[server_domain2]` with `server.local` and `server`, respectively.

Finally, excute the following code in the shell line by line.

```console
# exit the container
exit
# restart container nginx
sudo docker restart nginx
```

## Install Nextcloud

```console
# Create and run the container named "nextcloud". "--link" links a domain and a container IP in the hosts file
sudo docker run -itd  -v [path to data folder]:/var/www/html/data --name nextcloud --link mysql:mysql --link nginx:[server_domain1] --link nginx:[server_domain2] nextcloud
```

Replace [path to data folder] with a path on the server computer if you want to store Nextcloud data outside the default place; otherwise, remove the `-v [path to data folder]:/var/www/html/data` stuff. This could be useful when reinstalling the container or if one wants to store the folder on an external hard drive.

Now, open the browser on the server or a local client and navigate to `https://[server_domain1]/` (e.g., `https://server.local/`). The browser will display a warning indicating a Potential Security Risk Ahead,  becasue the website's certificate is self-signed. Ignore this warning by clicking on "Advanced..." and then "Accept the risk and continue" (for Firefox). The web page will prompt you to create an admin account for Nextcloud. Choose MySQL as the database type and enter the appropriate database username and password. The host is mysql. Skip the installation of apps, as there may be errors before further configuring Nextcloud using the code below,

```console
# Open the configuration file
docker exec -it nextcloud bash
apt update
apt install nano
nano config/config.php
```

Add [server_domain1] (domain for nextcloud) and [IP of the server] to `trusted_domains`.

Add the following code. Replace [IP of conainer nginx] with the IP address of the Nginx container:

```
  'trusted_proxies'   => ['[IP of conainer nginx]'],
  'overwriteprotocol' => 'https',
  'allow_local_remote_servers' => true,
  'default_phone_region' => 'CN',
  'maintenance_window_start' => 17,
```

Exit the container. The configuration changes will take effect automatically, so there is no need to restart the container.


## Install Onlyoffice (optional)

replace [PASSWORD3] with your desired password:

```
docker run -itd --name onlyoffice -e JWT_SECRET=[PASSWORD3] -e USE_UNAUTHORIZED_STORAGE=true --link nginx:[server_domain1] --link nginx:[server_domain2] onlyoffice/documentserver
```

### Configure Onlyoffice in Nextcloud

Browse https://[server_domain2]/ and trust the Onlyoffice site's certification (which uses the same KEY and CRT files as the Nextcloud, just with a different domain). Otherwise there would be errors loading Onlyoffice within Nextcloud. On the APPs page, install the "Onlyoffice" app. Go to administrative settings - onlyoffice, set the server URL to `https://[server_domain2]/`, Check "disable certificate verification" and click the "Apply".


## Install redis (optional)

Redis is used to improve the file locking efficiency in Nextcloud. If Redis is not used, a warning may appear on the administration page of Nextcloud.

```console
sudo docker run -itd --name redis --link nginx:[server_domain1] --link nginx:[server_domain2] redis
```

Add 

```console
  'memcache.locking' => '\OC\Memcache\Redis',
  'memcache.distributed' => '\OC\Memcache\Redis',
  'redis' => [
     'host' => '[IP of container redis]',
     'port' => 6379,
  ],
```

To Nextcloud's configuration file (config/config.php).

## Known issues

mDNS is not supported by Android. As a result, the domains [server_domain1] and [server_domain2] may not be accessible on Android devices. One can still use the IP address of the server to access Nextcloud. However, Onlyoffice integration within Nextcloud may not work in this scenario.
