FROM php:8.0.5-fpm

ENV TZ="America/Sao_Paulo"

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN apt-get update && apt-get install -y \
	cron \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd

#install some base extensions
RUN apt-get install -y \
        libzip-dev \
        zip \
  && docker-php-ext-install zip

RUN docker-php-ext-configure zip
RUN docker-php-ext-install zip

RUN docker-php-ext-install bcmath  
RUN docker-php-ext-install ctype  
RUN docker-php-ext-install pcntl
RUN docker-php-ext-install opcache

RUN pecl install redis && docker-php-ext-enable redis

# Install Postgre PDO
RUN apt-get install -y libpq-dev \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo pdo_pgsql pgsql

# Install Mysql PDO
RUN docker-php-ext-install pdo_mysql 
RUN docker-php-ext-install mysqli

RUN ["apt-get", "install", "-y", "vim"]
RUN ["apt-get" ,"install", "-y" ,"git-all"]

ARG packagist_user_token
RUN composer config --global --auth http-basic.repo.packagist.com  ${packagist_user_token}

WORKDIR /usr/share/nginx/html/

RUN apt-get install -y supervisor
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
CMD /usr/bin/supervisord;
