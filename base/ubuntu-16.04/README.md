### RUN APPLICATION

``` sh
# terminal
docker run \
    -p 9001:9001 -p 80:80 \
    -v /$(pwd):/app \
    -v /$(pwd)/docker/nginx.conf:/etc/nginx/sites-enabled/default:ro \
    -d --name app devbase-base 
```

``` sh
# terminal
docker-compose up -d
```


``` ruby
# docker-compose.yml
version: '3'

services:
  mysqlsrv:
    image: "mysql:5.7"
    container_name: db
    environment:
      MYSQL_ROOT_PASSWORD: "root"
      MYSQL_DATABASE: "root"
    ports:
      - "3306:3306"
    volumes:
      - ./docker/mysql:/var/lib/mysql
    networks:
      - mysql-compose-network

  appsrv:
    #build: ./docker/
    image: "facchin/devbase-base:ubuntu-16.04"
    container_name: app
    ports:
      - "80:80"
      - "9001:9001"
    volumes:
      - ./:/app
      - ./docker/nginx.conf:/etc/nginx/sites-enabled/default:ro
    networks:
      - mysql-compose-network

networks: 
  mysql-compose-network:
    driver: bridge
```


### PHP application example

``` sh
# Host system
$ tree
.
├── docker
│   └── nginx.conf
└── public
    └── index.php

2 directories, 2 files
```

``` php
<?php
// index.php
phpinfo();
```

``` nginx
# nginx.conf
server {
    listen 80 default_server;

    server_name example.com;

    index index.php;
    root /app/public;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass             unix:/run/php/php7.2-fpm.sock;
        fastcgi_index            index.php;

        include                  fastcgi_params;

        fastcgi_param            SCRIPT_FILENAME $document_root$fastcgi_script_name;

        fastcgi_intercept_errors on;
        fastcgi_read_timeout     300;
        fastcgi_buffer_size      16k;
        fastcgi_buffers          4 16k;
    }
}
```

## Build command
docker build -t devbase-base .

## LOGIN command
docker exec -it app bash

## OTHER commands
docker stop $(docker ps -a -q) && docker rm $(docker ps -a -q)

docker images -a | grep -vE "ubuntu" | awk '{print $3}' | xargs docker rmi

docker images -a | awk '{print $3}' | xargs docker rmi

docker rmi $(docker images -q) --force