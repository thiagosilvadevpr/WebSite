FROM php:7.4-fpm

ARG user
ARG uid

RUN apt-get update && apt-get install -y \
	libfreetype6-dev \
	libjpeg62-turbo-dev \
	libpng-dev \
	libonig-dev \
	libxml2-dev \
	curl \
	zip \
	unzip \
	nano \
  && docker-php-ext-configure gd --with-freetype --with-jpeg \
  && docker-php-ext-install -j$(nproc) gd

RUN apt-get clean && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-install mysqli pdo_mysql mbstring exif pcntl bcmath

RUN pecl install redis \
    && docker-php-ext-enable redis

RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

RUN pecl install igbinary-3.1.2 \
    apk add --no-cache --virtual .build-deps \
    g++ make autoconf yaml-dev

RUN rm -rf /var/cache/apk/*

COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

RUN ln -snf /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime \
  && echo America/Sao_Paulo > /etc/timezone

RUN useradd -G www-data,root -u $uid -d /home/$user $user
RUN mkdir -p /home/$user/.composer && \
    chown -R $user:$user /home/$user

#COPY ./config/php.ini /usr/local/etc/php/php.ini
#COPY ./config/www.conf /usr/local/etc/php-fpm.d/www.conf

WORKDIR /var/www

EXPOSE 9000

CMD [ "php-fpm" ]
