FROM php:7.1.2-fpm

MAINTAINER Vanderson Nunes <vann.nunes@gmail.com>

# configure gd library
RUN docker-php-ext-configure gd \
    --enable-gd-native-ttf \
    --with-jpeg-dir=/usr/lib \
    --with-freetype-dir=/usr/include/freetype2

# Install extensions using the helper script provided by the base image
RUN docker-php-ext-install \
    pdo_mysql \
    pdo_pgsql \
    gd \
    mcrypt

# Redis, Mongo, Xdebug 
RUN pecl install redis-3.1.0 \
    && pecl install xdebug-2.5.0 \
    && pecl install mongodb \
    && docker-php-ext-enable redis xdebug mongodb

RUN curl -L -o /tmp/memcached.tar.gz "https://github.com/php-memcached-dev/php-memcached/archive/php7.tar.gz" \
    && mkdir -p memcached \
    && tar -C memcached -zxvf /tmp/memcached.tar.gz --strip 1 \
    && ( \
        cd memcached \
        && phpize \
        && ./configure \
        && make -j$(nproc) \
        && make install \
    ) \
    && rm -r memcached \
    && rm /tmp/memcached.tar.gz \
    && docker-php-ext-enable memcached

WORKDIR /var/www

 
