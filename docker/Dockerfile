FROM ubuntu:20.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update -yq && apt-get upgrade -yq

RUN apt install -yq software-properties-common

RUN add-apt-repository ppa:ondrej/php

RUN apt-get update && apt-get install -yq --no-install-recommends \
    apt-utils \
    curl \
    # Instalar git
    git \
    # Instalar apache
    apache2 \
    # Instalar php 8.0
    php8.0 \
    php8.0-common \
    libapache2-mod-php8.0 \
    php8.0-cli \
    php8.0-curl \
    php8.0-fpm \
    php8.0-gd \
    php8.0-ldap \
    php8.0-mbstring \
    php8.0-mysql \
    php8.0-soap \
    php8.0-sqlite3 \
    php8.0-xml \
    php8.0-zip \
    php8.0-intl \
    php8.0-xdebug \
    php8.0-imagick \
    # Instalar tools
    tree \
    openssl \
    nano \
    ca-certificates \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

RUN usermod --non-unique --uid 1000 www-data && groupmod --non-unique --gid 1000 www-data

# Instalar composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Configurar PHP
COPY ./php/app.ini /etc/php/8.0/mods-available/
RUN phpenmod app
# Configurar apache
RUN a2enmod rewrite expires
RUN echo "ServerName localhost" | tee /etc/apache2/conf-available/servername.conf
RUN a2enconf servername
# Configurar vhost
COPY ./apache/app.conf /etc/apache2/sites-available/
RUN a2dissite 000-default
RUN a2ensite app.conf

EXPOSE 80 443

WORKDIR /var/www/html

RUN rm index.html

HEALTHCHECK --interval=5s --timeout=3s --retries=3 CMD curl -f http://localhost || exit 1

CMD apachectl -D FOREGROUND
