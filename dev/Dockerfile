FROM php:8.0-fpm
LABEL maintainer="mdelle@dynamo-media.com"

COPY --from=composer:2.3 /usr/bin/composer /usr/bin/composer

RUN export CFLAGS="$PHP_CFLAGS" CPPFLAGS="$PHP_CPPFLAGS" LDFLAGS="$PHP_LDFLAGS"; \
    apt-get update -qq \
        && apt-get install -qqy --no-install-recommends \
            build-essential \
            git \
            libicu-dev \
            libzip-dev \
            libxml2-dev \
            unzip \
            zip \
            libmagickwand-dev \
            libfreetype6-dev \
            libjpeg62-turbo-dev \
            libpng-dev \
            pkg-config \
            libmcrypt-dev \
            libmemcached-dev \
            zlib1g-dev \
        && pecl install imagick-3.7.0 \
        && pecl install mcrypt-1.0.5 \
        && pecl install memcached-3.2.0 \
        && pecl install redis-5.3.7 \
        && pecl install xdebug-3.1.4 \
        && docker-php-ext-configure intl \
        && docker-php-ext-configure gd \
            --with-gd \
            --with-freetype-dir=/usr/include/ \
            --with-png-dir=/usr/include/ \
            --with-jpeg-dir=/usr/include/ \
        && docker-php-ext-configure opcache \
        && docker-php-ext-configure zip \
            --with-libzip \
        && docker-php-ext-install -j"$(nproc)" \
            gd \
            intl \
            opcache \
            pdo_mysql \
            soap \
            zip \
            exif \
        && docker-php-ext-enable \
            imagick \
            mcrypt \
            memcached \
            redis \
            xdebug \
            pdo_mysql \
            gd \
            zip \
            exif; \
    apt-get clean \
        && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /var/cache/apt/*; \
    mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"

RUN docker-php-ext-install gd
RUN docker-php-ext-install zip
RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install exif
RUN docker-php-ext-enable gd
RUN docker-php-ext-enable zip
RUN docker-php-ext-enable pdo_mysql
RUN docker-php-ext-enable exif

COPY ./php.ini /usr/local/etc/php/conf.d/php-ini-overrides.ini