FROM php:8.1-fpm-alpine

RUN apk --no-cache update && apk --no-cache upgrade
RUN apk --no-cache add \
    autoconf \
    gcc \
    g++ \
    make
RUN docker-php-ext-install \
    bcmath \
    pdo_mysql

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
