## Docker php83-fpm-alpine image

Docker php83-fpm-alpine image, based on alpine image.

- Alpine
- PHP 8.3.9
- GD 2.1.0 Support
- Memcached 3.2.0 support
- Redis 6.0.2 support
- mbstring support
- mysqli support
- pdo_mysql support
- pdo_pgsql support

## How to use?

DockerHub repository: https://hub.docker.com/r/johnnyworks/php83-fpm-alpine

You can set your website up by using `docker-compose`

Here is sample of **docker-compose.yml**

```
version: '3.7'
services:
  nginx:
    image: nginx:mainline-alpine-slim
    restart: always
    ulimits:
      nofile:
        soft: 10240
        hard: 10240
    ports:
      - '80:80'
    volumes:
      - ./www:/data
      - ./conf:/etc/nginx/conf.d
    links:
      - phpfpm
    logging:
      driver: 'json-file'
      options:
        max-file: '2'
        max-size: '2m'
    networks:
      - default
  phpfpm:
    image: johnnyworks/php83-fpm-alpine:latest
    restart: always
    ulimits:
      nofile:
        soft: 10240
        hard: 10240
    volumes:
      - ./www:/data
      - ./php.ini:/usr/local/etc/php/php.ini
      - ./phpfpm.conf:/usr/local/etc/php-fpm.d/www.conf
    logging:
      driver: 'json-file'
      options:
        max-file: '2'
        max-size: '2m'
    networks:
      - default
networks:
  default:
```

Put correspond files to following path

- conf/nginx.conf
- php.ini
- phpfpm.conf

and `www` folder for web root.

then using commands to start your web server.

```
# docker-compose up -d
```