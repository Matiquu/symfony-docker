FROM php:7.4-fpm-alpine

RUN apk update && apk add $PHPIZE_DEPS \
    vim nano curl git acl wget zip unzip openssl-dev \
    gnupg g++ libxml2-dev libressl-dev rabbitmq-c-dev libmcrypt-dev procps libzip-dev \
    icu-dev oniguruma-dev jpeg-dev libpng-dev imagemagick-dev \
    nodejs yarn

RUN docker-php-ext-configure intl && \
    docker-php-ext-configure gd --with-jpeg && \
    docker-php-ext-install -j$(nproc) pdo pdo_mysql mbstring intl zip soap gd opcache bcmath && \
    pecl install amqp imagick apcu && \
    docker-php-ext-enable amqp imagick apcu

RUN pecl install xdebug && \
    echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini && \
    echo "xdebug.remote_enable=on" >> /usr/local/etc/php/conf.d/xdebug.ini && \
    echo "xdebug.remote_autostart=off" >> /usr/local/etc/php/conf.d/xdebug.ini

COPY php.ini /usr/local/etc/php/php.ini

RUN curl -sSk https://getcomposer.org/installer | php -- --disable-tls && \
	mv composer.phar /usr/local/bin/composer

RUN wget https://phar.phpunit.de/phpunit-9.phar -O phpunit.phar && \
    chmod +x phpunit.phar && \
    mv phpunit.phar /usr/local/bin/phpunit

WORKDIR /home/wwwroot/

EXPOSE 9000
CMD ["php-fpm"]
