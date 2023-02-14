FROM php:7.4.21-fpm-buster

RUN mkdir /code
WORKDIR /code

ENV ACCEPT_EULA=Y

RUN apt-get update
# Install requirements
RUN apt-get install -y gnupg \
    libcurl4-openssl-dev \
    mariadb-client \
    libonig-dev \
    zlib1g-dev \
    libzip-dev \
    libpng-dev \
    imagemagick \
    libpq-dev \
    cron \
    libxml2-dev\
    nano
# Install PHP packages
RUN docker-php-ext-install gd \
    mbstring \
    zip \
    pgsql \
    mysqli \
    pdo \
    pdo_mysql \
    soap\
    calendar 
#Install SQL driver for PHP
RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
RUN curl https://packages.microsoft.com/config/debian/11/prod.list > /etc/apt/sources.list.d/mssql-release.list
RUN apt-get update
RUN apt-get install -y --no-install-recommends locales apt-transport-https
RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen
RUN locale-gen
RUN apt-get update

RUN apt-get -y --no-install-recommends install unixodbc-dev  msodbcsql17
RUN apt-get -y --no-install-recommends install libxml2-dev

RUN pecl install sqlsrv pdo_sqlsrv xdebug
RUN docker-php-ext-enable sqlsrv pdo_sqlsrv xdebug

# Install the package for the composer
RUN curl -sS https://getcomposer.org/installer -o ~/composer-setup.php
RUN php ~/composer-setup.php --install-dir=/usr/local/bin --filename=composer

# Adding inicial script
ADD run.sh /code
RUN chmod +x run.sh

WORKDIR /var/www/html

CMD  /bin/bash -c '/code/run.sh' && /bin/bash