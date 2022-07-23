FROM php:7.2.1-fpm

RUN apt-get update

# Install tools
RUN apt-get install -y \
    python-setuptools \
    software-properties-common \
    wget \
    git \
    curl \
    zip \
    vim

RUN echo 'deb http://httpredir.debian.org/debian jessie contrib' >> /etc/apt/sources.list

RUN apt-get update
RUN export DEBIAN_FRONTEND=noninteractive && apt-get install -y --force-yes libssl-dev curl  libcurl4-gnutls-dev libxml2-dev libicu-dev libmcrypt4 libmemcached11 openssl

#CONFIGURAÇÕES DO OPCACHE
RUN docker-php-ext-install opcache

#CONFIGURAÇÕES DO APCU
RUN pecl install apcu-5.1.5 && docker-php-ext-enable apcu

#LIBS EXTRAS
RUN docker-php-ext-install bcmath
RUN apt-get install -y libbz2-dev
RUN docker-php-ext-install bz2
RUN docker-php-ext-install mbstring
RUN apt-get install -y libpq-dev
RUN apt-get install -y libicu-dev
RUN docker-php-ext-install intl

#GD
RUN apt-get install -y libfreetype6-dev libjpeg62-turbo-dev libpng16-16
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/
RUN docker-php-ext-install gd

#PDO
RUN docker-php-ext-install pdo_mysql

#sudo
RUN apt-get update && apt-get install -y sudo && rm -rf /var/lib/apt/lists/*    

VOLUME /var/www/html
WORKDIR /var/www/html
RUN pwd

# Allow Composer to be run as root
ENV COMPOSER_ALLOW_SUPERUSER 1

# Install composer
RUN wget https://getcomposer.org/download/1.5.5/composer.phar
RUN chmod +x composer.phar
RUN sudo mv composer.phar /usr/local/bin/composer

# Install libs/var_dumper
RUN mkdir -p /var/.composer
RUN composer global --no-interaction --working-dir=/var/.composer require slim/slim

EXPOSE 9000

CMD ["php-fpm"]
