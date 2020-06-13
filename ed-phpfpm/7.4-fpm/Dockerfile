FROM php:7.4-fpm

RUN apt-get update \
    && echo "postfix postfix/mailname string example.com" | debconf-set-selections \
    && echo "postfix postfix/main_mailer_type string 'Internet Site'" | debconf-set-selections \
    && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libz-dev \
        less \
        postfix \
        mariadb-client \
        libzip-dev \
    && docker-php-ext-install -j$(nproc) \
        mysqli \
        pdo_mysql \
        sockets \
        zip \
    && docker-php-ext-configure gd --with-jpeg=/usr/include \
                                   --with-freetype=/usr/include \
    && docker-php-ext-install -j$(nproc) gd \
    && pecl install xdebug-2.9.4 \
    && apt-get autoremove -y \
    && apt-get clean \
    && curl https://getcomposer.org/download/$(curl -LSs https://api.github.com/repos/composer/composer/releases/latest | grep 'tag_name' | sed -e 's/.*: "//;s/".*//')/composer.phar > composer.phar \
    && chmod +x composer.phar \
    && mv composer.phar /usr/local/bin/composer \
    && curl https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar > wp-cli.phar \
    && chmod +x wp-cli.phar \
    && mv wp-cli.phar /usr/local/bin/wp \
    && curl -L https://phar.phpunit.de/phpunit-8.phar > /tmp/phpunit.phar \
	&& chmod +x /tmp/phpunit.phar \
	&& mv /tmp/phpunit.phar /usr/local/bin/phpunit \
    && apt-get update && apt-get install -y \
		git \
		subversion \
		wget \
		libxml2-dev \
		msmtp \
		imagemagick \
		libmagickwand-dev \
		unzip \
		iproute2 \
		libicu-dev \
	&& pecl install imagick \
	&& docker-php-ext-enable imagick \
	&& docker-php-ext-install soap intl \
	&& apt-get remove -y libmagickwand-dev libxml2-dev libicu-dev \
	&& apt-get autoremove -y \
	&& apt-get clean \
	&& mkdir /var/www/.wp-cli \
	&& chown www-data:www-data /var/www/.wp-cli \
    && XDEBUG=$(find /usr/local/lib/php -name 'xdebug.so' | head -n 1 | tail -n 1) \
	&& ln -s $XDEBUG /usr/local/lib/php/extensions/xdebug.so

CMD ["php-fpm"]

EXPOSE 9000
