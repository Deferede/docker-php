# Для начала указываем исходный образ, он будет использован как основа
FROM php:7.2-fpm

# RUN выполняет идущую за ней команду в контексте нашего образа.
# В данном случае мы установим некоторые зависимости и модули PHP.
# Для установки модулей используем команду docker-php-ext-install.
# На каждый RUN создается новый слой в образе, поэтому рекомендуется объединять команды.
RUN apt-get update && apt-get install -y \
        curl \
        wget \
        git \
        nano \
        libfreetype6-dev \
        libwebp-dev \
        libjpeg62-turbo-dev \
	libpng-dev \
	libonig-dev \
	libzip-dev \
        libxpm-dev \
	libmcrypt-dev \
        libmemcached-dev \
        libssh-dev \
	librabbitmq-dev

RUN pecl install mcrypt-1.0.3 && docker-php-ext-enable mcrypt
RUN pecl install amqp && docker-php-ext-enable amqp
RUN docker-php-ext-install -j$(nproc) iconv mbstring mysqli pdo_mysql zip sockets sysvsem bcmath
RUN pecl install memcached && docker-php-ext-enable memcached
RUN pecl install memcache-4.0.5.2 && docker-php-ext-enable memcache
RUN pecl install xdebug-2.6.1 && docker-php-ext-enable xdebug
RUN pecl install mongodb && docker-php-ext-enable mongodb

RUN docker-php-ext-configure gd \
    --with-gd \
    --with-webp-dir \
    --with-jpeg-dir \
    --with-png-dir \
    --with-zlib-dir \
    --with-xpm-dir \
    --with-freetype-dir

RUN docker-php-ext-install gd

# Куда же без composer'а.
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Добавим свой php.ini, можем в нем определять свои значения конфига
ADD php.ini /usr/local/etc/php/php.ini

# Указываем рабочую директорию для PHP
WORKDIR /var/www

# Запускаем контейнер
# Из документации: The main purpose of a CMD is to provide defaults for an executing container. These defaults can include an executable, 
# or they can omit the executable, in which case you must specify an ENTRYPOINT instruction as well.
CMD ["php-fpm"]
