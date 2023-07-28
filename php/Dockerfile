FROM php:8.1-fpm

# Download script to install PHP extensions and dependencies
ADD https://raw.githubusercontent.com/mlocati/docker-php-extension-installer/master/install-php-extensions /usr/local/bin/

# PHP extensions https://github.com/mlocati/docker-php-extension-installer
RUN chmod uga+x /usr/local/bin/install-php-extensions && sync && \
    install-php-extensions gd imagick intl mysqli pdo_mysql xsl zip exif pdo_pgsql pgsql


# Clean up
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm /var/log/lastlog /var/log/faillog

# Configure non-root user.
RUN groupmod -o -g 1000 www-data && \
    usermod -o -u 1000 -g www-data www-data

# Install composer
RUN cd /var/tmp/ && curl -s http://getcomposer.org/installer | php && mv ./composer.phar /usr/local/bin/composer

WORKDIR /var/www

USER www-data
