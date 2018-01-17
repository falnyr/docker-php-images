FROM php:7.1-apache

# base
RUN apt-get update \
  && apt-get install -y libcurl4-openssl-dev sudo git libxslt-dev zlib1g-dev graphviz zip libmcrypt-dev libicu-dev g++ libpcre3-dev libgd-dev libfreetype6-dev sqlite \
  && apt-get clean \
  && docker-php-ext-install soap \
  && docker-php-ext-install zip \
  && docker-php-ext-install xsl \
  && docker-php-ext-install mcrypt \
  && docker-php-ext-install mbstring \
  && docker-php-ext-install gettext \
  && docker-php-ext-install curl \
  && docker-php-ext-install json \
  && docker-php-ext-install intl \
  && docker-php-ext-install opcache \
  && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-png-dir=/usr/include/ \
  && docker-php-ext-install gd

# postgres
RUN apt-get update && \
    apt-get install -y libpq-dev && \
    docker-php-ext-install pdo pdo_pgsql

# blackfire
RUN version=$(php -r "echo PHP_MAJOR_VERSION.PHP_MINOR_VERSION;") \
    && curl -A "Docker" -o /tmp/blackfire-probe.tar.gz -D - -L -s https://blackfire.io/api/v1/releases/probe/php/linux/amd64/$version \
    && tar zxpf /tmp/blackfire-probe.tar.gz -C /tmp \
    && mv /tmp/blackfire-*.so $(php -r "echo ini_get('extension_dir');")/blackfire.so \
    && printf "extension=blackfire.so\nblackfire.agent_socket=tcp://blackfire:8707\n" > $PHP_INI_DIR/conf.d/blackfire.ini

# composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# apache mod_rewrite
RUN a2enmod rewrite
RUN usermod -u 1000 www-data