FROM php:7.0-cli

WORKDIR /var/www/html

RUN sed -i 's/deb.debian.org\/debian/archive.debian.org\/debian/g' /etc/apt/sources.list \
    && sed -i 's/security.debian.org\/debian-security/archive.debian.org\/debian-security/g' /etc/apt/sources.list \
    && sed -i '/stretch-updates/d' /etc/apt/sources.list \
    && echo 'Acquire::Check-Valid-Until "false";' > /etc/apt/apt.conf.d/99no-check-valid-until \
    && apt-get update

RUN apt-get install -y --allow-unauthenticated \
        acl \
        apt-transport-https \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        libicu-dev \
        zlib1g-dev \
        git \
        sudo \
        unzip

RUN docker-php-ext-install -j$(nproc) pdo pdo_mysql iconv mcrypt intl zip
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/
RUN docker-php-ext-install -j$(nproc) gd

RUN apt-get autoremove -y \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN pecl channel-update pecl.php.net \
    && pecl install xdebug-2.7.2 \
    && pecl clear-cache \
    && echo "zend_extension=xdebug.so" > /usr/local/etc/php/conf.d/xdebug.ini

RUN echo -e "\nmemory_limit = -1" >> /usr/local/etc/php/conf.d/php.ini
RUN echo -e "\ndate.timezone = Europe/Warsaw" >> /usr/local/etc/php/conf.d/php.ini
RUN echo -e "\nshort_open_tag = off" >> /usr/local/etc/php/conf.d/php.ini
RUN echo "\nxdebug.remote_enable=on" >> /usr/local/etc/php/conf.d/xdebug.ini
RUN echo "\nxdebug.remote_autostart=off" >> /usr/local/etc/php/conf.d/xdebug.ini

RUN echo "export TERM=xterm" >> ~/.bashrc

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php \
    && php -r "unlink('composer-setup.php');" \
    && mv composer.phar /usr/local/bin/composer

COPY entrypoint.sh /usr/local/bin/entrypoint.sh

CMD ["/bin/bash", "/usr/local/bin/entrypoint.sh"]
