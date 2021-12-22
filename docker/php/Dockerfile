FROM php:8.1-fpm-alpine

# Copy File Config
# ADD ./docker/php/www.conf /usr/local/etc/php-fpm.d/www.conf

# Add and set Group
# RUN addgroup -g 1000 site \
#     && adduser -G site -g site -s /bin/sh -D site \
#     && mkdir -p /var/www/html \
#     && chown site:site /var/www/html

# Work in the specific space
WORKDIR /var/www/html

# Install dependencies
RUN apk add --no-cache freetype libpng libjpeg-turbo freetype-dev libpng-dev libjpeg-turbo-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg

RUN NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) && \
    docker-php-ext-install -j${NPROC} gd 

RUN apk del --no-cache freetype-dev libpng-dev libjpeg-turbo-dev

# Install extensions
COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/bin/
RUN install-php-extensions gd curl json mbstring pdo_mysql xml zip bcmath iconv

# Install swoole
RUN apk add --no-cache libstdc++ supervisor nano unzip ${PHPIZE_DEPS} \
    && pecl install openswoole \
    && docker-php-ext-enable openswoole \
    && apk del ${PHPIZE_DEPS}


COPY ./docker/php/supervisor/supervisor.conf /etc/supervisor/conf.d/supervisor.conf
COPY ./docker/php/supervisor/supervisord.conf /etc/supervisor/supervisord.conf

COPY ./docker/php/run.sh /tmp
ENTRYPOINT ["/tmp/run.sh"]