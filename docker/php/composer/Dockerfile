FROM composer:latest

# ADD and set Group
RUN addgroup -g 1000 site \
    && adduser -G site -g site -s /bin/sh -D site \
    && mkdir -p /var/www/html \
    && chown site:site /var/www/html

# Run in work space
WORKDIR /var/www/html