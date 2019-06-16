FROM php:7.2-fpm

MAINTAINER smallnews <1371606921@qq.com>

COPY ./sources.list.stretch /etc/apt/sources.list

# add apt-get frontend settings
ENV DEBIAN_FRONTEND noninteractive

# set timezone
RUN /usr/bin/apt-get install tzdata
RUN /bin/cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo 'Asia/Shanghai' > /etc/timezone


# apt-get install some packages
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        cron \
        vim \
        curl \
        wget \
        git \
        zip \
        libz-dev \
        libnghttp2-dev \
    && apt-get install --assume-yes apt-utils \
    && apt-get clean \
    && apt-get autoremove


# docker-php-ext-install install php extension
RUN apt-get update \
    && apt-get install -y libfreetype6-dev libjpeg62-turbo-dev libpng-dev \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && :\
    && apt-get install -y libicu-dev \
    && docker-php-ext-install intl \
    && :\
    && apt-get install -y libxml2-dev \
    && apt-get install -y libxslt-dev \
    && docker-php-ext-install soap \
    && docker-php-ext-install xsl \
    && docker-php-ext-install xmlrpc \
    && docker-php-ext-install wddx \
    && :\
    && apt-get install -y libbz2-dev \
    && docker-php-ext-install bz2 \
    && :\
    && docker-php-ext-install zip \
    && docker-php-ext-install pcntl \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install exif \
    && docker-php-ext-install bcmath \
    && docker-php-ext-install calendar \
    && docker-php-ext-install sockets \
    && docker-php-ext-install gettext \
    && docker-php-ext-install shmop \
    && docker-php-ext-install sysvmsg \
    && docker-php-ext-install sysvsem \
    && docker-php-ext-install sysvshm \
    && docker-php-ext-install opcache \
    && :\
    && apt-get install -y libmcrypt-dev \
    && pecl install mcrypt-1.0.1 \
    && docker-php-ext-enable mcrypt

# install composer
RUN curl -sS https://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer \
    && composer self-update --clean-backups \
    && composer config -g repo.packagist composer https://packagist.phpcomposer.com

COPY ./docker-entrypoint.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/docker-entrypoint.sh

ENTRYPOINT ["docker-entrypoint.sh"]

CMD ["phpfpm"]
