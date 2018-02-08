FROM php:7.2.0-fpm

ENV TERM xterm

ARG UID
ARG GUID
RUN usermod -u $UID www-data
RUN groupmod -g $GUID www-data

RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    apt-utils

RUN DEBIAN_FRONTEND=noninteractive apt-get install -y \
        bash \
        vim \
        mc \
        cron \
        libmagickwand-dev \
        libmagickcore-dev \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        librabbitmq-dev \
        libmcrypt-dev \
        libpng-dev \
        libpq-dev \
        imagemagick \
        libcurl4-gnutls-dev \
        libc-client-dev \
        libkrb5-dev \
        zlib1g-dev \
        libicu-dev \
        libexif-dev \
        libxrender-dev \
        libzlcore-dev \
        libfreetype6-dev \
        fontconfig \
        git \
        g++ \
        ssmtp \
        xvfb \
        xfonts-75dpi \
        && pecl install imagick \
        && docker-php-ext-enable imagick \
        && pecl install amqp \
        && docker-php-ext-enable amqp \
        && docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
        && docker-php-ext-install imap \
        && docker-php-ext-install -j$(nproc) bcmath curl json xml iconv pdo_pgsql opcache mbstring \
        && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
        && docker-php-ext-install -j$(nproc) gd \
        && docker-php-ext-install exif \
        && docker-php-ext-install fileinfo \
        && docker-php-ext-configure intl \
        && docker-php-ext-install intl \
        && rm -rf /var/lib/apt/lists/*

WORKDIR /var/www/html/site

COPY site.ini /usr/local/etc/php/conf.d

ENV TZ=Europe/Moscow
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin/ --filename=composer

EXPOSE 9000
