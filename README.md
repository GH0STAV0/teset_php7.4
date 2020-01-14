## PHP-FPM Image

 **Helpful PHP-FPM image from official ubuntu:xenial**
 >
 > PHP-FPM version - 7.4

 > DateTime - Europe/Kiev

 > Composer installed globally

### Extensions:

 * php7.4-pgsql
 * php7.4-mysql
 * php7.4-opcache
 * php7.4-common
 * php7.4-mbstring
 * php7.4-soap
 * php7.4-cli
 * php7.4-intl
 * php7.4-json
 * php7.4-xsl
 * php7.4-imap
 * php7.4-ldap
 * php7.4-curl
 * php7.4-gd
 * php7.4-zip
 * php7.4-fpm
 * php7.4-redis
 * php-memcached
 * php-mongodb
 * php7.4-imagick
 * php7.4-bcmath
 * php-zmq
 * php7.4-apcu
 * php7.4-sqlite
 * php7.4-sqlite3
 * sqlite3

### In addition

 * Composer (installed globally)
 * Cron
 * Browscap ([Browscap official page](http://browscap.org/))
 * wkhtmltopdf ([Official website](https://wkhtmltopdf.org/))
 
### Docker-compose usage (example)

```yaml
version: "2"
services:
 nginx:
   image: nginx:1.17
   depends_on:
    - php-fpm
   links:
    - php-fpm
   environment:
    - NGINX_PORT=80
    - FASTCGI_HOST=php-fpm
    - FASTCGI_PORT=9000
    - DOCUMENT_ROOT=/usr/local/src/app/web # ROOT folder for Symfony framework
   ports:
    - 8600:80
   volumes:
    - ./stack/nginx/templates/default.conf.template:/etc/nginx/conf.d/default.conf.template
    - ./stack/nginx/entrypoint.sh:/entrypoint.sh
   volumes_from:
    - php-fpm
   command: "/bin/bash /entrypoint.sh"

 database:
   image: mysql:5.7
   environment:
     MYSQL_ROOT_PASSWORD: root_password
     MYSQL_DATABASE: app
     MYSQL_USER: app
     MYSQL_PASSWORD: password
   ports:
    - 3309:3306
   volumes:
    - data:/var/lib/mysql

 php-fpm:
   image: itdevgroup/php-fpm7.4
   depends_on:
    - database
   links:
    - database
   volumes:
    - .:/usr/local/src/app
   working_dir: /usr/local/src/app
   extra_hosts:
    - "app:127.1.0.1"
   environment:
     DB_HOST: database
     DB_PORT: 3306
     DB_DATABASE: app
     DB_USERNAME: app
     DB_PASSWORD: password
volumes:
 data: {}
```
