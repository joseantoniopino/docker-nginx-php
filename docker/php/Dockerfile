# Get php image from docker hub
FROM php:7.4.6-fpm

# Get UID and expose it
ARG UID
EXPOSE $UID

# Create user appuser for container handle
RUN adduser -u ${UID} --disabled-password --gecos "" appuser

# Create directory for private keys storage
RUN mkdir /home/appuser/.ssh

# Define user:group for /home/appuser/ directory
RUN chown -R appuser:appuser /home/appuser/

# Say no when ask if want download config cert of private repository
RUN echo "StrictHostKeyChecking no" >> /home/appuser/.ssh/config

# Configure width of console bash
RUN echo "export COLUMNS=300" >> /home/appuser/.bashrc

# Added alias for bin/console. Uncomment for symfony projects
# RUN echo "alias sf=/appdata/www/bin/console" >> /home/appuser/.bashrc

# Copy php configuration inside of container
COPY ./php.ini /usr/local/etc/php/php.ini

# Install some packages and dependencies for php
RUN apt-get update \
    && apt-get install -y git acl openssl openssh-client wget zip vim libssh-dev \
    && apt-get install -y libpng-dev zlib1g-dev libzip-dev libxml2-dev libicu-dev \
    && docker-php-ext-install intl pdo pdo_mysql zip gd soap bcmath sockets \
    && pecl install xdebug \
    && docker-php-ext-enable --ini-name 05-opcache.ini opcache xdebug

# Install and update composer inside of container
RUN curl https://getcomposer.org/composer.phar -o /usr/bin/composer && chmod +x /usr/bin/composer
RUN composer self-update

# Code style for symfony. Uncomment for symfony projects
# RUN wget https://cs.symfony.com/download/php-cs-fixer-v2.phar -O php-cs-fixer
# RUN chmod a+x php-cs-fixer
# RUN mv php-cs-fixer /usr/local/bin/php-cs-fixer

RUN mkdir -p /appdata/www

COPY ./xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

USER appuser

# When enter in container, prompt appear in /appdata/www
WORKDIR /appdata/www
