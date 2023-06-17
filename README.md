# Setup Ubuntu Server

Update Ubuntu Server

<br>

```bash
sudo apt-get update
```

Add php Repository

```bash
sudo add-apt-repository ppa:ondrej/php
```

Install Nodejs With Various Versions

```url
https://github.com/nodesource/distributions
```

Install Nginx, Mysql, Git, Zip, PHP 7.4 with FPM

```bash
sudo apt-get install nginx mysql-server git install php7.4-fpm php7.4-common php7.4-mysql php7.4-xml php7.4-xmlrpc php7.4-curl php7.4-gd php7.4-imagick php7.4-cli php7.4-dev php7.4-imap php7.4-mbstring php7.4-opcache php7.4-soap php7.4-zip unzip -y
```

Install Nginx, Mysql, Git, Zip, PHP 8.0 with FPM

```bash
sudo apt-get install nginx mysql-server git php8.0-fpm php8.0-common php8.0-mysql php8.0-xml php8.0-xmlrpc php8.0-curl php8.0-gd php8.0-imagick php8.0-cli php8.0-dev php8.0-imap php8.0-mbstring php8.0-opcache php8.0-soap php8.0-zip unzip -y
```
Install Nginx, Mysql, Git, Zip, PHP 8.1 with FPM
```bash
sudo apt-get install nginx mysql-server git php8.1-fpm php8.1-common php8.1-mysql php8.1-xml php8.1-xmlrpc php8.1-curl php8.1-gd php8.1-imagick php8.1-cli php8.1-dev php8.1-imap php8.1-mbstring php8.1-opcache php8.1-soap php8.1-zip unzip -y
```

Install MySQL 8.0
```bash
sudo apt install mysql-server-8.0
```

Mysql Configration File

```bash
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
```

Mysql Initialize
```bash
mysqld --initialize --console
```

Create Mysql User For Particular IP

```sql
CREATE USER 'sandip'@'192.168.0.1' IDENTIFIED WITH mysql_native_password BY 'password';
```

Create Mysql User For Any IP

```sql
CREATE USER 'sandip'@'%' IDENTIFIED WITH mysql_native_password BY 'password';
GRANT ALL PRIVILEGES ON database_name.* TO 'sandip'@'%';
GRANT ALL PRIVILEGES ON *.* TO 'sandip'@'%';
revoke all privileges on demo.* from 'sandip'@'%';
FLUSH PRIVILEGES;
```

remove any package from ubuntu
```bash
sudo apt-get remove --purge mysql*
```

Change Password For ***root*** User

```sql
ALTER USER 'codersandip'@'%' IDENTIFIED BY 'Vandana@25';
```

```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'Admin@workcy';
```

Mysql Confi File
```bash
/etc/mysql/mysql.conf.d/mysqld.cnf
```

Restart Nginx

```bash
sudo systemctl restart nginx
```
Restart PHP FPM

```bash
sudo service php7.4-fpm restart
```

Create Shortcut Link Of Nginx

```bash
sudo ln -s /etc/nginx/sites-available/demo.com /etc/nginx/sites-enabled/
```

Virtual Host Congruation Path

```bash
cd /etc/nginx/sites-available/
```

Restart Mysql

```bash
sudo systemctl restart mysql
```

Install Composer

```bash
cd ~
curl -sS https://getcomposer.org/installer -o composer-setup.php
HASH=`curl -sS https://composer.github.io/installer.sig`
echo $HASH
php -r "if (hash_file('SHA384', 'composer-setup.php') === '$HASH') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php --install-dir=/usr/local/bin --filename=composer
```

Storage Permission Laravel

```bash
sudo chown -R $USER:$USER ./
sudo chown -R $USER:www-data storage
sudo chown -R $USER:www-data bootstrap/cache
sudo chmod -R 775 storage
sudo chmod -R 775 bootstrap/cache
```

Ubuntu create SSH key and move to server

```bash
ssh-keygen
ssh-copy-id user@192.168.0.0
```

Ubuntu create User

```bash
sudo adduser newuser
```

Remove Password From Username
```bash
sudo passwd -d 'username'
```

Assign Sudo Permission to User

```bash
sudo usermod -aG sudo newuser
```

Delete User Ubuntu

```bash
sudo deluser --remove-home newuser
```

Get Storage of Current Directory
```bash
du -sh
```
MongoDB Config file

```bash
$ /etc/mongod.conf
```
 Bind Address changed from 127.0.0.1 to 0.0.0.0
```bash
bindIp: 127.0.0.1
```

```bash
bindIp: 0.0.0.0
```

Nginx Configration File
https://thisinterestsme.com/php-fpm-settings
```bash
##
# You should look at the following URL's in order to grasp a solid understanding
# of Nginx configuration files in order to fully unleash the power of Nginx.
# https://www.nginx.com/resources/wiki/start/
# https://www.nginx.com/resources/wiki/start/topics/tutorials/config_pitfalls/
# https://wiki.debian.org/Nginx/DirectoryStructure
#
# In most cases, administrators will remove this file from sites-enabled/ and
# leave it as reference inside of sites-available where it will continue to be
# updated by the nginx packaging team.
#
# This file will automatically load configuration files provided by other
# applications, such as Drupal or Wordpress. These applications will be made
# available underneath a path with that package name, such as /drupal8.
#
# Please see /usr/share/doc/nginx-doc/examples/ for more detailed examples.
##

# Default server configuration
#
server {
        listen 80;
#       listen [::]:80;

        # SSL configuration
        #
        # listen 443 ssl default_server;
        # listen [::]:443 ssl default_server;
        #
        # Note: You should disable gzip for SSL traffic.
        # See: https://bugs.debian.org/773332
        #
        # Read up on ssl_ciphers to ensure a secure configuration.
        # See: https://bugs.debian.org/765782
        #
        # Self signed certs generated by the ssl-cert package
        # Don't use them in a production server!
        #
        # include snippets/snakeoil.conf;

        root /var/www/html/blog/public;

        # Add index.php to the list if you are using PHP
        index index.php; # index.html index.htm index.nginx-debian.html;

        server_name blog.com;

        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri/ /index.php; # =404;
        }

        # pass PHP scripts to FastCGI server
        #
        location ~ \.php$ {
                include snippets/fastcgi-php.conf;
        #
        #       # With php-fpm (or other unix sockets):
                fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
        #       # With php-cgi (or other tcp sockets):
        #       fastcgi_pass 127.0.0.1:9000;
        }

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        location ~ /\.ht {
                deny all;
        }
}


# Virtual Host configuration for example.com
#
# You can move that to a different file under sites-available/ and symlink that
# to sites-enabled/ to enable it.
#
#server {
#       listen 80;
#       listen [::]:80;
#
#       server_name example.com;
#
#       root /var/www/example.com;
#       index index.html;
#
#       location / {
#               try_files $uri $uri/ =404;
#       }
#}

```
Nginx Load Balancer Configration File


**First Config File**
```
upstream mywebapp1 {
    server 127.0.0.1:81;
    server 127.0.0.1:82;
}

server {
    listen 83;
    server_name sandip.test;

    include "D:/server/laragon/etc/nginx/alias/*.conf";

    location / {
        #try_files $uri $uri/ =404;
                autoindex on;
        proxy_pass http://mywebapp1;
        proxy_set_header Host $server_port;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

```


**Second Server Config File**
```
server {
    listen 81;
    server_name sandip.test;
    root "D:/server/laragon/www/sandip/public/";

    index index.html index.htm index.php;

    # Access Restrictions
    #allow       127.0.0.1;
    #deny        all;

    include "D:/server/laragon/etc/nginx/alias/*.conf";

    location / {
        try_files $uri $uri/ =404;
                autoindex on;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass php_upstream;
        #fastcgi_pass unix:/run/php/php7.0-fpm.sock;
    }


    charset utf-8;

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }
    location ~ /\.ht {
        deny all;
    }
}
```


**Third Server Config File**
```
server {
    listen 82;
    server_name sandip.test;
    root "D:/server/laragon/www/tushar/public/";

    index index.html index.htm index.php;

    # Access Restrictions
    #allow       127.0.0.1;
    #deny        all;

    include "D:/server/laragon/etc/nginx/alias/*.conf";

    location / {
        try_files $uri $uri/ =404;
                autoindex on;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass php_upstream;
        #fastcgi_pass unix:/run/php/php7.0-fpm.sock;
    }


    charset utf-8;

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }
    location ~ /\.ht {
        deny all;
    }

}
```

```bash
sudo update-alternatives --config php

cd /etc/alternatives

```

check symlink