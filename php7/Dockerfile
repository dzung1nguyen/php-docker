FROM php:7.4-fpm

# Configure non-root user.
RUN groupmod -o -g 1000 www-data && \
    usermod -o -u 1000 -g www-data www-data

RUN mkdir -p /home/www-data && chown -R www-data:www-data /home/www-data

RUN set -xe; \
    apt-get update -yqq && \
    pecl channel-update pecl.php.net && \
    apt-get install -yqq \
    apt-utils \
    git \
    ca-certificates\
    curl \
    libzip-dev \
    zip \
    unzip

# Download script to install PHP extensions and dependencies
ADD https://raw.githubusercontent.com/mlocati/docker-php-extension-installer/master/install-php-extensions /usr/local/bin/

# PHP extensions https://github.com/mlocati/docker-php-extension-installer
RUN chmod uga+x /usr/local/bin/install-php-extensions && sync && \
    install-php-extensions gd imagick intl mysqli pdo_mysql xsl zip exif pdo_pgsql pgsql gmp bcmath

# Clean up
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm /var/log/lastlog /var/log/faillog

# Install composer
RUN cd /var/tmp/ && curl -s http://getcomposer.org/installer | php && mv ./composer.phar /usr/local/bin/composer

WORKDIR /var/www

USER www-data

EXPOSE 9007
