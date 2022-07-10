FROM php:7.4.24-apache-buster

ENV TIMEZONE            Europe/Moscow
ENV TZ                  $TIMEZONE
ENV PHP_MEMORY_LIMIT    1024M
ENV UPLOAD_MAX_FILESIZE 20M
ENV PHP_MAX_POST        20M
ENV ERROR_REPORTING     0
ENV DISPLAY_ERRORS      off

ENV SUPERCRONIC_URL=https://github.com/aptible/supercronic/releases/download/v0.1.12/supercronic-linux-amd64 \
    SUPERCRONIC=supercronic-linux-amd64 \
    SUPERCRONIC_SHA1SUM=048b95b48b708983effb2e5c935a1ef8483d9e3e

RUN apt-get update && apt-get install -y tzdata && \
    rm /etc/localtime && \
    ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && \
    dpkg-reconfigure -f noninteractive tzdata

RUN apt-get install -y locales wget nano curl libpq-dev libzip-dev zip unzip git libonig-dev libxml2-dev libcurl4-openssl-dev libpng-dev libc-client-dev libkrb5-dev libldap2-dev libedit-dev build-essential libtool libxslt-dev libmcrypt-dev\
        && CFLAGS="-I/usr/src/php"
RUN docker-php-ext-configure imap --with-kerberos --with-imap-ssl && docker-php-ext-install imap
#RUN docker-php-ext-configure ldap --with-kerberos --with-imap-ssl
#RUN docker-php-ext-install -j$(nproc)  imap ldap intl mysqlnd phar posix readline shmop simplexml sockets sysvmsg sysvsem sysvshm tokenizer xmlreader xmlwriter xsl mbstring

RUN echo 'ru_RU.UTF-8 UTF-8' >> /etc/locale.gen && \
    locale-gen
ENV LANG ru_RU.UTF-8
ENV LANGUAGE ru_RU:en
ENV LC_ALL ru_RU.UTF-8

RUN pecl channel-update pecl.php.net
RUN pecl install -o -f redis \
    &&  rm -rf /tmp/pear \
    &&  docker-php-ext-enable redis

RUN docker-php-ext-install curl \
    && docker-php-ext-install opcache \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install zip \
    && docker-php-ext-install pdo \
    && docker-php-ext-install xml \
    && docker-php-ext-install gd \
    && docker-php-ext-install iconv \
    && docker-php-ext-install calendar \
    && docker-php-ext-install ctype \
    && docker-php-ext-install dom \
    && docker-php-ext-install exif \
    && docker-php-ext-install fileinfo \
    && docker-php-ext-install ftp \
    && docker-php-ext-install gettext \
    && docker-php-ext-install json \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install phar \
    && docker-php-ext-install posix \
    && docker-php-ext-install shmop \
    && docker-php-ext-install simplexml \
    && docker-php-ext-install sockets \
    && docker-php-ext-install sysvsem \
    && docker-php-ext-install sysvmsg \
    && docker-php-ext-install sysvshm \
    && docker-php-ext-install tokenizer \
    && docker-php-ext-install xmlwriter \
    && docker-php-ext-install xsl \
    && docker-php-ext-install mbstring \
    && docker-php-ext-install -j$(nproc) ldap

RUN curl -fsSLO "$SUPERCRONIC_URL" \
 && echo "${SUPERCRONIC_SHA1SUM}  ${SUPERCRONIC}" | sha1sum -c - \
 && chmod +x "$SUPERCRONIC" \
 && mv "$SUPERCRONIC" "/usr/local/bin/${SUPERCRONIC}" \
 && ln -s "/usr/local/bin/${SUPERCRONIC}" /usr/local/bin/supercronic

RUN curl -sS https://getcomposer.org/installer -o composer-setup.php
RUN php composer-setup.php --install-dir=/usr/local/bin --filename=composer

#RUN cd /var/www/html && composer install --ignore-platform-reqs

EXPOSE 80

WORKDIR /var/www/html
