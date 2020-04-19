FROM php:7.2-fpm-alpine

ENV COMPOSER_ALLOW_SUPERUSER 1

RUN apk update \
    && apk add  --no-cache \
        git \
        mysql-client \
        curl \
        openssh-client \
        icu \
        libpng \
        libjpeg-turbo \
    && apk add --no-cache --virtual \
        build-dependencies \
        icu-dev \
        postgresql-dev \
        libxml2-dev \
        freetype-dev \
        libpng-dev \
        libjpeg-turbo-dev \
        g++ \
        make \
        autoconf \
    && docker-php-source extract \
    && pecl install redis \
    && docker-php-ext-enable redis \
    && docker-php-source delete \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install \
        pdo_pgsql \
        mysqli \
        pgsql \
        pdo_mysql \
        intl \
        zip \
        gd \
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && curl https://getcomposer.org/download/$(curl -LSs https://api.github.com/repos/composer/composer/releases/latest | grep 'tag_name' | sed -e 's/.*: "//;s/".*//')/composer.phar > composer.phar \
    && chmod +x composer.phar \
    && mv composer.phar /usr/local/bin/composer \
    && curl https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar > wp-cli.phar \
    && chmod +x wp-cli.phar \
    && mv wp-cli.phar /usr/local/bin/wp \
    && mkdir /src && cd /src && git clone https://github.com/xdebug/xdebug.git \
    && cd xdebug \
    && sh ./rebuild.sh \
    && cd  / && rm -fr /src \
    && apk del build-dependencies \
    && rm -rf /tmp/*

USER www-data

WORKDIR /var/www
CMD ["php-fpm"]
