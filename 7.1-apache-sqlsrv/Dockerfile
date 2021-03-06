FROM php:7.1-apache

# pre-install
RUN apt-get update && apt-get install -y \
    zlib1g-dev \
    libicu-dev \
    g++ \
    libssl-dev \
    apt-transport-https

# microsoft repository
RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - \
    && curl https://packages.microsoft.com/config/debian/8/prod.list > /etc/apt/sources.list.d/mssql-release.list \
    && apt-get update

# base
RUN ACCEPT_EULA=Y apt-get install -y \
  libcurl4-openssl-dev sudo git libxslt-dev graphviz zip libmcrypt-dev libpcre3-dev libgd-dev libfreetype6-dev sqlite \
  unixodbc unixodbc-dev odbcinst msodbcsql locales \
  && echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && locale-gen

RUN apt-get clean \
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

# sqlsrv
RUN pecl install pdo_sqlsrv && \
    echo -e "extension=pdo_sqlsrv.so" > /usr/local/etc/php/conf.d/ext-pdosqlsrv.ini && \
    docker-php-ext-enable pdo_sqlsrv

# composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# apache mod_rewrite
RUN a2enmod rewrite
RUN usermod -u 1000 www-data